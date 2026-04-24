
### TASK_1

кодирование и декодирование прямо в командлайне
``` sh
# Кодирование
echo "Hello World" | base64
# SGVsbG8gV29ybGQK

# Декодирование  
echo "SGVsbG8gV29ybGQK" | base64 -d
# Hello World
```

Кодирование через сай КиберШеф

https://gchq.github.io/CyberChef/

### TASK_2

опредедить объем текста 
https://allcalc.ru/node/1997

XOR кодирование https://md5decrypt.net/en/Xor/


### TASK_3
Хеширование и сравнение

```sh
#!/bin/bash

ORIGINAL_HASH="e239f67756bba3af660e4226c340183a9ca4bdc40038c0cfdea2fbaa59605be32548df2535e5a9f9ceedb12d9666c6fb153ada99830ed5cd84eb0c2c4d00260a"

echo "Testing 'secretpassword':"
HASH1=$(echo -n "secretpassword" | sha512sum | cut -d' ' -f1)
echo "Generated: $HASH1"
echo "Original: $ORIGINAL_HASH"
echo "Match: $([ "$HASH1" = "$ORIGINAL_HASH" ] && echo "YES" || echo "NO")"
echo ""

echo "Testing 'Pa$$w0rd':"
HASH2=$(echo -n 'Pa$$w0rd' | sha512sum | cut -d' ' -f1)
echo "Generated: $HASH2"
echo "Original: $ORIGINAL_HASH"
echo "Match: $([ "$HASH2" = "$ORIGINAL_HASH" ] && echo "YES" || echo "NO")"
```
 
 Подробнее про работу с [[Hash (хэш)]]