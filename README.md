# HealthLink

HealthLink is a Django-based healthcare portal prototype that gives doctors and
patients separate sign-up, login, and dashboard flows. Patients can request
appointments with doctors, and both user types can exchange messages.

The application uses Django templates for its web pages and MySQL for
persistence. It is a learning/demo project, not a production-ready clinical
system.

## Features

- Separate doctor and patient registration and login
- Session-based doctor and patient dashboards
- Patient appointment requests and doctor appointment lists
- Message sending and a JSON endpoint for retrieving sent and received messages
- Django admin registration for the project's models

The data models also define medicines and patient orders. The current URL
configuration does not provide a medicine catalog or order-management flow.

## Technology

- Python
- Django 5.0.6
- MySQL
- Django HTML templates

## Project structure

```text
miniapp/
├── manage.py
├── healthlink/
│   ├── migrations/
│   ├── models.py
│   ├── urls.py
│   └── views.py
├── miniapp/
│   ├── settings.py
│   └── urls.py
└── templates/
```

## Prerequisites

- Python installed and available as `python`
- A running MySQL server
- A MySQL database driver supported by Django (for example, `mysqlclient`)

The repository does not currently include a dependency manifest. Install Django
and the MySQL driver in a virtual environment:

```powershell
cd miniapp
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install "Django==5.0.6" mysqlclient
```

If PowerShell blocks virtual-environment activation, use the activation command
appropriate for your shell, or invoke the environment's Python executable
directly.

## Database configuration

Create a MySQL database for the application, then configure the database
connection in `miniapp/miniapp/settings.py` to match your local MySQL setup.
The current settings expect a database named `healthlink`.

**Security warning:** the checked-in Django settings contain a development
secret key and database credentials, and the current login views compare
passwords directly rather than using Django's password-hashing facilities.
Rotate any credentials that have been exposed, do not reuse them, and do not
use real patient information with this project. Before deployment, move secrets
out of source control and implement secure authentication and production-ready
Django settings.

## Run locally

From the `miniapp` directory, after installing dependencies and configuring
MySQL:

```powershell
python manage.py migrate
python manage.py runserver
```

Open [http://127.0.0.1:8000/](http://127.0.0.1:8000/) in your browser. The root
page links to the doctor and patient login screens; each login screen also
provides its corresponding sign-up flow.

To access the registered models in Django admin, create a Django administrator
account and visit [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/):

```powershell
python manage.py createsuperuser
```

## Main routes

| Route | Purpose |
| --- | --- |
| `/` | Portal landing page |
| `/doc_signup/`, `/doc_login/` | Doctor registration and login |
| `/doc_home/` | Doctor dashboard |
| `/patient_signup/`, `/patient_login/` | Patient registration and login |
| `/patient_home/` | Patient dashboard and appointment request form |
| `/make_appointment/` | Submit an appointment request |
| `/send_message/`, `/get_messages/` | Send messages and retrieve messages as JSON |
| `/admin/` | Django administration |

## Current limitations

HealthLink is an early-stage prototype. Authentication, authorization,
validation, privacy, and deployment hardening need further work before it is
suitable for use beyond local demonstrations. No automated test coverage or
dependency manifest is currently provided.
