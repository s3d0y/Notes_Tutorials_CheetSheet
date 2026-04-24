
```sh 
# Обновляем пакеты
sudo apt update

# Устанавливаем зависимости
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

# Добавляем официальный GPG ключ Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Добавляем репозиторий Docker
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Обновляем пакеты и устанавливаем Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```



``` sh
# Проверяем версию
docker --version

# Запускаем тестовый контейнер
sudo docker run hello-world
```


Добавляем пользователя в группу docker:

``` sh 
# Добавляем s3d0y в группу docker (чтобы не использовать sudo)
sudo usermod -aG docker s3d0y

# Применяем изменения (перелогиниваемся)
exec su -l s3d0y

# Проверяем что работает без sudo
docker ps
```

``` sh
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```


``` sh
# Устанавливаем последнюю версию Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Даем права на выполнение
sudo chmod +x /usr/local/bin/docker-compose

# Проверяем
docker-compose --version
```


``` sh

# Устанавливаем инструменты для работы с Docker
sudo apt install -y \
    htop \
    tree \
    jq \          # для работы с JSON
    yamllint \    # для проверки YAML файлов
    httpie        # современный curl

# Или выборочно что **нужно**
```



``` sh
mkdir -p ~/docker-labs/{projects,data,backups,scripts,logs,shared} && \
mkdir -p ~/docker-labs/projects/{web-app,database,monitoring,network,security} && \
mkdir -p ~/docker-labs/projects/web-app/{nginx,php,db,mysql-data} && \
mkdir -p ~/docker-labs/projects/database/{config,backups,logs} && \
mkdir -p ~/docker-labs/projects/monitoring/{prometheus,grafana,alertmanager} && \
mkdir -p ~/docker-labs/projects/network/{vpn,proxy,firewall} && \
mkdir -p ~/docker-labs/projects/security/{kali-lab,nmap-scans} && \
mkdir -p ~/docker-labs/shared/{nginx,db-backups,logs,data} && \
mkdir -p ~/docker-labs/shared/nginx/{conf.d,ssl,certs} && \
mkdir -p ~/docker-labs/shared/logs/{nginx,mysql,app} && \
mkdir -p ~/docker-labs/shared/data/{uploads,media,static}
```

```
web-app/           # Полноценное веб-приложение
database/          # Изолированный сервер БД  
monitoring/        # Система мониторинга
network/           # Эксперименты с сетями
security/          # Инструменты безопасности

nginx/            # Общие настройки веб-сервера
db-backups/       # Бэкапы всех баз данных
logs/             # Централизованные логи
data/             # Общие файлы между проектами

data/             # Тома для данных контейнеров
backups/          # Резервные копии конфигов
scripts/          # Скрипты развертывания/управления
logs/             # Общие логи инфраструктуры

```


 И еще алиасы 

``` sh
# Добавляем в ~/.zshrc
cat >> ~/.zshrc << 'EOF'

# Docker Aliases
alias dps='docker ps'
alias dpsa='docker ps -a'
alias di='docker images'
alias drm='docker rm'
alias drmi='docker rmi'
alias dstop='docker stop $(docker ps -aq)'
alias dclean='docker system prune -f'

# Docker Compose
alias dcup='docker-compose up -d'
alias dcdown='docker-compose down'
alias dclogs='docker-compose logs -f'

# Labs
alias labs='cd ~/docker-labs'
alias projects='cd ~/docker-labs/projects'
EOF

# Применяем
source ~/.zshrc
```