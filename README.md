
Django for Everybody Marketplace
================================

This is a starter repository to be used to set up the series of
marketplace assignments on the www.dj4e.com web site.

There is a video walkthrough of these instructions 
at <a href="https://youtu.be/a3CODtpZCLM" target="_blank">Installing the initial
version of DJ4E Marketplace from github</a>.

Installing this on PythonAnywhere
---------------------------------

If you are installing this in the middle of taking the DJ4E course, you should
already have a virtual environment set up in your bash console and in your web
tab. Open a bash console and it should look like this:

    (.ve52) 14:15 ~ $

If you have no Django 5.2 virtual envronment set up, please see the install instructions
at www.dj4e.com.

With a properly configured virtual environment in place, checkout this repository:

    cd ~/django_projects  
    git clone https://github.com/csev/dj4e-market.git market
    cd market
    python --version

The Python version for Django 5.2 should be at least `3.11`.  It will most likely be
`3.12` or later.  Once you verify your Python version is correct, run:

    cd ~/django_project/market
    pip install --upgrade pip
    pip install -r requirements52.txt
    python -m django --version

Your Django version should be `5.2` or later.

You will notice that in this project, the project-wide `settings.py` and project-wide
`urls.py` is in a folder called `market/config` not `market/market`. 
Many developers find it less confusing that using the project folder name as the project
configuration folder name.

To make sure you have your dependencies have been installed correctly, run:

    python manage.py check

Until you see output like:

    python manage.py check
    Using registration/login.html as the login template
    System check identified no issues (0 silenced).

*Important*: If you get any kind of traceback, you need to stop and fix any errors before continuing.
You should not continue with these instructions until `python manage.py check` runs without errors.

Running on PythonAnywhere
-------------------------

We are going to switch your application on PythonAnywhere from using an
SQLite database to a MySQL database for the rest of this course.  If you keep using
SQLite and your application stores too much data it will start to slow down.

**Note** If you are running this on a local computer you can just use SQLite as it is
very fast when run on a real hard drive.  If you are installing locally, skip this
section and go to "Initializing your Database".

To use MySQL, first go to the `Databases` tab in PythonAnywhere. Set a database password
that is different from your PythonAnywhwere password and different from any password
you use on other other system.

Make a MySQL database named `market` and choose a name and password and write them down.
Edit `~/django_projects/market/config/settings.py` and find the existing
SQLite value for the `DATABASES` variable and comment it out.

    # DATABASES = {
    #     'default': {
    #         'ENGINE': 'django.db.backends.sqlite3',
    #         'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    #     }
    # }

Add (or update) an entry to point Django at your newly created MySQL database.  In this example
DATABASES value, your PythonAnywhere account is `drchuck` and the database you
created is `market` and the password you set for the database is `phone_153742`.
Change the sample values below to match the values for your MySQL database.

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'drchuck$market',
            'USER': 'drchuck',
            'PASSWORD': 'phone_153742',
            'HOST': 'drchuck.mysql.pythonanywhere-services.com',
             'OPTIONS': {
                'init_command': "SET sql_mode='STRICT_TRANS_TABLES'",
            },
        }
    }

Once you have made the changes to `~/django_projects/market/config/settings.py` 
run 

    python manage.py check

until there are no errors.

If you are installing this to be submitted to the DJ4E autograder - make sure to launch the
autograder and check if there are additional requirements like adding a particular
adminstrator user or setting a code string for the autograder.

Initializing your Database
--------------------------

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

    path = os.path.expanduser('~/django_projects/market')
    if path not in sys.path:
        sys.path.insert(0, path)
    os.environ['DJANGO_SETTINGS_MODULE'] = 'config.settings'
    from django.core.wsgi import get_wsgi_application
    from django.contrib.staticfiles.handlers import StaticFilesHandler
    application = StaticFilesHandler(get_wsgi_application())

You can edit these files and settings in the Web tab to switch between
your various projects on PythonAnywhere.  Make sure to reload under the Web tab after
every file or configuration change.

Once this is done reload your web application and navigate to your application.

Changing the Name of Your Application
-------------------------------------

In the `market/config/settings.py` there is a `APP_NAME` variable.   Give your application a name
other than the default of 'Chucks Marketplace".  It does not have to be your name - just something
other than the default.

Once this is done reload your web application and navigate to your application and verify the name
in the top menu has chaged.

Testing your Application
------------------------

Once your application starts and is running, you can enter a few URLs in your browser
to check how much is working.

    https://your-account.pythonanywhere.com/

Will should show a simple welcome page - we will replace this in a later assignment.

    https://your-account.pythonanywhere.com/home

Will show the same simple welcome page - we will keep this throughout the series of assignments

    https://your-account.pythonanywhere.com/favicon.ico

Will show a small "favicon" with "4E" and a blue background - you will replace this later

    https://your-account.pythonanywhere.com/admin

Will allow you to log in and test your administrator account and password and
show a few Django internal tables.



