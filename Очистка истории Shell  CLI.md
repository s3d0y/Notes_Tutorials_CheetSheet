
Надо понять всю историю чистит, вне зависимсти от того какой SH используется? 

``` sh
# Очистить историю в памяти (текущая сессия)
history -c
```

``` sh
# Очистить файл истории полностью
cat /dev/null > ~/.bash_history

# Или удалить файл и создать заново
rm ~/.bash_history && touch ~/.bash_history
```

``` sh
# Просмотреть историю с номерами
history

# Удалить конкретную команду по номеру
history -d 123

# Удалить диапазон команд
history -d 123-130as
```

``` sh
# Комбинация: очистить память + файл
history -c && history -w
```


``` sh
# Очистить историю zsh
history -p
# Или
rm ~/.zsh_history && touch ~/.zsh_history
```

``` sh
# Временно отключить запись истории
set +o history

# Включить обратно
set -o history

# Или добавить в ~/.bashrc для постоянного отключения:
echo "set +o history" >> ~/.bashrc
```


``` sh
# Очистка памяти ОС (требует sudo)
sudo sync && echo 3 | sudo tee /proc/sys/vm/drop_caches

# Очистка swap
sudo swapoff -a && sudo swapon -a
```