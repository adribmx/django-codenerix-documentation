Settings
========

.. code:: python
    
    # Warning. When you start a new project with django, you should keep base configuration from django. 

    # Codenerix Constants


    USERNAME_MIN_SIZE = 6
    PASSWORD_MIN_SIZE = 8
    ALARMS_LOOPTIME = 15000     # Refresh alarms every 15 seconds (15.000 miliseconds)
    ALARMS_QUICKLOOP = 1000     # Refresh alarms every 1 seconds (1.000 miliseconds) when the system is on quick loop processing (without focus)
    ALARMS_ERRORLOOP = 5000     # Refresh alarms every 5 seconds (5.000 miliseconds) when the http request fails
    CONNECTION_ERROR = 60000    # Connection error after 60 seconds (60.000 miliseconds)
    ALL_PAGESALLOWED = True

    LIMIT_FOREIGNKEY = 25

    PQPRO_CASSANDRA == TRUE       # If True, Log and GenLog doesn't exist

    CODENERIX_CSS = '<link href="/static/codenerix/codenerix.css" rel="stylesheet">'
    CODENERIX_JS ='<script type="text/javascript" src="/static/codenerix/codenerix.js"></script>'
    CODENERIX_JS +='<script type="text/javascript" src="/static/codenerix/codenerix.extra.js"></script>'

    # Other definitions about dates, hours
    DATETIME_FORMAT='Y-m-d H:i'

    DATETIME_INPUT_FORMATS=('%Y-%m-%d %H:%M',)

    TIME_FORMAT='H:i'

    TIME_INPUT_FORMATS=('%H:%M', '%H%M')

    DATETIME_RANGE_FORMAT=('%Y-%m-%d', 'YYYY-MM-DD') 

    DATERANGEPICKER_OPTIONS = '{{'
    DATERANGEPICKER_OPTIONS+= '    format: "{Format}",'
    DATERANGEPICKER_OPTIONS+= '    timePicker: true,'
    DATERANGEPICKER_OPTIONS+= '    timePicker12Hour: false,'
    DATERANGEPICKER_OPTIONS+= '    showDropdowns: true,'
    DATERANGEPICKER_OPTIONS+= '    locale: {{'
    DATERANGEPICKER_OPTIONS+= '        firstDay: 1,'
    DATERANGEPICKER_OPTIONS+= '        fromLabel: "{From}",'
    DATERANGEPICKER_OPTIONS+= '        toLabel: "{To}",'
    DATERANGEPICKER_OPTIONS+= '        applyLabel: "{Apply}",'
    DATERANGEPICKER_OPTIONS+= '        cancelLabel: "{Cancel}",'
    DATERANGEPICKER_OPTIONS+= '        daysOfWeek: ["{Su}", "{Mo}", "{Tu}", "{We}", "{Th}", "{Fr}","{Sa}"],'
    DATERANGEPICKER_OPTIONS+= '        monthNames: ["{January}", "{February}", "{March}", "{April}", "{May}", "{June}", "{July}", "{August}", "{September}", "{October}", "{November}", "{December}"],'
    DATERANGEPICKER_OPTIONS+= '    }},'
    DATERANGEPICKER_OPTIONS+= '}}'


    # Codenerix keys

    SECRET_KEY = '*******************************'
    PUBLIC_KEY = {
        'KERNEL': ('*************', '*************'),
    }

    MIDDLEWARE = [
        'django.middleware.security.SecurityMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.middleware.common.CommonMiddleware',
        'django.middleware.csrf.CsrfViewMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'django.contrib.messages.middleware.MessageMiddleware',
        'django.middleware.clickjacking.XFrameOptionsMiddleware',

        # Codenerix middlewares
        'codenerix.authbackend.TokenAuthMiddleware',
        'codenerix.authbackend.LimitedAuthMiddleware',
        'codenerix.middleware.SecureRequiredMiddleware',
        'codenerix.middleware.CurrentUserMiddleware',
    ]

    # Installed apps
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',

        # Codenerix apps
        'codenerix',
    ]

    # Authentication backends

    AUTHENTICATION_BACKENDS = (
        "codenerix.authbackend.TokenAuth",                # TokenAuth
        "codenerix.authbackend.LimitedAuth",            # LimitedAuth
    )

    # Templates
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                    'codenerix.context.codenerix',
                    'codenerix.context.codenerix_js',
                ],
            },
        },
    ]


Description
+++++++++++

Codenerix have some constants and extra parameters detailed in the next table.

==========================  ===================================================================================================================
Name                        Description
==========================  ===================================================================================================================
USERNAME_MIN_SIZE           Minimun size for usernames
PASSWORD_MIN_SIZE           Minimun size for password
ALARMS_LOOPTIME             Time of alarms refresh (miliseconds)
ALARMS_QUICKLOOP            Time of alarms refresh when the system is on quick loop processing (without focus)(miliseconds)
ALARMS_ERRORLOOP            Time of alarms refresh when the http request fails (miliseconds)
CONNECTION_ERROR            Connection error after X seconds (miliseconds)
ALL_PAGESALLOWED            In genlist, allowed search all pages exist.
LIMIT_FOREIGNKEY            Limit of rows showed when you do a query. (Default 100)
DATETIME_FORMAT             Format of datetime.
DATETIME_INPUT_FORMATS      Format of datetime for inputs.
TIME_FORMAT="H:i"           Format of time.
TIME_INPUT_FORMATS          Format of time for inputs.
DATETIME_RANGE_FORMAT       Format of datetime for ranges.
DATERANGEPICKER_OPTIONS     Extra options for daterangericker
PQPRO_CASSANDRA             If PQPRO are used, Log and Genlog doesn't create.
==========================  ===================================================================================================================


Django enviroment configuration
+++++++++++++++++++++++++++++++


==========
MIDDLEWARE
==========

Codenerix have four middlewares which are required to add to Django configuration:

-  codenerix.authbackend.TokenAuthMiddleware
-  codenerix.authbackend.LimitedAuthMiddleware
-  codenerix.middleware.SecureRequiredMiddleware
-  codenerix.middleware.CurrentUserMiddleware

.. code:: python
    
    MIDDLEWARE = [
        .....
        # Codenerix middlewares
        'codenerix.authbackend.TokenAuthMiddleware',
        'codenerix.authbackend.LimitedAuthMiddleware',
        'codenerix.middleware.SecureRequiredMiddleware',
        'codenerix.middleware.CurrentUserMiddleware',
    ]

==============
INSTALLED_APPS
==============

Apps required by Codenerix to work correctly.

.. code:: python

    INSTALLED_APPS = [
        ....,
        'codenerix',
    ]

=======================
AUTHENTICATION_BACKENDS
=======================

Django have a system of authentication, but Codenerix implements its own system. You must add the following two modules:

-  codenerix.authbackend.TokenAuth
-  codenerix.authbackend.LimitedAuth

.. code:: python

    AUTHENTICATION_BACKENDS = (
        'codenerix.authbackend.TokenAuth',                # TokenAuth
        'codenerix.authbackend.LimitedAuth',            # LimitedAuth
    )

=========
TEMPLATES
=========

Finally, is necessary to add to the templates options the two Codenerix context managers:

-  codenerix.context.codenerix
-  codenerix.context.codenerix_js

.. code:: python
    
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                     ..., 
                    'codenerix.context.codenerix',
                    'codenerix.context.codenerix_js',
                ],
            },
        },
    ]
