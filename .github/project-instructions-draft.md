# Portfolio System Design & Requirements

## 1. Project Overview
A professional, monolithic Django web application serving as a personal portfolio, tool hub, and blog. The project prioritizes clean architecture, background task processing for heavy processes, and professional deployment standards.

## 2. Tech Stack
- **Framework:** Django (Python)
- **Database:** PostgreSQL (Production), SQLite (Development)
- **Background Tasks:** Celery + Redis
- **Frontend:** Django Templates + Tailwind CSS (via CDN) + HTMX (for dynamic updates)
- **Package Management:** uv + Virtualenv (venv)

## 3. App Architecture (Modular Monolith)
The project is split into functional Django apps:
- `pages`: Static content (Home, About, Metrics dashboard).
- `blog`: Database-driven notes on reading and triathlon progression.
- `showcase`: Portfolio items (e.g., Jet Boat) with Many-to-Many relationships for tech stacks.
- `tools`: Web interfaces for a e.g. Rostering App and a Data Pipeline runner.

## 4. Future Integration Strategy: Data Pipeline
- The Data Pipeline is an external repository.
- **Integration Method:** Installed as a Python package via GitHub URL in `pyproject.toml`.
- **Execution:** Triggered via Django views, executed asynchronously by Celery workers to avoid request timeouts.

## 5. Data Models Schema
### Blog App
- `Post`: Title, Slug, Body (Markdown), Created_at, Status (Draft/Published).

### Showcase App
- `Technology`: Name, Icon/Logo reference.
- `Project`: Title, Description, Image, Many-to-Many(Technology), GitHub_Link, Project_URL.

### Tools App
- `PipelineRun`: Timestamp, Status (Pending/Running/Complete), Result_Summary (JSON), Logs.

## 6. Development Workflow
1. Use `.env` files for all sensitive configurations (SECRET_KEY, DATABASE_URL).
2. Follow Django's Migration system for all schema changes.
3. Use `git` for version control, pushing to GitHub for deployment via Render/Fly.io.

## 7. Performance & Scaling
- Use Django's native caching for metrics to be displayed on home page.
- Database indexes on Slugs and Created_at fields for fast querying.
- Scalable to "zero" using PostgreSQL providers like Neon.