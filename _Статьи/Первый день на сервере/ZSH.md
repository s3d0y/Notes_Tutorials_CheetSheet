``` sh
# Обновляем пакеты
sudo apt update

# Устанавливаем zsh
sudo apt install zsh

# Проверяем версию
zsh --version
```

``` sh
# Установка через curl
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Или через wget
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

``` sh
# Меняем оболочку для текущего пользователя
chsh -s $(which zsh)

# Или явно указываем путь
sudo chsh -s /usr/bin/zsh s3d0y

# Перезагружаем терминал или выполняем
exec zsh
```

``` sh
# Создаем папку для кастомных плагинов
mkdir -p ~/.oh-my-zsh/custom/plugins

# Устанавливаем zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# Устанавливаем zsh-autosuggestions  
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```


``` sh
# Редактируем конфиг
nano ~/.zshrc
```


ТЕМА КАК В КАЛИ 

``` sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

```
# Устанавливаем:
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Применяем изменения 

``` sh
source ~/.zshrc
```


Запустить настройки заного 

``` sh
p10k configure

или 

# Перезагружаем zsh
exec zsh

# Пробуем снова
p10k configure

или 

# Запускаем скрипт напрямую
source ~/.p10k.zsh
# Или
~/.p10k.zsh
```