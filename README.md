# 08-08-2020
# Profile REST API

# Vagrant
-> Bash
- vagrant up / exit
- cd /vagrant

# Create Python virtual Environment
-> SHELL
- python -m venv ~/env (home directory -> folder)
+ activate env
- source ~/env/bin/activate
+ deactivate
- deactivate
# Install packge in env
- Create requirements.txt
- From env shell -> pip install -r requirements.txt
# Create django project
- django-admin.py startproject profiles_project . (from root folder has Vagrantfile)
# Create django app in project
- python manage.py startapp profiles_api
# Add setting in django project
+ Add restframework and profiles_api in project/setting.py INSTALLED_APPS like this:
- INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'rest_framework.authtoken',
    'profiles_api', 
]