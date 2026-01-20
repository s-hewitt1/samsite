# Copilot Instructions for `samsite`

These instructions help AI coding agents work productively in this repository.
Keep changes minimal, consistent with the current Django setup, and focused on the task.

## Big Picture
- Framework: Django 6.x with a single project module `config/` (no custom apps yet).
- Entrypoints: `config/asgi.py` (ASGI) and `config/wsgi.py` (WSGI). Admin enabled via `config/urls.py`.
- Settings: `config/settings.py` uses SQLite at `db.sqlite3`, `DEBUG=True`, default middleware and static settings.
- CLI: Use `manage.py` for Django management tasks; `pyproject.toml` + `uv` manage dependencies and execution.
- Non-Django file: `main.py` is a standalone script not used by Django.

## Developer Workflows
- Install dependencies: `uv add <package>` (project uses Python >=3.12).
- Run server (dev): `uv run manage.py runserver`.
- Create app: `uv run manage.py startapp <app_name>`; then add to `INSTALLED_APPS` in `config/settings.py`.
- Migrations: `uv run manage.py makemigrations` and `uv run manage.py migrate` (DB: `db.sqlite3`).
- Admin user: `uv run manage.py createsuperuser` (visit `/admin/`).
- Shell: `uv run manage.py shell` for quick inspection.

## Project Conventions
- Module layout: Keep Django apps as top-level folders alongside `config/` (e.g., `blog/`, `home/`).
- URLs: Root routing is in `config/urls.py`. Include app URLs using `path()`/`include()`.
- Templates: Default Django loader is enabled (`APP_DIRS=True`); app-level `templates/` directories will be discovered.
- Static: Uses `STATIC_URL = 'static/'`. No custom `STATICFILES_DIRS` configured.
- Settings module: Code should assume `DJANGO_SETTINGS_MODULE='config.settings'` (set by `manage.py`).

## Examples and Patterns
- Add a simple homepage:
  - Create `home` app: `uv run manage.py startapp home`.
  - In `home/views.py`: define `def index(request): ...` returning `HttpResponse`.
  - In `config/urls.py`: `from home import views` then `path('', views.index)`.
- Register app:
  - Add `'home'` to `INSTALLED_APPS` in `config/settings.py`.
- Data access: Use Django ORM with the default SQLite DB (`DATABASES['default']`).

## Tests
- `pyproject.toml` declares `pytest` in `dev` group; no tests are present.
- If adding tests, keep them next to apps (e.g., `home/tests/test_views.py`) and run with `uv run pytest`.

## Integration Points
- HTTP: `config/asgi.py`/`config/wsgi.py` are the server callables for deployment.
- Admin: `/admin/` is configured via `config/urls.py`.

## Guardrails for Agents
- Prefer `uv run manage.py ...` over invoking `python` directly.
- Keep changes scoped: update `INSTALLED_APPS`, `config/urls.py`, and app files; avoid global refactors.
- Do not move or rename `config/` without explicit instruction.
- Treat `SECRET_KEY` as dev-only; do not change settings unless asked.

## Key Files
- `pyproject.toml`: dependencies and Python version.
- `manage.py`: Django command runner (sets `DJANGO_SETTINGS_MODULE`).
- `config/settings.py`, `config/urls.py`, `config/asgi.py`, `config/wsgi.py`.
- `db.sqlite3`: development database.
