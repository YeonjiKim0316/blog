PS C:\IT\Django\todoapp\todo_list> python manage.py startapp base
PS C:\IT\Django\todoapp\todo_list> python manage.py runserver


1. app들을 connect
C:\IT\Django\todoapp\todo_list\todo_list\settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'base.apps.BaseConfig',
]

