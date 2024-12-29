###### Начальные настройки
```bash
pip install django
django-admin startproject [name_project]
python manage.py startapp [name_app]
```
##### Запуск сервера
```bash
python manage.py runserver 
```
##### Создание миграции
```bash
python manage.py makemigrations [name_app]
python manage.py migrate
```
##### Создание суперпользователя
```bash
python manage.py createsuperuser
```