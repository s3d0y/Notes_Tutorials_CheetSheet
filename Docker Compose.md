 Примет файла сборки докеркомпоуз образа

``` docker-compouse.yml
services:
  fcgi-app:
    build: .
    container_name: fcgi_app
    networks:
      - app-network

  nginx-proxy:
    image: nginx:alpine
    container_name: nginx_proxy
    ports:
      - "80:8080"
    volumes:
      - ./nginx-proxy/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - fcgi-app
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```


### Docker Compose 

Управляет несколькими контейнерами как единым приложением

- Каждый сервис = отдельный контейнер
- Но они работают вместе как одно целое

### **2. Сеть `app-network`**

- **Bridge сеть** = виртуальная сеть внутри Docker
- Контейнеры в одной сети **видят друг друга по именам**
- `fcgi-app` → доступен как `http://fcgi-app:81`
- `nginx-proxy` → доступен как `http://nginx-proxy:8080`

### **3.  `image: nginx:alpine` для nginx-proxy**

- Не собирает, а **скачивает готовый образ**
- Быстрее и проще

### **4. Volumes - монтирование конфига**

- **Да, ты прав!** Это "монтирование" файла с хоста в контейнер
- `./nginx-proxy/nginx.conf` - файл на твоём компьютере
- `/etc/nginx/nginx.conf` - путь внутри контейнера
- `:ro` = read only (только чтение)
### **5. `depends_on` - зависимости**

- **Говорит Docker Compose**: "Сначала запусти fcgi-app, потом nginx-proxy"
- Но НЕ ждёт пока fcgi-app действительно будет готов принимать соединения
- Для настоящей проверки готовности нужен `healthcheck`
