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

1. Внесите изменения в `settings.py`:
   Добавьте в список `ALLOWED_HOSTS` внешний IP сервера, `127.0.0.1`, `localhost` и ваш домен:

   ```python
   ALLOWED_HOSTS = ['xxx.xxx.xxx.xxx', '127.0.0.1', 'localhost', 'ваш_домен']
   ```

2. Отключите режим отладки (Debug mode) в settings.py. В файле settings.py установите значение DEBUG в False:

   ```python
   DEBUG = False
   ```

3. Подготовьте бэкенд-приложение для сбора статики. В файле `settings.py` укажите директорию, куда будет сложена статика:

   ```python
   # Замените стандартное значение 'static' на 'static_backend'.
   STATIC_URL = 'static_backend'
   # Укажите директорию, куда бэкенд-приложение должно сложить статику.
   STATIC_ROOT = BASE_DIR / 'static_backend'
   ```

5. Скопируйте директорию static_backend/ в директорию /var/www/название_проекта/:

   ```python
   sudo cp -r путь_к_директории_с_бэкендом/static_backend /var/www/название_проекта
   ```

6. Скопируйте статику фронтенд-приложения в директорию по умолчанию:

   ```python
   # Синтаксис команды.
   # Точка после build важна — будет скопировано содержимое директории.
   sudo cp -r путь_к_директории_с_фронтенд-приложением/build/. /var/www/имя_проекта/
   ```

