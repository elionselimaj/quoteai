# QuoteAI — Plan

Tick a box when it's really done. Dates are targets, so skipping a day is fine.
How-to for each step (searches and "done when" checks): `docs/plan.md`

## Week 1 — API: text → quote JSON (target 2–8 Nov)

**Setup**
- [x] Python 3.13 + uv installed
- [x] `services/api` created with `uv init --package`
- [x] Repo pushed to GitHub

**Quote math**
- [ ] `uv add --dev pytest ruff`
- [ ] Test: line total = quantity × unit price
- [ ] Test: subtotal = sum of line totals
- [ ] Test: VAT 20%, total = subtotal + VAT
- [ ] `quote_math.py` makes all tests pass (using `Decimal`)
- [ ] `ruff check` clean

**FastAPI**
- [ ] `uv add fastapi`, `uv add --dev httpx`
- [ ] Test: `GET /health` → `{"status": "ok"}`
- [ ] `main.py` with the app and route, test passes
- [ ] App runs locally, `/docs` opens

**First LLM call**
- [ ] `.env` added to `.gitignore`, API key in `.env`
- [ ] `uv add anthropic python-dotenv`
- [ ] `draft_quote(description) -> str` in `llm.py`
- [ ] Test with a fake client (no network)
- [ ] One real call works by hand

**`POST /quote`**
- [ ] Test: description in → `{"quote": ...}` out (LLM faked)
- [ ] Test: empty description → `422`
- [ ] Route works with a real request

**Structured output**
- [ ] Pydantic `LineItem` + `Quote` models
- [ ] LLM returns JSON matching the schema
- [ ] Test: good JSON → `200` with correct totals
- [ ] Test: bad JSON → `502`

**Wrap up**
- [ ] `services/api/README.md` (install, run, test)
- [ ] GitHub Actions runs ruff + pytest, green
- [ ] `docs/design.md` first draft
- [ ] `docs/adr/001-python-fastapi-uv.md`

## Week 2 — Web: Next.js dashboard, the quote streams in (target 9–15 Nov)
- [ ] Next.js app in `apps/web` (TypeScript)
- [ ] Vitest + React Testing Library set up
- [ ] Form: job description → calls API
- [ ] Quote streams in with the Vercel AI SDK
- [ ] Loading and error states

## Week 3 — API: structured output + deploy (target 16–22 Nov)
- [ ] Stronger validation and retry on bad LLM output
- [ ] Streaming endpoint for the web app
- [ ] API deployed (Railway or Fly.io)
- [ ] ADR: hosting choice

## Week 4 — Web: generative UI + deploy (target 23–29 Nov)
- [ ] Quote renders as an editable component
- [ ] Edit line items, totals update
- [ ] Web deployed on Vercel
- [ ] First Playwright E2E test

## Week 5 — API: audio → quote (target 30 Nov–6 Dec)
- [ ] Transcription endpoint (audio file in, text out)
- [ ] Audio → transcript → quote pipeline
- [ ] Tests with sample audio files

## Week 6 — Mobile: voice capture (target 7–13 Dec)
- [ ] Expo app in `apps/mobile`
- [ ] Record a voice note
- [ ] Upload → show quote
- [ ] Jest + React Native Testing Library test

## Week 7 — API: RAG over the price list (target 14–20 Dec)
- [ ] Supabase Postgres + pgvector set up
- [ ] Sample price list loaded with embeddings
- [ ] Quote uses retrieved prices
- [ ] ADR: RAG approach

## Week 8 — Web: show sources (target 21–27 Dec, light week)
- [ ] Each line item shows which price it came from

## Week 9 — API: agent with tools (target 28 Dec–3 Jan)
- [ ] Tools: price lookup, VAT calculation
- [ ] Agent loop builds the quote with the tools
- [ ] Tests for each tool

## Week 10 — Web: agent progress UI (target 4–10 Jan)
- [ ] Show agent steps live
- [ ] Edit and regenerate a quote

## Week 11 — API: observability + evals (target 11–17 Jan)
- [ ] Langfuse tracing on LLM calls
- [ ] `evals/` set of test jobs
- [ ] LLM-judge scores, run in CI

## Week 12 — Polish + demo (target 18–24 Jan)
- [ ] READMEs for every app
- [ ] `docs/design.md` final
- [ ] Demo video
- [ ] Show it to real tradespeople and note their feedback

## Stretch — real-time collaborative editing
- [ ] Yjs or Liveblocks on the quote editor
