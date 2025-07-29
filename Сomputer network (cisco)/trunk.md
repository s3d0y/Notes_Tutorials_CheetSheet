это тип подключения между коммутаторами или коммутатором и маршрутизатором который позволяет передать трафик нескольких VLAN  через один физический порт. 

Порт должен быть настроен как L2 (коммутатор)

####  **Основные особенности Trunk:**

- Передаёт кадры **с тегами VLAN (802.1Q)**.
    
- **Native VLAN** — VLAN, которая передаётся **без тега** (по умолчанию VLAN 1).
    
- Используется для связи коммутаторов и маршрутизаторов в много-VLAN-овых сетях.


- ``switchport mode trunk `` - перевод порта в режим trank

- ``switchport trunk native vlan 99 `` - установка native vlan
  
- ``switchport trunk allowed vlan 10,20,30 `` - достпуные vlan 
  
- ``show interfaces trunk`` - Статус trunk-портов


``` c
# Переход в привилегированный режим
enable                          
# Вход в режим конфигурации
configure terminal         
# Выбор интерфейса для настройки
interface FastEthernet1/15 
# Установка режима trunk
switchport mode trunk           
# Назначение Native VLAN (если не указано, будет VLAN 1)
switchport trunk native vlan 99 
# Разрешение только указанных VLAN
switchport trunk allowed vlan 10,20,30  
# Включение интерфейса
no shutdown                     
# Выход в привилегированный режим
end                             
# Сохранение конфигурации
write memory                    
```

