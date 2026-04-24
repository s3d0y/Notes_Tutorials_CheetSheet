```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp2s0:
      dhcp4: no
      addresses: 
        - 192.168.88.250/24
      routes:
        - to: default
          via: 192.168.88.1
      nameservers:
        addresses: 
          - 8.8.8.8
          - 1.1.1.1
        search: 
          - local
```