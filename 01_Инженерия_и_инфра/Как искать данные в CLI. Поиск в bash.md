
``` sh
# Ищем "alias" в файле .zshrc
grep "alias" ~/.zshrc

# Ищем с подсветкой
grep --color=auto "alias" ~/.zshrc
```

``` sh
# Ищем во ВСЕХ файлах в домашней директории
grep -r "alias" ~/

# Ищем только в .zshrc и .bashrc
grep "alias" ~/.zshrc ~/.bashrc
```

``` sh
# 1. Ищем алиасы в основном файле zsh
grep "alias" ~/.zshrc

# 2. Ищем во ВСЕХ zsh конфигах (включая подпапки)
grep -r "alias" ~/.zsh* ~/.oh-my-zsh/

# 3. Ищем только строки НАЧИНАЮЩИЕСЯ с alias
grep "^alias" ~/.zshrc

# 4. Ищем с контекстом (показывает строки до/после)
grep -A 2 -B 2 "alias" ~/.zshrc
```

``` sh
 # Ищем алиасы, игнорируя регистр
grep -i "alias" ~/.zshrc

# Ищем с номерами строк (удобно для редактирования)
grep -n "alias" ~/.zshrc

# Ищем только имена алиасов (регулярное выражение)
grep -o "alias [^=]*" ~/.zshrc
```

``` sh
# Найти все алиасы и отсортировать
grep "^alias" ~/.zshrc | sort

# Найти и показать только имена алиасов
grep "^alias" ~/.zshrc | cut -d' ' -f2 | cut -d'=' -f1

```


``` sh

# 1. Найти ВСЕ алиасы в системе
grep -r "^alias" ~/.* /etc/zsh/ 2>/dev/null

# 2. Найти где определён конкретный алиас (например, 'll')
grep -r "alias ll" ~/

# 3. Найти алиасы с комментариями
grep -A 1 "^alias" ~/.zshrc | grep -E "(alias|#)"

```