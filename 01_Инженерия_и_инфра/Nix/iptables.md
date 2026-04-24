
Пример из задачи

``` sh
#!/bin/bash
WAN_IF="eth0"
LAN_IF="eth1"
LAN_NET="10.20.0.0/26"
WS22="10.20.0.3"

#clear tables ruules
iptables -F
iptables -F -t nat

#policy drop
iptables -P FORWARD DROP

#icmp accept
iptables -A FORWARD -p icmp -j ACCEPT

#traffic lan to wan
iptables -A FORWARD -i "$LAN_IF" -s "$LAN_NET" -o "$WAN_IF" -j ACCEPT

#traffic wan to lan  
iptables -A FORWARD -i "$WAN_IF" -o "$LAN_IF" -d "$WS22" -p tcp --dport 80 -j ACCEPT

#input tcp
iptables -A FORWARD -i eth0 -o eth1 -p tcp --sport 80 -j ACCEPT

#SNAT
iptables -t nat -A POSTROUTING -s "$LAN_NET" -o "$WAN_IF" -j MASQUERADE

#DNAT
iptables -t nat -A PREROUTING -p tcp -d 10.100.0.12 --dport 8080 -j DNAT --to "$WS22:80"
```

iptables - инструмент управления сетями в Linux, позволяет управлять входящими и исходящими пакетами данных. Основной инструмент настройки межсетевых экранов в Linux.

Работает путем проверки пакетов данных на соответвие определенным критериям и выполнения заданных действий если пакеты соответсвуют этим критериям. Критери определяются в таблицах, которые состоят из набора правил.

![[Pasted image 20250724125110.png]]

```sh

# ЧЕТЫРЕ ОСНОВНЫЕ ТАБЛИЦЫ

'filter' - основная, для фильтрации пакетов
'NAT' -для настройки NAT (network address translation)
'Mangle' - спец обработка пакетов
'Raw' - используется для обхода системы отслеживания состояний
```

Каждая таблица состоит из цепочек. 

``` sh
Цепочка - последовательность правил, которые применяются к пакетам. 

для таблицы filter

'INPUT' - входящие пакеты для ЭТОЙ систему
'FORWARD' - проходящие ЧЕРЕЗ (сквозные) систему пакеты 
'OUTPUT' - исходящие пакеты от ЭТОЙ системы
```

``` sh
для таблицы nat

'PREROUTING' - меняет назначение пакета до его маршрутизации (напр. DNAT)
'POSTROUTING' - меняет отправителя после маршрутизации (напр. SNAT)
'OUTPUT' -  применется к локально генерируемым пакетам до их отправки
```

``` sh
для таблицы mangle

'PREROUTING' - перед маршрутизацией
'INPUT' - до поступления на хост
'FORWARD' -  через хост
'OUTPUT' - исходящий
'POSTROUTING' - перед отправкой
```


Флаги 

| Краткий флаг | Полный флаг          | Назначение                                                  |
| ------------ | -------------------- | ----------------------------------------------------------- |
| `-t`         | `--table`            | Таблица (filter, nat, mangle…)                              |
| `-A`         | `--append`           | Добавить правило в цепочку ( в конец цепочки)               |
| `-I`         | `--insert`           | Вставить правило в цепочку (в начало или по номеру позиции) |
| `-D`         | `--delete`           | Удалить правило                                             |
| `-L`         | `--list`             | Показать правила                                            |
| `-F`         | `--flush`            | Очистить цепочку                                            |
| `-p`         | `--protocol`         | Протокол (tcp, udp, icmp…)                                  |
| `-s`         | `--source`           | Источник (IP-адрес/сеть)                                    |
| `-d`         | `--destination`      | Назначение (IP-адрес/сеть)                                  |
| `--sport`    | `--source-port`      | Исходный порт                                               |
| `--dport`    | `--destination-port` | Конечный порт                                               |
| `-i`         | `--in-interface`     | Входящий интерфейс                                          |
| `-o`         | `--out-interface`    | Исходящий интерфейс                                         |
| `-j`         | `--jump`             | Действие (ACCEPT, DROP, DNAT…)                              |
| `-v`         | `--verbose`          | Подробный вывод                                             |
| `-n`         | `--numeric`          | Вывод без DNS-имен                                          |

Команды просмотра правил

|Короткий|Полный|Описание|
|---|---|---|
|`-L`|`--list`|Показать правила в таблице|
|`-v`|`--verbose`|Подробный вывод (с интерфейсами, байтами)|
|`-n`|`--numeric`|Не пытаться резолвить имена (IP, порты)|
|`-t`|`--table`|Указать таблицу (`filter`, `nat`, ...)|
|`--line-numbers`|—|Показывает номера правил (удобно для `-D`)|
Восстановление, сохранение, просмотр. 

``` sh
'iptables-save' - выводит данные в stdout виде машиночитаемого конфига, показывает все таблицы сразу, можно перенаправить > в файл для последующего восстановления (sudo iptables-save > /etc/iptables/rules.v4. Можно перенаправить через  '| tee /etc/iptables/rules.v4'

'iptables-restore' - востановить (sudo iptables-restore < /etc/iptables/rules.v4)

'iptables -S' - показать правила как команды (не забыть что талицы разные)
```


Файлы 
```
- `/etc/iptables/rules.v4` - для IPv4 правил
    
- `/etc/iptables/rules.v6` - для IPv6 правил
    
- `/etc/iptables/rules.v4.bak` - backup предыдущей версии
```

`netfilter-persistent` - служба, которая автоматически загружает правила при загрузе

команда `save`  сохраняет текущие правила в файлы