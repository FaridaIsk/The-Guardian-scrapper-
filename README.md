# The Guardian Scraper (Content API → SQLite)

A resumable scraper for the **Guardian Content API** that downloads articles from selected sections over a specified date range and stores them in yearly SQLite databases. Designed for long archival runs with resume state, upsert (no duplicates), and rate-limit friendly request handling.

## Features

- Download articles by **section** and **date range** (year → month → paginated API calls)
- Optional full text extraction via `show-fields` (`headline`, `byline`, `bodyText`)
- Saves output into **per-year SQLite files**: `guardian_YYYY.sqlite`
- Stores **resume state** per year: `guardian_YYYY_state.json`
- **Upsert by article id** (safe re-runs without duplicates)
- Handles network errors + server errors with retries/backoff
- Handles `HTTP 429` with `Retry-After` (auto-wait and continue)

## Output (SQLite schema)

Table: `articles`

- `id` (PRIMARY KEY)
- `section_id`, `section_name`, `requested_section`
- `type`
- `web_publication_date`
- `web_title`, `web_url`
- `headline`, `byline`
- `body_text`
- `fetched_at`

Indexes are created on `web_publication_date`, `section_id`, and `requested_section`.

## Requirements

- Python 3.9+
- `requests` (and standard library modules)

Install:
```bash
pip install requests
