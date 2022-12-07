# Sentinel

Sentinel is an application written in Python that may be used to perform sentiment analysis on a piece of text. It leverages an external API for this purpose and provides a safe default when the API is unavailable.

## Commands

The following commands describe how the application may be built, run and tested. Note that [Python 3.10](https://docs.python.org/3/whatsnew/3.10.html) and [Poetry](https://python-poetry.org/) are required.

**Setup**

```sh
poetry install
```

**Run**

```sh
uvicorn sentinel.api:app --host 0.0.0.0 --port 8000 --reload
```

From another terminal window, you may query the application with:

```sh
curl -XPOST http://localhost:8000/analyze -H "Content-Type: application/json" -d '{"text": "This is a test."}'
```

**Unit tests and coverage**

```sh
poetry run coverage run -m pytest tests/test_unit.py
poetry run coverage report --fail-under 90
```

**Regression tests**

By default, regression tests run against an application running on `localhost:8000`. However, you may use the `BASE_URL` environment variable to run against a different URL.

```sh
BASE_URL=http://localhost:8000 poetry run pytest tests/test_regression.py
```

**Integration tests**

```sh
poetry run pytest tests/test_integration.py
```

## Deployment pipeline

 - There are currently two branches that correspond to two environments
    - dev (development)
    - main (production)
 - The anticipated workflow is:
    - Development is done on a branch from the dev branch
    - A PR is raised, which is then merged into dev, and the code is deployed to the development environment
    - When deveopers are happy that dev is stable, a PR is raised to merge dev -> main
    - That is then merged, and the resulting code is deployed to production
