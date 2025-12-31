
## **Детализация nginx.conf:**

### **1. `pid /tmp/nginx.pid;`**


```nginx

pid /tmp/nginx.pid;

- PID файл - где nginx хранит ID своего главного процесса
- Обычно в `/run/nginx.pid`, но там нужны root права
- Мы используем `/tmp/` для работы под non-root пользователем
```  

### **2. `events` блок**

``` nginx

events {
    worker_connections 1024;
}

- Настройки обработки соединений
- `worker_connections 1024` = каждый worker процесс может обрабатывать до 1024 одновременных соединений
- Worker = отдельный процесс nginx для обработки запросов
  
```
### **3. `http` блок**

``` nginx

http {
    server {
        listen 8080;
        server_name localhost;

- HTTP сервер - обрабатывает HTTP запросы
- `listen 8080` = слушает порт 8080
- `server_name localhost` = отвечает на запросы к "localhost"
```
### **4. `location /` - главная страница**

``` nginx

location / {
    include /etc/nginx/fastcgi_params;
    fastcgi_pass fcgi-app:81;
    fastcgi_param SCRIPT_FILENAME /app/hello_fcgi;
}

- `location /` = все запросы к корню сайта (`http://localhost:8080/`)
- `fastcgi_pass fcgi-app:81` = проксирует запросы на контейнер `fcgi-app` порт 81
- `fastcgi_param SCRIPT_FILENAME` = указывает путь к FCGI приложению
```    
### **5. `location /status` - статус nginx**

``` nginx

location /status {
    stub_status on;
    access_log off;
}

- Специальный endpoint для мониторинга: `http://localhost:8080/status`
- `stub_status on` = показывает статистику nginx (обработанные запросы, активные соединения)
- `access_log off` = не логирует запросы к статусу (чтобы не засорять логи)
  
```