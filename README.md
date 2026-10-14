# Internship alert system

I built this to spend less time checking company career pages and keep track of internships in one place. It collects job postings, ranks them against a resume, and sends email or mobile notifications for matching roles.

The app uses Python, FastAPI, PostgreSQL/SQLite, and ONNX embeddings. A background collection pipeline handles retries and duplicate URLs, while the dashboard tracks applications and lets me filter by role and location.

## Try it

[Open the recruiter demo](https://internship-alerts-1412.onrender.com/demo/). It uses fictional job listings and sample profiles. The free hosting instance may take about a minute to wake up.

To run the isolated demo locally:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
AI_ENABLED=0 python scripts/demo.py
DEMO_MODE=1 AI_ENABLED=0 uvicorn backend.main:app --host 127.0.0.1 --port 8000
```

Open http://localhost:8000. `AI_ENABLED=0` uses keyword matching without downloading an embedding model. Optional ONNX matching is configured through `.env.example` and `requirements-ai.txt`.

## What I built

- A dashboard with job filtering, resume comparisons, saved applications, and application stages.
- Concurrent collection from company boards, with deduplication and retries.
- Cached semantic matching plus keyword scoring.
- Email and ntfy alerts, with persisted delivery tracking.
- A Google Sheets bridge for application tracking.

My resume records a historical collection run that found 89 eligible roles across 100+ boards in 284 seconds. The 40,000+ figure refers to stored job records at that time, not currently available openings. Those production records are not included in this repository.

## Where to look

- [backend/](backend/): FastAPI routes, authentication, templates, and dashboard.
- [poller/](poller/): sources, normalization, collection, and alert delivery.
- [shared/matching.py](shared/matching.py): keyword and semantic ranking.
- [shared/db.py](shared/db.py): database models and storage.
- [tests/](tests/): regression tests with mocked external services.
- [Implementation notes](docs/IMPLEMENTATION.md): matching, retries, migration, and deployment details.

## Tests and configuration

```sh
python -m pip install -r requirements-dev.txt
AI_ENABLED=0 python -m pytest -q
```

The public copy excludes live databases, resumes, application records, notification credentials, and scheduled production workflows. To enable alerts in your own copy, supply your own environment variables from `.env.example`. The demo does not need notification credentials.
