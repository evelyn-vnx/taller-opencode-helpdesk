# AGENTS.md — Helpdesk (Django 3.1)

## Quick start

```bash
python -m venv .venv && .venv\Scripts\activate
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

## Structure

| Directory | Purpose |
|-----------|---------|
| `core/` | Django project config: settings, root URL conf, ASGI/WSGI |
| `account/` | Custom user model (`MyUser`), `Account` profile, `Rol` (Regular/Agent) |
| `helpdesk/` | Tickets, Comments, Attachments, Vacation, Logs |

**Custom user model**: `account.MyUser` (extends `AbstractUser`), set via `AUTH_USER_MODEL`. Signals in both apps auto-create `Account` and `Rol` on user creation.

**URLs**: `/` → helpdesk, `/account/` → account, `/admin/` → admin.

**Auth**: `LOGIN_URL = 'account:login'`, `LOGIN_REDIRECT_URL = 'helpdesk:dashboard'`.

## Key dependencies

- Django 3.1.6, `django-widget-tweaks`, `Pillow`, `python-dotenv`

## Environment

`python-dotenv` is loaded at the top of `core/settings.py`. Create a `.env` file in the project root for any overrides. `*.env` is gitignored.

## Database

SQLite (`db.sqlite3`), gitignored. After cloning, run `makemigrations` then `migrate`.

- `USE_TZ = False` — no timezone handling.

## Tests

No tests exist (stub files only). `python manage.py test` runs both apps' empty test suites.

## Media

Uploaded files go to `media/account_images/` and `media/attachments/`. Served via Django during dev. The `media/` dir is not gitignored (keep or empty as needed).

## Language

- Django locale: `es-us`
- Inline comments may be in Spanish
- App templates and model verbose names mix Spanish and English

## Conventions

- `Rol` model uses `OneToOneField` to `MyUser` — each user gets exactly one role (is_regular or is_agent)
- Ticket statuses: `Pending`, `Ongoing`, `Closed`
- Vacation statuses: `pending`, `approved`, `declined`
- Ticket URLs use date-based paths: `/ticket/<year>/<month>/<day>/<code>/`