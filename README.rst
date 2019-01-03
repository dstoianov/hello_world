

.. EDIT the below links to use the project's github repo path. Or just remove them.

.. image:: https://requires.io/github/GITHUB_ORG/hello_world/requirements.svg?branch=master
.. image:: https://requires.io/github/GITHUB_ORG/hello_world/requirements.svg?branch=develop

Hello_World
========================

Below you will find basic setup and deployment instructions for the hello_world
project. To begin you should have the following applications installed on your
local development system:

- Python >= 3.5
- NodeJS >= 6.11
- npm >= 3.10.10
- `pip <http://www.pip-installer.org/>`_ >= 1.5
- `virtualenv <http://www.virtualenv.org/>`_ >= 1.10
- `virtualenvwrapper <http://pypi.python.org/pypi/virtualenvwrapper>`_ >= 3.0
- Postgres >= 9.3
- git >= 1.7

A note on NodeJS 6.x for Ubuntu users: this LTS release may not be available through the
Ubuntu repository, but you can configure a PPA from which it may be installed::

    curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
    sudo apt-get install -y nodejs

You may also follow the manual instructions if you wish to configure the PPA yourself:

    https://github.com/nodesource/distributions#manual-installation

Django version
------------------------

The Django version configured in this template is conservative. If you want to
use a newer version, edit ``requirements/base.txt``.

Getting Started
------------------------

First clone the repository from Github and switch to the new directory::

    $ git clone git@github.com:[ORGANIZATION]/hello_world.git
    $ cd hello_world

To setup your local environment you can use the quickstart make target `setup`, which will
install both Python and Javascript dependencies (via pip and npm) into a virtualenv named
"hello_world", configure a local django settings file, and create a database via
Postgres named "hello_world" with all migrations run::

    $ make setup
    $ workon hello_world

If you require a non-standard setup, you can walk through the manual setup steps below making
adjustments as necessary to your needs.

To setup your local environment you should create a virtualenv and install the
necessary requirements::

    # Check that you have python3.5 installed
    $ which python3.5
    $ mkvirtualenv hello_world -p `which python3.5`
    (hello_world)$ pip install -r requirements/dev.txt
    (hello_world)$ npm install

Next, we'll set up our local environment variables. We use `django-dotenv
<https://github.com/jpadilla/django-dotenv>`_ to help with this. It reads environment variables
located in a file name ``.env`` in the top level directory of the project. The only variable we need
to start is ``DJANGO_SETTINGS_MODULE``::

    (hello_world)$ cp hello_world/settings/local.example.py hello_world/settings/local.py
    (hello_world)$ echo "DJANGO_SETTINGS_MODULE=hello_world.settings.local" > .env

Create the Postgres database and run the initial migrate::

    (hello_world)$ createdb -E UTF-8 hello_world
    (hello_world)$ python manage.py migrate

If you want to use `Travis <http://travis-ci.org>`_ to test your project,
rename ``project.travis.yml`` to ``.travis.yml``, overwriting the ``.travis.yml``
that currently exists.  (That one is for testing the template itself.)::

    (hello_world)$ mv project.travis.yml .travis.yml

Development
-----------

You should be able to run the development server via the configured `dev` script::

    (hello_world)$ npm run dev

Or, on a custom port and address::

    (hello_world)$ npm run dev -- --address=0.0.0.0 --port=8020

Any changes made to Python, Javascript or Less files will be detected and rebuilt transparently as
long as the development server is running.

Deployment
----------

There are `different ways to deploy <http://caktus.github.io/developer-documentation/deploy-strategies.html>`_.
Here are a couple of them that could be used for hello_world.

Deployment with fabric
......................

Fabric does not yet support Python 3. You
must either create a new virtualenv for the deployment::

    # Create a new virtualenv for the deployment
    $ mkvirtualenv hello_world-deploy -p `which python2.7`
    (hello_world-deploy)$ pip install -r requirements/deploy.txt

or install the deploy requirements
globally::

    $ sudo pip install -r requirements/deploy.txt


You can deploy changes to a particular environment with
the ``deploy`` command::

    $ fab staging deploy

New requirements or migrations are detected by parsing the VCS changes and
will be installed/run automatically.

Deployment with Dokku
.....................

Alternatively, you can deploy the project using Dokku. See the
`Caktus developer docs <http://caktus.github.io/developer-documentation/dokku.html>`_.
