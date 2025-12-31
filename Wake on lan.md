ЧЕРНОВИК требует проверки - НАСТРОЙКУ МИКРОТИКА ОТДЕЛЬНО

-  Настроить в биосе сервера

**Wake-on-LAN (WoL)** - это технология, позволяющая включить компьютер удаленно по сети с помощью специального [[Волшебный пакет ( "magic packet")]]


Можно проверить на прямую с микротика 

```
[admin@MikroTik] > /tool wol mac=FF:FF:FF:FF:FF:FF interface=bridge-l
ocal
```

## Предварительная настройка
### Шаг 1: Проверяем поддержку WoL

``` sh
# Проверяем текущий статус WoL
sudo ethtool enp2s0 | grep -i wake

# Пример вывода:
# Supports Wake-on: pumbg
# Wake-on: d
```

**Расшифровка:**
- `Supports Wake-on: pumbg` - карта поддерживает WoL
- `Wake-on: d` - WoL отключен (`g` - включен)

### Шаг 2: Включаем WoL вручную

``` sh
# Включаем Wake-on-LAN
sudo ethtool -s enp2s0 wol g

# Проверяем что включилось
sudo ethtool enp2s0 | grep -i wake
# Должно быть: Wake-on: g
```

### Шаг 3: Проверяем необходимость службы

**Проблема:** После перезагрузки настройки WoL сбрасываются!

``` sh
# Включаем WoL
sudo ethtool -s enp2s0 wol g

# Перезагружаем сервер
sudo reboot

# После перезагрузки проверяем:
sudo ethtool enp2s0 | grep -i wake
```

**Если видите `Wake-on: d`** - нужна служба!


## Создаем systemd службу для WoL

### Шаг 1: Создаем файл службы

``` sh
sudo nano /etc/systemd/system/wol.service
```

### Шаг 2: Содержимое службы с комментариями

```sh
[Unit]
Description=Enable Wake-on-LAN
# Запускаем после загрузки сети
After=network.target
Wants=network.target

[Service]
Type=oneshot
# Включаем WoL для сетевого интерфейса
ExecStart=/sbin/ethtool -s enp2s0 wol g
# Служба считается активной после выполнения
RemainAfterExit=yes
User=root

[Install]
# Запускаем при загрузке системы
WantedBy=multi-user.target
```

### Шаг 3: Настраиваем службу

``` sh
# Даем правильные права файлу
sudo chmod 644 /etc/systemd/system/wol.service

# Перезагружаем systemd демон
sudo systemctl daemon-reload

# Включаем автозагрузку службы
sudo systemctl enable wol.service

# Запускаем службу (применит настройки сразу)
sudo systemctl start wol.service

# Проверяем статус
sudo systemctl status wol.service
```

### Шаг 4: Проверяем результат
```sh
# Проверяем что WoL включен
sudo ethtool enp2s0 | grep -i wake
# Должно быть: Wake-on: g

# Перезагружаем для окончательной проверки
sudo reboot

# После перезагрузки проверяем снова
sudo ethtool enp2s0 | grep -i wake
# Теперь должно остаться: Wake-on: g
```

### Использование 

``` sh
# Отправка в локальной сети
wakeonlan FF:FF:FF:FF:FF:FF

# или Указываем конкретный IP (для маршрутизации)
wakeonlan -i 192.168.88.1 FF:FF:FF:FF:FF:FF
```