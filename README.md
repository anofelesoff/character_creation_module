# Проект infra_sprint1 aka Kittygram

Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Это полностью рабочий проект, который состоит из бэкенд-приложения на Django и фронтенд-приложения на React.

## Установка

## Зависимости

- Python (версия 3.10.12)
- Django (версия 4.2.4.)
- Django REST framework (версия 3.14.0)

## Установка

1. Клонируйте реппозиторий
   
   ```bash
   git clone https://github.com/anofelesoff/infra_sprint1.git
   ```
3. Перейдите в папку проекта
   
   ```bash
   cd infra_sprint1
   ```
5. [Создайте и активируйте виртуальное окружение](https://docs.python.org/3/library/venv.html) для проекта:

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
6. Установитbashе зависимости из файла requirements.txt:

   ```bash
   pip install -r requirements.txt
   ```
7. Выполните миграции
   
   ```bash
   python manage.py migrate
   ```
8. Создайте суперпользователя

   ```bash
   python manage.py createsuperuser
   ```
9. Перейдите в папку с frontend
10. Находясь в директории с фронтенд-приложением, установите зависимости для него:
    ```bash
    npm i
    ```
11. Из директории с фронтенд-приложением выполните команду:
    ```bash
    npm run build
    ```

## Настройка приложения
### Настройка backend проекта на сервере

Внесите изменения в `settings.py`:

   Добавьте в список `ALLOWED_HOSTS` внешний IP сервера, `127.0.0.1`, `localhost` и ваш домен:

   ```python
   ALLOWED_HOSTS = ['xxx.xxx.xxx.xxx', '127.0.0.1', 'localhost', 'ваш_домен']
```
   
