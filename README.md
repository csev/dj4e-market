
Dr. Chuck's Marketplace
========================

This is a starter repository to be used to set up the series of
marketplace assignments on the www.dj4e.com web site.

Installing this on PythonAnywhere
---------------------------------

Checkout this repository:

    cd ~/django_projects   # Or wherever if you are developing locally
    git clone https://github.com/csev/dj4e-market.git market
    cd market
    python -m venv .ve52
    source .ve52/bin/activate
    python --version

The Python version for Django 5.2 should be at least `3.10`.  It will most likely be
`3.12` or later.  Once you verify your python version is correct, run:

    pip install --upgrade pip
    pip install -r requirements52.txt
    python -m django --version

Your Django version should be `5.2` or later.  To make sure you have your depenencies
correct run

    python manage.py check

Until you see output like:

    python manage.py check
    When you want to use social login, please see dj4e-samples/github_settings-dist.py
    Using registration/login.html as the login template
    System check identified no issues (0 silenced).

*Important*: If you get any kind of traceback, you need to fix it before continuing.
You should run `python manage.py check` until it has no errors.

Running on PythonAnywhere
-------------------------

We are going to switch your application on PythonAnywhere from using an
SQLite database to a MySQL database for the rest of this course.  If you keep running
SQLite and your application stores too much data it will start to slow down.
If you are running locally, you can keep using SQLite.

To use MySQL, first go to the `Databases` tab in PythonAnywhere. Make a MySQL database
named `mkt` and choose a name and password and write them down.

(8) Edit `~/django_projects/market/market/settings.py` and find the existing
value for the `DATABASES` variable and comment it out.

    # DATABASES = {
    #     'default': {
    #         'ENGINE': 'django.db.backends.sqlite3',
    #         'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    #     }
    # }

Add an entry to point Django at your newly created MySQL database.  In this example,
your PythonAnywhere account is `drchuck` and the database you created is `mkt` and
the password you set for the database is `phone_153742`.   Change the sample values below
to match the values for your database.

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'drchuck$ads',
            'USER': 'drchuck',
            'PASSWORD': 'phone_153742',
            'HOST': 'drchuck.mysql.pythonanywhere-services.com',
             'OPTIONS': {
                'init_command': "SET sql_mode='STRICT_TRANS_TABLES'",
            },
        }
    }

Once you have made the changes to `~/django_projects/market/market/settings.py` 
run `python manage.py check` until there are no errors.

Once `check` works you will need to run your migrations and make a new
administrator account.  Again if you encounter any error in these commands
stop and figure out the error before proceeding.

    cd ~/django_projects/market
    python manage.py makemigrations      # Probably will say "no changes"
    python manage.py migrate
    python manage.py createsuperuser

At this point you need to configure your PythonAnywhere `Web` application to
point to the new project directory.
Under the Web tab, update the config files to point to your new project:

    Source code:                /home/--your-account--/django_projects/market
    Working Directory:          /home/--your-account--/django_projects/market

Edit the `WGSI configuration file` and replace it with the following:

    import os
    import sys

    path = os.path.expanduser('~/django_projects/-market')
    if path not in sys.path:
        sys.path.insert(0, path)
    os.environ['DJANGO_SETTINGS_MODULE'] = 'market.settings'
    from django.core.wsgi import get_wsgi_application
    from django.contrib.staticfiles.handlers import StaticFilesHandler
    application = StaticFilesHandler(get_wsgi_application())

You can edit these files and settings in the Web tab to switch between
your various projects on PythonAnywhere.  Make sure to reload under the Web tab after
every file or configuration change.

Once this is done reload your web application and navigate to your application.
