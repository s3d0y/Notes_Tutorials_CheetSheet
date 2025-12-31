
отправка файлов через [[SCP - отправка и вытягивание файлов]]

### Задание 1. Создание скрипта сбора информации о дистрибутиве

тут более менее все очевидно. пишется с ИИ и обкатвается на тестовой машине

``` sh

#!/bin/bash

# 1. Проверка на root
if [ "$EUID" -ne 0 ]; then
	echo "Need root"
	echo "$EUID"
	exit 1
fi

# 2. Обновление пакетов 
echo "Updating package list..."
apt-get update -qq || { echo "Failed to update"; exit 1; }

echo "Upgrading installed packages..."
apt-get upgrade -y -qq || { echo "Failed to upgrade"; exit 1; }

# 3. Установка cowsay и sl

install_if_missing() {
	local package=$1
	if dpkg -s "$package" &> /dev/null; then
		echo "$package already installed"
	else
		echo "installing $package"
		apt-get install -y -qq "$package"
	fi
}

install_if_missing cowsay
install_if_missing sl

# 4. Создание файла info
# 4.1. Запись в info: версия ОС
echo -e "\n===OS===" > info
cat /etc/os-release | grep "PRETTY_NAME" >> info

# 4.2. Запись в info: версия ядра
echo -e "\n===Kernel===" >> info
uname -r >> info

# 4.3. Запись в info: установленные пакеты
echo -e "\n=== Installed Packages ===" >> info
dpkg -l >> info

# 4.4. Запись в info: запущенные процессы
echo -e "\n===Process===" >> info
ps aux >> info

# 4.5. Запись в info: открытые порты
echo -e "\n===Open ports===" >> info
ss -tunl >> info

# 5. Архивация info в OS_RESULT.tar
tar -cf OS_RESULT.tar info

# 6. (Опционально) Удаление info
rm info
/usr/games/cowsay "System info collected!"  >&2


```
### Задание 2. Создание скрипта настройки Linux

это не причесанный вариант 

``` sh 
#!/bin/bash

# 1. Проверка на root
if [ "$EUID" -ne 0 ]; then
	echo "Need root"
	echo "$EUID"
	exit 1
fi

# Create group ad user

groupadd default_users
groupadd secret_users

useradd -m -g default_users -s /bin/bash user

# i want add user by circle
for user in secret_agent secret_spy secret_boss ; do
	useradd -m -g secret_users -s /bin/bash "$user"
	chmod 750 /home/"$user"

done	

#list Current user
echo -e "\n===Current users ==="
getent passwd | awk -F: '$3 >= 1000 && $3 < 65534 {print $1}'
echo -e "\n===Current groups ==="
getent group | awk -F: '$3 >= 1000 && $3 < 65534 {print $1}'

ls -l /home

chmod 755 /var
echo -e "\n===Var_access==="
ls -l / | grep 'var'

echo "Updating package list..."
apt-get update  || { echo "Failed to update"; exit 1; }

echo "Upgrading installed packages..."
apt-get upgrade -y || { echo "Failed to upgrade"; exit 1; }


install_if_missing() {
	local package=$1
	if dpkg -s "$package" &> /dev/null; then
		echo "$package already installed"
	else
		echo "installing $package"
		apt-get install -y -qq "$package"
	fi
}

install_if_missing apache2
if systemctl is-active -q apache2; then
	echo 'apach UP'
else
	echo 'apach DOWN'
fi
 
echo '%default_users ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/default_users
chmod 0440 /etc/sudoers.d/default_users


```

### Задание 3. Настройка мониторинга Linux

1. Настройки logwatch

по умолчанию 
`/usr/share/logwatch/` - стандартные настройки (обновляются при апгрейдах)

лучше перенести  (скопировать) в 
`/etc/lowatch/conf/logwatch.conf`

Logwatch сначала читает настройки из `/etc/logwatch/`, затем из `/usr/share/logwatch/`. Таким образом, ваши кастомные настройки имеют приоритет.

основные настройки 

```conf
Output = mail
Format = text
MailTo = mailing_s3d0y@mail.ru
MailFrom = mailing_test@bk.ru
Range = yesterday
Detail = High
```

2. Настройки postfix 
Тут было проблем  настройкой 

в итоге рабочее решение такое:

Файл /etc/postfix/mail.cf

``` cf
myhostname = kali
mydomain = bk.ru
myorigin = $mydomain

inet_interfaces = loopback-only
inet_protocols = ipv4

relayhost = [smtp.mail.ru]:587

smtp_tls_security_level = may
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt

smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl/saslpaswd
smtp_sasl_security_options = noanonymous
smtp_sasl_mechanism_filter = plain, login

sender_canonical_maps = regexp:/etc/postfix/sender_canonical

compatibility_level = 3.6
```

настройка файла /etc/postfix/sasl/sasl_passwd 

``` cf
[smtp.mail.ru]:587    ваш_логин@mail.ru:ваш_пароль
```

про [[SASL]] 

создать .db с помощью postmap
выдат права 600 через chmod

про [[Postmap]]

sudo postmap /etc/postfix/sasl/sasl_passwd

не забывать перезапускать postfix через systemctl
``` sh
sudo systemctl reload postfix
sudo systemctl restart postfix
```

тестируем через 
``` sh

echo "test text" | mail -s "s it's a subject - zagolovok" `почта получателя`
```

3. работа с [[Cron]]
задача -  0 0 * * *  /usr/bin/logwatch

Принудительный выполненение всех  крон задач - помогло протестить

```sh 
sudo run-parts /etc/cron.daily/
```



### Задание 4. Защита операционной системы встроенными инструментами

Делаем ключ на локальной машине (ssh-keyen)  и отправляем его на виртуалку (ssh-copy-id -p 2222 userid@localhost)

на виртуальке дополняем файл /etc/ssh/sshd_config

```
# Разрешаем аутентификацию по ключам PubkeyAuthentication yes 

# ЗАПРЕЩАЕМ аутентификацию по паролю PasswordAuthentication no 

# Запрещаем вход под root (опционально, но рекомендуется) PermitRootLogin no 

# Разрешаем только определенных пользователей (опционально) AllowUsers s3d0y
```

для блокировки 80 порта прописываем в ip tables 
``` sh
iptables -A INPUT -p tcp --dport 80 -j DROP
```