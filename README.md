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
# Start test after config setting.py
- python manage.py runserver 0.0.0.0:8000
# Create Model in models.py
+ Create UserProfile
- class UserProfile(AbstractBaseUser, PermissionsMixin):
    """Data base model for users in the system"""
    email = models.EmailField(max_length=255, unique=True)
    name = models.CharField(max_length=255,)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)

    objects = UserProfileManager()

    USERNAME_FIELD = 'email' --instead of 'username' field, default is required
    REQUIRED_FIELDS = ['name']

    def get_full_name(self):
        """"Retrieve full name of user"""
        return self.name
    
    def get_short_name(self):
        """"Retrieve short name of user"""
        return self.name

    def __str__(self):
        """Return string representation of our user"""
        return self.email
+ Create UserProfileManager
- class UserProfileManager(BaseUserManager):
    """Mangaer for user profile"""

    def create_user(self, email, name, password=None):
        """Create a new user profile"""
        if not email:
            raise ValueError('User must have an email address')

        email = self.normalize_email(email)
        user = self.model(email=email, name=name)

        user.set_password(password)
        user.save(using=self._db)

        return user
    
    def create_superuser(self, email, name, password):
        """Create and save superuser with given details"""
        user = self.create_user(email, name, password)
        
        user.is_superuser = True
        user.is_staff = True
        user.save(using=self._db)

        return user
# Save with select database type with object.save(using=self._db)
+ DATABASES = {
    'default': {
        'NAME': 'app_data',
        'ENGINE': 'django.db.backends.postgresql',
        'USER': 'postgres_user',
        'PASSWORD': '****'
    },
    'new_users': {
        'NAME': 'user_data',
        'ENGINE': 'django.db.backends.mysql',
        'USER': 'mysql_user',
        'PASSWORD': '****'
    }
}
-> Then if you want to use default database then use user.save(using=self._db) If you want to use new_users database then use user.save(using="new_users")
# Create migrate database django
+ Create migration app
- python manage.py makemigrations profiles_api
+ Run migate project
- python manage.py migrate