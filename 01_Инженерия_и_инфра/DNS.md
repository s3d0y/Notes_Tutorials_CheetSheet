



полезная команда 
### dig

Утилита dig помогает проверить, как система преобразует доменные имена в IP-адреса.

```
dig selectel.ru
```

Вывод включает имя и IP-адрес, задержку запроса, а также сервер, который дал ответ:

```
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 9676
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;selectel.ru.			IN	A

;; ANSWER SECTION:
selectel.ru.		78	IN	A	85.119.149.3

;; Query time: 56 msec
;; SERVER: 192.168.122.1#53(192.168.122.1) (UDP)
;; WHEN: Wed Jul 02 16:26:36 EDT 2025
;; MSG SIZE  rcvd: 56
```