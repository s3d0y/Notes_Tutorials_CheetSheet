КОРОТКО В ВИДЕ КОДА

``` shell
enable
configure terminal
interface GigabitEthernet0/0
# Реальный IP R1
ip address 192.168.1.2 255.255.255.0
no shutdown
# Виртуальный IP HSRP
standby 1 ip 192.168.1.1             
# Приоритет R1 (главный)
standby 1 priority 110           
# Возврат роли при восстановлении
standby 1 preempt                     
end
wr
```

``` c
enable
configure terminal
interface GigabitEthernet0/0

# Реальный IP R2
ip address 192.168.1.3 255.255.255.0
no shutdown
# Тот же виртуальный IP
standby 1 ip 192.168.1.1             
# Приоритет R2 (резервный)
standby 1 priority 90                
# Возврат роли при восстановлении
standby 1 preempt                    
end 
wr
```


``` shell
# просмотр состояния
show standby brief

#ожидаемый вывод
Interface   Grp  Prio State    Active    Standby     Virtual IP
Gi0/0       1    110  Active   local     192.168.1.3 192.168.1.1

```

ТУТ ДОПОЛНЕНИЯ ПО ТЕМЕ БОЛЕЕ РАЗВЕРНУТО