Настриивается в файле etc/ssh/sshd_conf

Строка PermitRootLogin

| значение            | описание                                |
| ------------------- | --------------------------------------- |
| `yes`               | Полный доступ`root`по SSH (небезопасно) |
| `no`                | Полный запрет входа под`root`           |
| `prohibit-password` | Только без пароля (через ключи)         |
| `without-password`  | То же, что и`prohibit-password`         |
после измений нужно перезапустить службу `sudo systemctl restart sshd`