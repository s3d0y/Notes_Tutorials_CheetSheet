``` sh
# Скачаем в домашнюю директорию где есть права
cd ~
wget https://github.com/goodwithtech/dockle/releases/download/v0.4.13/dockle_0.4.13_Linux-64bit.tar.gz

# Распакуем
tar xzf dockle_0.4.13_Linux-64bit.tar.gz

# Проверим что файл dockle появился
ls -la dockle

# Установим
sudo mv dockle /usr/local/bin/
sudo chmod +x /usr/local/bin/dockle

# Проверим установку
dockle --version
```