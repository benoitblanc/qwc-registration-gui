[![](https://github.com/qwc-services/qwc-registration-gui/workflows/build/badge.svg)](https://github.com/qwc-services/qwc-registration-gui/actions)
[![docker](https://img.shields.io/docker/v/sourcepole/qwc-registration-gui?label=Docker%20image&sort=semver)](https://hub.docker.com/r/sourcepole/qwc-registration-gui)

Registration GUI for QWC Services
=================================

Provides an application form, so users can submit group membership requests.

These membership requests can then be approved or declined by an admin user in the [QWC configuration backend](https://github.com/qwc-services/qwc-admin-gui) (if `GROUP_REGISTRATION_ENABLED` is set to `true` in the `qwc-admin-gui` configuration).


Setup
-----

Uses PostgreSQL connection service `qwc_configdb` (ConfigDB).

Setup PostgreSQL connection service file `pg_service.conf`:

```
[qwc_configdb]
host=localhost
port=5439
dbname=qwc_demo
user=qwc_admin
password=qwc_admin
sslmode=disable
```

Place this file in your home directory, or set the `PGSERVICEFILE` environment variable to point to the file.


Configuration
-------------

### Mailer

Set the `ADMIN_RECIPIENTS` environment variable to a comma separated list of admin users who should be notified of new registration requests (default: `None`).

[Flask-Mail](https://pythonhosted.org/Flask-Mail/) is used for sending mails like admin notifications. These are the available options:

* `MAIL_SERVER`: default ‘localhost’
* `MAIL_PORT`: default 25
* `MAIL_USE_TLS`: default False
* `MAIL_USE_SSL`: default False
* `MAIL_DEBUG`: default app.debug
* `MAIL_USERNAME`: default None
* `MAIL_PASSWORD`: default None
* `MAIL_DEFAULT_SENDER`: default None
* `MAIL_MAX_EMAILS`: default None
* `MAIL_SUPPRESS_SEND`: default app.testing
* `MAIL_ASCII_ATTACHMENTS`: default False

In addition the standard Flask `TESTING` configuration option is used by Flask-Mail in unit tests.

### Translations

Translation strings are stored in a JSON file for each locale in `translations/<locale>.json` (e.g. `en.json`). Add any new languages as new JSON files.

Set the `DEFAULT_LOCALE` environment variable to choose the locale for the application form and notifications (default: `en`).


Usage
-----

Run standalone application:

    python src/server.py

Registration form (if user is signed in):

    http://localhost:5032/register


Docker usage
------------

See sample [docker-compose.yml](https://github.com/qwc-services/qwc-docker/blob/master/docker-compose-example.yml) of [qwc-docker](https://github.com/qwc-services/qwc-docker).


Development
-----------

Install dependencies and run service:

    MAIL_SUPPRESS_SEND=True MAIL_DEFAULT_SENDER=from@example.com ADMIN_RECIPIENTS=admin@example.com uv run src/server.py
