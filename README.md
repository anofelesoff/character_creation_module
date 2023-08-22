# Проект infra_sprint1 aka Kittygram

Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Это полностью рабочий проект, который состоит из бэкенд-приложения на Django и фронтенд-приложения на React.

## Установка

## Зависимости

- Python (версия 3.10.12)
- Django (версия 4.2.4.)
- Django REST framework (версия 3.14.0)

## Установка

1. Клонируйте реппозиторий ```git clone https://github.com/anofelesoff/infra_sprint1.git```
2. Перейдите в папку проекта `cd infra_sprint1`
3. [Создайте и активируйте виртуальное окружение](https://docs.python.org/3/library/venv.html) для проекта:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
4. Установитbashе зависимости из файла requirements.txt:
   ```pip install -r requirements.txt```
6. Выполните миграции
   
   `sh
   python manage.py migrate`
