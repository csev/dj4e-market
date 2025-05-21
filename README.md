
Dr. Chuck's Marketplace
========================

This is a starter repository to be used to set up the series of
marketplace assignments on the www.dj4e.com web site.

Running Locally on the Mac once the ve52 exists
-----------------------------------------------

If you have Django installed on your local computer you can test any of the sample
applications by going into the folder and starting the server:

    cd dj4e-maeket
    source .ve52/bin/activate   # If needed
    python manage.py runserver

And visit `http://localhost:8000`.

Setting up a Virtual Environment on the Mac or Linux
----------------------------------------------------

Then checkout this repository:

    cd ~/django_projects   # Or wherever
    git clone https://github.com/csev/dj4e-market.git market
    cd market
    python -m venv .ve52
    source .ve52/bin/activate
    python --version

The Python version for Django 5.2 should be at least `3.10`.

    pip install -r requirements52.txt
    python -m django --version

Running on PythonAnywhere
-------------------------

Once you have checked out the code under `django_projects`, and
ran the migrations and load scripts,
go under the Web tab, update the config files to point to your new application:

    Source code:                /home/--your-account--/django_projects/market
    Working Directory:          /home/--your-account--/django_projects/market

Use this as your `WGSI configuration file`:

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

