# How to create a Django REST API

Instructions for how to start a Django REST API
---

1. ## Create a directory to hold your project

   ### Terminal Commands

   ```shell
   # Create Directory
   $ mkdir djangorestapi

   # Open directory (VScode)
   $ code djangorestapi

   # Open directory (Mac Termina)
   $ open djangorestapi
   ```

   ### Current File Structure

   ```
   djangorestapi
   ```

1. ## .gitignore for Djanjo Application

   If you are pushing your code to github, you will need to ignore certain foles before pushing up. An example .gitignore file is provided in this repo

   ### Current File Structure

   ```
   djangorestapi
       .gitignore
   ```

1. ## Create Virtual Environment

   After creating this environment dot forget to add it to the .gitignore

   ### Terminal Commands

   ```shell
   $ python -m venv env # On Windows use `env\Scripts\activate`
   ```

   ### Current File Structure

   ```
   djangorestapi
   │   .gitignore
   │
   └───env
   ```

1. ## Install Django and Django REST Framework into the Virtual Environment

   ### Terminal Commands

   ```shell
   $ pip install django
   $ pip install djangorestframework
   ```

1. ## Create project

   - Create project in a directory named after the project name

     ### Terminal Commands

     ```shell
     $ django-admin startproject djangorestproj

     ```

     ### Current File Structure

     ```
     djangorestapi
     │   .gitignore
     │
     ├───env/
     │
     └───djangorestproj/
         │   manage.py
         │
         └───djangorestproj/
             │   __init__.py
             │   settings.py
             │   urls.py
             │   wsgi.py
             └───__pycache__/
     ```

Next, install these third-party packages for use in your project.

```sh
pipenv install django autopep8 pylint djangorestframework django-cors-headers pylint-django
```

   - Create project in current directory

     ### Terminal Commands

     ```shell
     $ django-admin startproject djangorestproj .
     ```

     ### Current File Structure

     ```
     djangorestapi/
     │   .gitignore
     │   manage.py
     │
     ├───env/
     │
     └───djangorestproj/
         │   __init__.py
         │   settings.py
         │   urls.py
         │   wsgi.py
         └───__pycache__/
     ```

1. ## Start Server

   ### Terminal Commands

   ```shell
   $ python manage.py runserver
   ```
## Controlling Lint Errors

### Add Pylint file

The pylint package is very good at ensuring that you follow the community standards for variable naming, but there are certain times that you want to use variable names that are short and don't use snake case. You can put those variable names in a `.pylintrc` file in your project.

Without this configuration, your editor will put orange squiggles under those variable names to alert you that you violated community standards. It becomes annoying, so you override the default rules.

```sh
echo '[FORMAT] \n  good-names=i,j,ex,pk\n\n[MESSAGES CONTROL]\n  disable=broad-except,imported-auth-user,missing-class-docstring,no-self-use,abstract-method\n\n[MASTER]\n  disable=C0114,\n' > .pylintrc
```

### Select Python Interpreter

Open VS Code and press <kbd>⌘</kbd><kbd>SHIFT</kbd><kbd>P</kbd> (Mac), or <kbd>Ctrl</kbd><kbd>SHIFT</kbd><kbd>P</kbd> (Windows) to open the Command Palette, and select "Python: Select Interpreter".

Find the option that has:

`<your folder name>-<random string>`

### Configure Pylint

After selecting the python interpreter, you may see a pop-up asking if you'd like to enable Pylint. If so, click yes.

Otherwise, open the VS Code Command Palette <kbd>⌘</kbd><kbd>SHIFT</kbd><kbd>P</kbd> (Mac), or <kbd>Ctrl</kbd><kbd>SHIFT</kbd><kbd>P</kbd> (Windows), and select "Python: Select Linter".

Find the option that has:

`pylint`

#### Pylint Settings for Django

There should now be a .vscode folder in your directory. Open the `settings.json` file and add the following lines:

> `levelup-server/.vscode/settings.json`

```json
"python.linting.pylintArgs": [
    "--load-plugins=pylint_django",
    "--django-settings-module=<folder name>.settings",
],
```

#### *Notice that `<folder name>` should be the name of the folder that has the `settings.py` file, in this case it will be `levelup.settings`*



1. ## Create application

   ### Terminal Commands

   ```shell
   $ cd djangorestproj
   $ python manage.py startapp djangorestapp
   ```

   ### Current File Structure

   ```
    djangorestapi/
    │   .gitignore
    │   manage.py
    │
    ├───env/
    │
    └───djangorestproj/
        │   __init__.py
        │   settings.py
        │   urls.py
        │   wsgi.py
        │
        ├───__pycache__/
        │
        └──djangorestapp/
            │   __init__.py
            │   admin.py
            │   apps.py
            │   models.py
            │   tests.py
            │   views.py
            │
            └────migrations/
                    __init__.py
   ```

## Add Content To .gitignore File

Create a `.gitignore` file and generate the content for it by running this command

```sh
curl -L -s 'https://raw.githubusercontent.com/github/gitignore/master/Python.gitignore' > .gitignore
```

## Update Settings

Below, there are four sections of your project's `settings.py` module. Replace your existing sections with the code below.

These settings changes will be needed for any REST API application that you make. The only thing that will differ between applications is the name of the application itself.

Below, you can see `appname` in the list of installed apps. Whatever project you create in the future, your application names in that project will go there instead.

> `/settings.py`

```py

# UPDATE THIS
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'rest_framework.authtoken',
    'corsheaders',
    'appname',
]

# THIS IS NEW
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.TokenAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
}

# THIS IS NEW
CORS_ORIGIN_WHITELIST = (
    'http://localhost:3000',
    'http://127.0.0.1:3000'
)

# UPDATE THIS
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

1. ## Migrate database

   ### Terminal Commands

   ```shell
   # First go to directory that contains the `manage.py`
   $ python manage.py migrate
   ```

1. ## Create a superuser

   Follow the directions that come up after running the following command

   ### Terminal Commands

   ```shell
   $ python manage.py createsuperuser
   ```