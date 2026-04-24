**Postmap** - утилита  из пакета postfix для создания и управления базами данных в формате hash, btree или dbm, которые использует Postfix для быстрого доступа к данным.

### Зачем нужен?

Postfix не читает текстовые файлы напрямую - он использует скомпилированные базы данных для скорости.

### Как работает:

1. Берутся текстовые файлы (sasl_passwd, aliases и т.д.)
    
2. Postmap создает файлы `.db` (например: sasl_passwd.db)
    
3. Postfix использует эти `.db` файлы для быстрого поиска
```
# Редактируем текстовый файл
sudo nano /etc/postfix/sasl/sasl_passwd

# Создаем/обновляем базу
sudo postmap /etc/postfix/sasl/sasl_passwd

# Перезагружаем Postfix
sudo systemctl reload postfix

```