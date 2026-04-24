#DNF #nix #repo

Текстовые файлы лежат тут - /etc/yum.repos.d

выглядят так 
```
[yandex]
name=Yandex
baseurl=http://repo.mirror.yandex.net/yandex-disk/rpm/stable/$basearch/
enabled=0
gpgcheck=1
gpgkey=http://repo.mirror.yandex.net/yandex-disk/YANDEX-DISK-KEY.GPG
```

Установил в параметре  enabled=0 чтобы он не опрашивал этот репозиторий при обнове. Но нужно проверить работу диска тогда 