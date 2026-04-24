
Установка и запуск

``` sh
# Обновляем список пакетов
sudo apt update

# Устанавливаем необходимые зависимости
sudo apt install -y curl

# Скачиваем официальный бинарный файл GitLab Runner для архитектуры AMD64/x86-64
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"

# Даем права на выполнение
sudo chmod +x /usr/local/bin/gitlab-runner

# Создаем пользователя gitlab-runner для безопасности
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Устанавливаем и запускаем GitLab Runner как службу
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
# проверка статуса
sudo gitlab-runner status
```

Про безопастность:

```sh 
# Проверим пароль gitlab-runner
sudo passwd -S gitlab-runner

# Вы увидите `L` или `NP` - что означает "заблокирован" или "нет пароля". **Это нормально!**
```

**Почему это безопасно:**

- Пользователь `gitlab-runner` - **системный**, не предназначен для интерактивного входа
- Вход по SSH возможен только для вашего пользователя (`s3d0y`)
-  Для выполнения команд от имени `gitlab-runner` нужно иметь `sudo` права (есть только у вас)

Дополнительно сделаем и проверим:
``` sh 
# 1. Запретим логин для gitlab-runner
sudo usermod -s /usr/sbin/nologin gitlab-runner

# 2. Проверим, что нет SSH ключей у gitlab-runner
sudo ls -la /home/gitlab-runner/.ssh/

# 3. Убедимся, что gitlab-runner не входит в sudoers
sudo grep gitlab-runner /etc/sudoers
```

``` sh
# регистрация ранера на машине. До этого нужно получить урл и токен
sudo gitlab-runner register
```


Прошел по шагам регистрации

``` sh
1. **Enter the GitLab instance URL:**
   - Введите URL из настроек Runners (например, `https://gitlab.com`).
2. **Enter the registration token:**
   - Введите тот самый **Token** из секции Specific Runners.
3. **Enter a description for the runner:**
   - Введите понятное описание, например `my-ubuntu-22-server`. Это поможет вам идентифицировать Runner в веб-интерфейсе.
4. **Enter tags for the runner (comma separated):**
   - Тэги — это метки, которые позволяют запускать определенные задания только на определенных Runner. Пока можно пропустить, нажав `Enter`. Позже мы их настроим.

5. **Enter optional maintenance note for the runner:**
   - Это необязательное поле. Можно пропустить (`Enter`).
5. **Enter an executor:**
   - Это очень важный вопрос. Executor определяет, _как_ Runner будет выполнять ваши задания.
   - Для начала рекомендую выбрать **`shell`**. Это самый простой вариант, который запускает команды напрямую в оболочке (shell) вашей Ubuntu. Он отлично подходит для обучения и простых сценариев.
   - Другие популярные варианты (которые мы, возможно, рассмотрим позже): `docker`, `docker-ssh`, `virtualbox`.


После ввода всех данных вы должны увидеть сообщение, что Runner зарегистрирован.

```

Возможо пригодится путь и токен

``` sh
WARNING: Support for registration tokens and runner parameters in the 'register' command has been deprecated in GitLab Runner 15.6 and will be replaced with support for authentication tokens. For more information, see https://docs.gitlab.com/ci/runners/new_creation_workflow/ 
Registering runner... succeeded

correlation_id=01K8NH1XT852AKYBC8APFH5DCN runner=GK94R288j

Enter an executor: docker, docker-windows, kubernetes, docker-autoscaler, instance, custom, shell, ssh, docker+machine, parallels, virtualbox:
shell
Runner registered successfully. Feel free to start it, but if its running already the config should be automatically reloaded!
 
Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml" 

```