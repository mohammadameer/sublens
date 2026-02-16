# sublens

Rails 8 app with Tailwind CSS 4, DaisyUI 5, and PostgreSQL.

## Local setup

- Ruby (see `.ruby-version`), PostgreSQL, Node not required (Tailwind via tailwindcss-rails).
- `bundle install && ./bin/rails db:create && ./bin/dev` (or `./bin/rails server` and in another terminal `./bin/rails tailwindcss:watch`).

## Docker Compose

Runs PostgreSQL and the Rails app (Puma + Tailwind watcher) with one command:

```bash
docker compose up --build
```

- **App:** http://localhost:3000 (or `WEB_PORT` from `.env`)
- **PostgreSQL:** localhost:5433 by default (or `POSTGRES_PORT` from `.env`), user `sublens_app`, password `password`, database `sublens_app_development`

**Avoiding port conflicts:** This project uses host port **5433** for Postgres (not 5432) so you can run other Compose stacks or a local Postgres on 5432. Copy `.env.example` to `.env` and set `POSTGRES_PORT` and `WEB_PORT` per project (e.g. sublens: 5433/3000, other app: 5434/3001). Optionally set `COMPOSE_PROJECT_NAME` so container/network names stay unique.

## Deploy to Railway

**Required environment variables:**
- `DATABASE_URL`: Set to `${{Postgres.DATABASE_PUBLIC_URL}}` (or `${{Postgres.DATABASE_URL}}`) - **This is critical!** Without it, Rails will try to connect via Unix sockets and fail.
- `RAILS_ENV`: Set to `production`
- `SECRET_KEY_BASE` or `RAILS_MASTER_KEY`: From your `config/master.key` file

**Steps:**
1. Add a PostgreSQL service in Railway
2. In your app service → Variables, add `DATABASE_URL` = `${{Postgres.DATABASE_PUBLIC_URL}}`
3. Add `RAILS_ENV=production` and `SECRET_KEY_BASE` (or `RAILS_MASTER_KEY`)
4. The start command in `railway.json` will run `db:prepare` automatically
5. Generate a domain in Settings → Networking
