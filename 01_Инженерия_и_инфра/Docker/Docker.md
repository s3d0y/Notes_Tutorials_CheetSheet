
Тут нет ничего, только вводный видос
https://www.youtube.com/watch?v=lr1rYnUubpQ

Команды в терминале начинаются с обращения 
``` bash
docker
```

и далее испольщуются команды с ключами или без ключей

``` bash
docker run --name TrainingPostgres \
  -e POSTGRES_PASSWORD=12345 \
  -e POSTGRES_MULTIPLE_DATABASES="my_training_db,another_db" \
  -p 5432:5432 \
  -d postgres:latest
```

Пример выше - добавление в  PostgreSQL (psql) второй БД

``` bash
docker exec -it TrainingPostgres psql -U postgres -c "\l"
```

Обращенене к докеру (docker)  выполнить интерактивно (-it) в базе постгрес (psql) с именем TrainingPostgres от имени пользователя (-U) postgres команду (-с) вывести список (\l") БД

Команды 
start 
stop 
restart
ps -  список запущенных контейнеров
ps -a - список всех контейнеров
-W ключ явного запроса пароля

``` bash
docker exec -it my-postgres psql -U myuser -W -c "\list"
```



``` bash

docker run --name my-postgres \
  -e POSTGRES_USER=myuser \
  -e POSTGRES_PASSWORD=mypassword \
  -e POSTGRES_DB=mydatabase \
  -p 5432:5432 \
  -d postgres:latest

```

Создание нового контейнера