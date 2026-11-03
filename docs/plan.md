# QuoteAI — working plan

Ticks live in `PLAN.md`. This file is the how-to: steps, searches, "done when".

All commands run from `services/api` unless noted.

Dates are targets, not deadlines. Days get skipped. The real progress is whatever the code, tests and ticked boxes show.

---

## Week 1 (2–8 Nov) — Track A: API that turns a job description into a quote

Goal by Sunday: `POST /quote` takes text like "replace kitchen tap, 2 hours" and returns validated quote JSON from an LLM. Tests and CI are green.

### Mon 2 Nov — setup
- Python 3.13 + uv installed
- `services/api` created with `uv init --package`
- Repo pushed to GitHub

### Tue 3 Nov — Python basics, using the quote domain
Learn type hints, dataclasses, Decimal and pytest by building the quote math.

- `uv add --dev pytest ruff`
- Write the tests first in `tests/test_quote_math.py`:
  - the line total is quantity × unit price
  - the subtotal is the sum of the line totals
  - VAT is 20% of the subtotal, and the total is subtotal + VAT
- Make them pass in `src/api/quote_math.py`: a `LineItem` dataclass and a `Quote` dataclass with `subtotal`, `vat` and `total`
- `uv run pytest` is green and `uv run ruff check` is clean

Search: "python dataclass", "python Decimal money rounding", "pytest getting started", "uv add dev dependency"

Done when: 3+ tests pass.

### Wed 4 Nov — FastAPI + `GET /health`
- `uv add fastapi` and `uv add --dev httpx`
- Test first: `tests/test_health.py` uses `TestClient`, and `GET /health` returns `200` with `{"status": "ok"}`
- Write `src/api/main.py` with the FastAPI app and the route
- Run it with `uv run fastapi dev src/api/main.py` and open http://localhost:8000/docs

Search: "FastAPI first steps", "FastAPI testing TestClient"

Done when: the test passes and `/docs` loads.

### Thu 5 Nov — first LLM call
- Get an API key and put it in `services/api/.env` (add `.env` to `.gitignore` first!)
- `uv add anthropic python-dotenv`
- Write `src/api/llm.py` with `draft_quote(description: str) -> str`
- Test with a fake client, so no real API call happens in tests
- Run it once by hand with a real job description

Search: "Anthropic Python SDK quickstart", "pytest monkeypatch", "dependency injection python function"

Done when: the real call returns a quote as text and the test passes with no network.

### Fri 6 Nov — `POST /quote` (text in, text out)
- Test first: post `{"description": "..."}` and get back `200` with `{"quote": "..."}` (LLM faked)
- Add a Pydantic request model and the route, which calls `draft_quote`
- Test an empty description: it should return `422`
- Try it with curl or `/docs`

Search: "FastAPI request body", "FastAPI dependency_overrides"

Done when: the tests pass and a real request works.

### Sat 7 Nov — structured JSON output
- Turn the Tue dataclasses into Pydantic models: `LineItem` and `Quote`
- Make the LLM return JSON that matches the `Quote` schema
- Validate it with `Quote.model_validate`, and return `502` if the LLM sends bad JSON
- Tests: good fake JSON gives `200` with a quote; bad fake JSON gives `502`

Search: "Pydantic v2 models", "Anthropic structured outputs" / "tool use JSON schema"

Done when: `POST /quote` returns typed quote JSON with correct totals.

### Sun 8 Nov — wrap up
- `services/api/README.md`: how to install, run and test
- GitHub Actions: `.github/workflows/api.yml` runs `ruff` + `pytest` on every push
- `docs/design.md` first draft (half a page: problem, goals, non-goals, architecture sketch)
- `docs/adr/001-python-fastapi-uv.md`: why this stack
- Plan week 2 here

Search: "astral-sh/setup-uv GitHub Action"

Done when: CI is green on GitHub.

---

## Week 2 (9–15 Nov) — Track B: Next.js dashboard, the quote streams in
Broken into daily steps on Sun 8 Nov.
