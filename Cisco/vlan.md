``` C 
enable  
configure terminal  
# Создаём VLAN 10  
vlan 10
# Опционально: задаём имя  
name SALES  
exit  

vlan 20  
name HR  
exit

```


получается если устройсто L2 то на нем нельзя настроить ip  адреса для интерфейсов vlan ? 


! На L3-коммутаторе или роутере:

``` C

interface Vlan10
 ip address 192.168.10.1 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.1 255.255.255.0
```

