
## **Концептуальный список команд Dockerfile:**

### **1. `FROM` - Базовый образ**

```dockerfile

FROM alpine:latest

- Обязательная первая команда
- Определяет основу для образа
- Можно использовать несколько в multi-stage build
```    

### **2. `LABEL` - Метаданные (опционально)**

```dockerfile

LABEL maintainer="your@email.com"
LABEL version="1.0"

- Добавляет метаданные к образу
- Удобно для организации
``` 

### **3. `ENV` - Переменные окружения**

``` dockerfile

ENV APP_HOME=/app
ENV DOCKER_CONTENT_TRUST=1

- Устанавливает переменные, доступные в контейнере  
- Работают и при сборке, и при запуске
  ```

### **4. `RUN` - Команды сборки**


```dockerfile

RUN apk add --no-cache python3
RUN pip install -r requirements.txt

- Выполняется при сборке образа
- Каждый RUN создаёт новый слой 
- Для уменьшения размера: объединяй команды через `&&`
```   

### **5. `COPY` / `ADD` - Копирование файлов**

```dockerfile

COPY . /app
COPY requirements.txt .
ADD https://example.com/file.tar.gz /tmp/

- `COPY` - простое копирование файлов
- `ADD` - может распаковывать архивы и скачивать URL
```

### **6. `WORKDIR` - Рабочая директория**


```dockerfile

WORKDIR /app
WORKDIR /src

- Устанавливает текущую директорию
- Автоматически создаёт директорию если её нет
```

### **7. `USER` - Смена пользователя**

``` dockerfile

USER nobody
USER 1000:1000

- Меняет пользователя для последующих команд
- Важно для безопасности
  
```

### **8. `VOLUME` - Объявление томов**


``` dockerfile

VOLUME /data
VOLUME ["/data", "/logs"]

- Объявляет точки монтирования
- В основном для документации
  ```

### **9. `EXPOSE` - Декларация портов**


```dockerfile

EXPOSE 80
EXPOSE 8080/tcp

- **Не открывает порты**, только документирует
- Фактическое открытие через `-p` в `docker run`
``` 

### **10. `HEALTHCHECK` - Проверка здоровья**

``` dockerfile

HEALTHCHECK --interval=30s CMD curl -f http://localhost/ || exit 1

- Проверяет что приложение работает корректно
```

### **11. `CMD` / `ENTRYPOINT` - Запуск приложения**

``` dockerfile

CMD ["python", "app.py"]
ENTRYPOINT ["/app/start.sh"]

- Выполняется при запуске контейнера
- `CMD` = аргументы по умолчанию
- `ENTRYPOINT` = основная команда
```


###   Типичная очередность

``` dockerfile

# 1. Базовый образ
FROM alpine:latest

# 2. Метаданные
LABEL maintainer="dev@company.com"

# 3. Переменные окружения
ENV APP_HOME=/app

# 4. Установка пакетов
RUN apk add --no-cache python3

# 5. Рабочая директория
WORKDIR $APP_HOME

# 6. Копирование кода
COPY . .

# 7. Сборка/установка
RUN pip install -r requirements.txt

# 8. Права и пользователь
RUN adduser -D appuser
USER appuser

# 9. Декларации
EXPOSE 8000
HEALTHCHECK --interval=30s CMD curl -f http://localhost:8000/health || exit 1

# 10. Запуск приложения
CMD ["python", "app.py"]
```

##  **Разница CMD vs ENTRYPOINT:**
ТУТ НЕ ПОНИМАЮ 

|Сценарий|CMD|ENTRYPOINT|
|---|---|---|
|**Переопределение**|Полностью меняется|Добавляются аргументы|
|**Пример**|`CMD ["python", "app.py"]`|`ENTRYPOINT ["python"]`|
|**Запуск**|`docker run my-app`|`docker run my-app app.py`|