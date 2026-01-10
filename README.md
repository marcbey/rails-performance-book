# Rails Performance Book — Sample App

This repository is a Rails 8 (Ruby 3.3.4) sample application used to explore performance techniques: preload vs eager load, pagination, indexing, and caching/queueing with Solid Cache and Solid Queue. It ships with a seeded dataset of films, stores, customers, and rentals plus a small JSON API (`/api/v1`).

## Requirements
- Ruby 3.3.4 with Bundler.
- MySQL (default) or Postgres. Switch adapters by setting `DB_MODE=postgres`; otherwise MySQL credentials from `config/database.yml` are used.
- Redis for Solid Cache/Queue (if you enable those features).

## Setup
```sh
bundle install
bin/setup               # installs gems, prepares DBs, clears logs/tmp, restarts app
bin/rails db:seed       # loads sample data from lib/data.csv (takes a few minutes)
```

## Running the app
```sh
bin/rails server        # boots on http://localhost:3000
bin/rails console       # interactive console
bin/rails db:prepare    # recreates/migrates main, cache, and queue schemas
```
Routes include `/films`, `/stores`, `/customers/:id`, `/happynewyear`, and `/api/v1` endpoints for films, stores, and customer timelines/rentals.

## Tests
Minitest is the default.
```sh
bin/rails test          # full suite (parallelized)
bin/rails test:system   # Selenium system tests
```
Add fixtures under `test/fixtures` and keep tests deterministic (no external network).

## Benchmarking tasks
Several Rake tasks in `lib/tasks` demonstrate performance patterns:
- `bin/rails benchmark_preload_simple` – compares N+1 vs preload/eager load.
- `bin/rails benchmark_paginations` – pagination approaches.
- `bin/rails benchmark_indexes` – index impact on lookups.
- `bin/rails benchmark_narrow_fetches` – narrow selects vs full rows.
Each task prints timing/memory stats; run against a seeded DB.

## Coding notes
- 2-space indentation, snake_case methods, CamelCase classes.
- Keep controllers thin; push serialization to presenters under `app/presenters/api/v1`.
- Prefer ERB for new views; Slim is present but should be consistent within a view subtree.
- Migrations and Solid Cache/Queue migrations live under `db/migrate`, `db/cache_migrate`, and `db/queue_migrate`.

## Deployment hints
- Set `DB_MODE` and database credentials via environment variables or credentials.
- Run `bin/rails assets:precompile` before deploying.
- Avoid committing secrets; use Rails credentials or env vars for keys/passwords.
