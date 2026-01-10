# Repository Guidelines

## Project Structure & Module Organization
- Rails 8 app on Ruby 3.3.4. Core code lives in `app/` (`controllers`, `models`, `views`, `jobs`, `mailers`, `helpers`, `presenters`). API presenters are under `app/presenters/api/v1/`.
- Front-end assets and import maps live in `app/assets` and `app/javascript`; Hotwire (Turbo/Stimulus) is available.
- Configuration sits in `config/` (routes, environments, credentials). `config/database.yml` supports MySQL or Postgres via `DB_MODE`.
- Database migrations are in `db/migrate`; Solid Cache and Solid Queue use `db/cache_migrate` and `db/queue_migrate`. Seeds belong in `db/seeds.rb`.
- Tests use the default Minitest layout in `test/` with fixtures in `test/fixtures`.

## Build, Test, and Development Commands
- `bin/setup` — install gems, prepare DBs (respects `DB_MODE`), clear logs/tmp, restart app.
- `bin/rails db:prepare` — create/migrate main, cache, and queue databases.
- `bin/rails server` — start the app (defaults to port 3000).
- `bin/rails test` — run the full Minitest suite; use `bin/rails test test/models/user_test.rb` for a file or `... TESTOPTS="--name test_example"` for a test.
- `bin/rails test:system` — run system/feature tests via Selenium.
- `bin/rails assets:precompile` — build assets for production-like runs.

## Coding Style & Naming Conventions
- Follow standard Ruby/Rails style: 2-space indentation, snake_case for methods/variables, CamelCase for classes/modules, and use frozen string literals where practical.
- Keep controllers RESTful and thin; push presentation logic into `app/presenters` or helpers, and data access into models.
- Prefer ERB templates; Slim is available but use consistently within a view subtree.
- Name migrations descriptively (`timestamp_create_orders.rb`); API namespaces should match version folders (`Api::V1`).

## Testing Guidelines
- Minitest is the default; every change should have unit coverage in `test/models` or `test/controllers`, with integration in `test/integration` and end-to-end in `test/system` when UI behavior changes.
- Use fixtures for predictable data; add new ones under `test/fixtures` and reference with symbols.
- Keep tests deterministic (no external network); clean up data with transactional tests or explicit teardown when adding non-DB resources.

## Commit & Pull Request Guidelines
- Commit messages follow the existing history: short, imperative or gerund-style summaries (e.g., "Add Redis", "Fix checkout flow"). One focused change per commit when possible.
- PRs should include: a concise description, linked issues, screenshots for UI changes, notes on migrations or DB mode assumptions, and how you validated (commands run).

## Environment & Configuration Tips
- Set `DB_MODE=postgres` to switch adapters; defaults to MySQL. Ensure matching credentials in `config/database.yml` or env vars.
- Solid Cache and Solid Queue use their own schemas; run `bin/rails db:prepare` after adding jobs or cache migrations.
- Avoid committing secrets; prefer Rails credentials or environment variables for keys, URLs, and passwords.
