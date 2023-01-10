# Api_yamdb

## Описание проекта:
API проекта api_yamdb.


## Шаблон наполнения env-файла

Секретные ключи, доступы, токены не следует хранить в коде. 
А для работы приложения их нужно передать в контейнер.
Для этого нужно - создать файл .env и прописать переменные окружения в нём.

```bash
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

## Описание команд для запуска приложения в контейнерах
Запустите docker-compose командой
```bash
docker-compose up
```
Теперь в контейнере web нужно выполнить миграции, создать суперпользователя и собрать статику. 
Команды внутри контейнеров выполняют посредством подкоманды docker-compose exec. 
Это эквивалент docker exec: с её помощью можно выполнять произвольные команды в сервисах внутри контейнеров.
```bash
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```
## Описание команды для заполнения базы данными
Задачу хранения данных в докере решает volume. 
В самом общем смысле volume — это директория, доступная для контейнера. 

Измените файл settings.py, чтобы значения загружались из переменных окружения:
```bash
DATABASES = {
    'default': {
        'ENGINE': os.getenv('DB_ENGINE'),
        'NAME': os.getenv('DB_NAME'),
        'USER': os.getenv('POSTGRES_USER'),
        'PASSWORD': os.getenv('POSTGRES_PASSWORD'),
        'HOST': os.getenv('DB_HOST'),
        'PORT': os.getenv('DB_PORT')
    }
}
```
Дальше проект api_yamdb будет работать с PostgreSQL.
## Авторы

- [@DianaKab](https://github.com/DianaKab)
