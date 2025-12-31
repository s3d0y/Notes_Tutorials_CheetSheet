### Шаг 1: Установка Fail2Ban

``` sh
# Устанавливаем Fail2Ban
sudo apt update
sudo apt install fail2ban

# Проверяем что служба запустилась
sudo systemctl status fail2ban
```

### Шаг 2: Создаем конфигурационный файл

```
# Создаем локальный конфиг (переопределяет настройки по умолчанию)
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

### Шаг 3: Настройка для порта 3322
``` ini
[DEFAULT]
# Общие настройки
bantime = 3600         # Время бана в секундах (1 час)
findtime = 600         # Окно анализа в секундах (10 минут)  
maxretry = 3           # Количество попыток до бана
backend = auto         # Автоматическое определение бэкенда

[sshd]
# Настройки для SSH защиты
enabled = true         # Включаем защиту SSH
port = 3322            # Указываем нестандартный порт
logpath = /var/log/auth.log  # Путь к логам аутентификации
maxretry = 3           # 3 неудачные попытки = бан
bantime = 3600         # Бан на 1 час
```

### Шаг 4: Применяем настройки

```
# Перезагружаем Fail2Ban
sudo systemctl restart fail2ban

# Проверяем статус
sudo systemctl status fail2ban
sudo fail2ban-client status
```

## Проверка работы Fail2Ban

### Тестирование блокировки

```sh
# Смотрим статус защиты SSH

sudo fail2ban-client status sshd

# Пример вывода:
# Status for the jail: sshd
# |- Filter
# |  |- Currently failed: 0
# |  |- Total failed:     0
# |  `- File list:        /var/log/auth.log
# `- Actions
#    |- Currently banned: 0
#    |- Total banned:     0
#    `- Banned IP list:
```

### Мониторинг логов:

``` sh
# Смотрим логи Fail2Ban
sudo tail -f /var/log/fail2ban.log

# Смотрим логи аутентификации
sudo tail -f /var/log/auth.log | grep -i fail
```

### Ручное тестирование блокировки:

``` sh
# Временно баним тестовый IP
sudo fail2ban-client set sshd banip 192.168.1.100

# Проверяем что IP забанен
sudo fail2ban-client status sshd

# Разбаниваем IP
sudo fail2ban-client set sshd unbanip 192.168.1.100
```


ТАКОЕ НЕ ДЕЛАЛ 

## Расширенные настройки
### Защита от DDoS-атак:

``` sh
[sshd-ddos]
enabled = true
port = 3322
logpath = /var/log/auth.log
maxretry = 5           # Больше попыток для DDoS
findtime = 600         # 10 минут окно
bantime = 3600         # Бан на 1 час
```


### Уведомления по email (опционально):

``` ini
[DEFAULT]
destemail = your-email@example.com
sender = fail2ban@your-server.com
action = %(action_mwl)s  # mail whois log
```

### Настройка для нескольких портов:

``` ini
[sshd]
enabled = true
port = ssh,3322        # Защищаем оба порта
logpath = /var/log/auth.log
```


## Интеграция с UFW

### Проверяем что Fail2Ban работает с UFW:

```sh
# Смотрим правила UFW
sudo ufw status verbose

# После срабатывания Fail2Ban должны появиться правила:
# 192.168.1.100 DENY IN    Anywhere
```

### Ручное добавление правил UFW:

```  sh
# Разрешаем SSH на нестандартном порте
sudo ufw allow 3322/tcp comment 'SSH custom port'

# Проверяем
sudo ufw status numbered
```