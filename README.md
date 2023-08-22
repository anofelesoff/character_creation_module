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

# Установка и настройка WSGI-сервера Gunicorn

1. Подключитесь к удалённому серверу, активируйте виртуальное окружение
   бэкенд-приложения и установите пакет `gunicorn`:

   ```bash
   # Установка и настройка WSGI-сервера Gunicorn
   pip install gunicorn==20.1.0
   ```
2. Перейдите в директорию с файлом manage.py, и запустите Gunicorn:

   ```bash
   gunicorn --bind 0.0.0.0:8000 backend.wsgi
   ```

3. Создайте файл конфигурации юнита systemd для Gunicorn в директории
`/etc/systemd/system/`. Назовите его по шаблону gunicorn_название_проекта.service:
   ```bash
   sudo nano /etc/systemd/system/gunicorn_название_проекта.service
   ```

4. Подставьте в код из листинга свои данные, добавьте этот код без комментариев в файл
конфигурации Gunicorn и сохраните изменения:
   ```bash
   [Unit]
   # Это текстовое описание юнита, пояснение для разработчика.
   Description=gunicorn daemon
   # Условие: при старте операционной системы запускать процесс только после того,
   # как операционная система загрузится и настроит подключение к сети.
   After=network.target

   [Service]
   # От чьего имени будет происходить запуск.
   User=имя_пользователя_в_системе
   # Путь к директории проекта.
   WorkingDirectory=/home/имя_пользователя/папка_с_проектом/папка_с_файлом_manage.py/
   # Команду, которую вы запускали руками, теперь будет запускать systemd.
   # Чтобы узнать путь до Gunicorn, воспользуйтесь командой which gunicorn.
   ExecStart=/.../venv/bin/gunicorn --bind 0.0.0.0:8000 backend.wsgi:application

   [Install]
   # В этом параметре указывается вариант запуска процесса.
   # Значение <multi-user.target> указывает, чтобы systemd запустил процесс
   # доступный всем пользователям и без графического интерфейса.
   WantedBy=multi-user.target
   ```

Команда `sudo systemctl` с параметрами start, stop или restart запустит, остановит
или перезапустит Gunicorn. Например, вот команда запуска:

   ```bash
   sudo systemctl start gunicorn_название_проекта
   ```

Чтобы systemd следил за работой демона Gunicorn, запускал его при старте системы
и при необходимости перезапускал, используйте команду:

   ```bash
   sudo systemctl enable gunicorn_название_проекта
   ```

# Установка и настройка веб- и прокси-сервера Nginx

1. Если Nginx ещё не установлен на удалённый сервер, установите его:
   ```bash
   sudo apt install nginx -y
   ```
2. Запустите Nginx командой:
   ```bash
   sudo systemctl start nginx
   ```
3. Обновите настройки Nginx. Для этого откройте файл конфигурации веб-сервера…
   ```bash
   sudo nano /etc/nginx/sites-enabled/default
   ```
…очистите содержимое файла и запишите новые настройки:
   ```python
server {
 listen 80;
 server_name ваш_домен;
 location /api/ {
 proxy_pass http://127.0.0.1:8000;
 }

 location /admin/ {
 proxy_pass http://127.0.0.1:8000;
 }
 location / {
 root /var/www/имя_проекта;
 index index.html index.htm;
 try_files $uri /index.html;
 }
}
```
Сохраните изменения в файле, закройте его и проверьте на корректность:
   ```bash
   sudo nano /etc/nginx/sites-enabled/default
   ```
Перезагрузите конфигурацию Nginx:
   ```bash
   sudo systemctl reload nginx
   ```
# Настройка файрвола ufw

Файрвол установит правило, по которому будут закрыты все порты, кроме тех, которые
вы явно укажете. 
1. Активируйте разрешение принимать запросы только на порты 80, 443 и 22
   ```bash
   # 80, 443: с ними будут работать пользователи, делая запросы к приложению.
   sudo ufw allow 'Nginx Full'
   # 22: нужен, чтобы вы могли подключаться к серверу по SSH.
   sudo ufw allow OpenSSH
   ```
2. Включите файрвол:
   ```bash
   sudo ufw enable
   ```
В терминале выведется запрос на подтверждение операции. Введите y и нажмите Enter.
3. Проверьте работу файрвола:
   ```bash
   sudo ufw status
   ```

# Получение и настройка SSL-сертификата

1. Находясь на сервере, установите certbot, если он ещё не установлен:
   ```bash
   # Установка пакетного менеджера snap.
   sudo apt install snapd
   # Установка и обновление зависимостей для пакетного менеджера snap.
   sudo snap install core; sudo snap refresh core
   # Установка пакета certbot.
   sudo snap install --classic certbot
   # Обеспечение доступа к пакету для пользователя с правами администратора.
   sudo ln -s /snap/bin/certbot /usr/bin/certbot
   ```

2. Запустите certbot и получите SSL-сертификат. Для этого выполните команду:
   ```bash
   sudo certbot --nginx
   ```

Далее система попросит вас указать электронную почту и ответить на несколько вопросов. Сделайте это.
Следующим шагом укажите имена, для которых вы хотели бы активировать HTTPS:

   ```bash
Account registered.
Which names would you like to activate HTTPS for?
We recommend selecting either all domains, or all domains
in a VirtualHost/server block.
1: <доменное_имя_вашего_проекта_1>
2: <доменное_имя_вашего_проекта_2>
3: <доменное_имя_вашего_проекта_3>
...
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
```
Введите номер нужного доменного имени и нажмите `Enter`.
После этого certbot отправит ваши данные на сервер Let's Encrypt и там будет выпущен
сертификат, который автоматически сохранится на вашем сервере в системной директории /etc/ssl/. Также будет автоматически изменена конфигурация Nginx: в файл
/etc/nginx/sites-enbled/default добавятся новые настройки и будут прописаны пути к сертификату.

3. Проверьте конфигурацию Nginx, и если всё в порядке, перезагрузите её.
