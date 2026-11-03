# QuoteAI — project notes for Claude Code

## What this is
Portfolio project (GitHub) to show AI engineering + senior frontend skills.
A tradesperson records a voice note or describes a job, and AI drafts a priced quote.
Portfolio first: no payments, accounts, PDFs or marketing for now.

## How to help me
- Short, simple English. Core idea only, no long explanations.
- Let me write the code first, then review and explain. Don't write everything for me.
- When I'm stuck, give hints and what to search, not full solutions.
- TDD: write the test first, then the code.

## Repo layout (monorepo)
```
quoteai/
  apps/web       Next.js + Vercel AI SDK (streaming, generative UI)
  apps/mobile    React Native (Expo), voice capture
  services/api   Python + FastAPI (LLM calls, agents, RAG, evals)
  evals/         test jobs + LLM-judge scores
  docs/          design.md + adr/
```

## Stack
- Python 3.13, uv, FastAPI, Pydantic
- Postgres + pgvector (Supabase), Anthropic/OpenAI API, Langfuse
- Next.js, Vercel AI SDK, React Native (Expo), TypeScript
- Deploy: Vercel (web), Railway or Fly.io (API)
- Editor: VS Code

## Testing
- API: pytest
- Web: Vitest + React Testing Library
- Mobile: Jest + React Native Testing Library
- E2E: Playwright
- AI quality: LLM-judge evals
- CI: GitHub Actions runs everything on every push

## Docs
- `docs/design.md`: problem, goals/non-goals, architecture, data flow, tech choices, testing, risks
- `docs/adr/NNN-title.md`: one short file per big decision

## 12-week plan (start Mon 2 Nov 2026)
Odd weeks = backend (Track A), even weeks = UI (Track B).

| Week | Starts | Track | Build |
|---|---|---|---|
| 1 | 2 Nov | A | Python + FastAPI, first LLM call, `POST /quote` returns quote JSON |
| 2 | 9 Nov | B | Next.js + AI SDK dashboard, quote streams in |
| 3 | 16 Nov | A | Structured output, validation, deploy API |
| 4 | 23 Nov | B | Generative UI (editable quote), deploy web |
| 5 | 30 Nov | A | Transcription endpoint (audio to quote) |
| 6 | 7 Dec | B | React Native voice capture |
| 7 | 14 Dec | A | RAG over price list (pgvector) |
| 8 | 21 Dec | B | Show sources in UI (light week) |
| 9 | 28 Dec | A | Agent pipeline with tools (price lookup, VAT) |
| 10 | 4 Jan | B | Agent progress UI, edit/regenerate |
| 11 | 11 Jan | A | Langfuse observability + LLM-judge evals |
| 12 | 18 Jan | A+B | Polish, READMEs, demo video, show to tradespeople |
| 13–14 | stretch | B | Real-time collaborative quote editing (Yjs/Liveblocks) |

## Week 1 tasks
- `PLAN.md`: checklist the user ticks themselves. Don't tick boxes for them.
- `docs/plan.md`: how-to for each step (searches, "done when").
- Days get skipped: check the code + tests to see real progress, not the date.

## Progress log
<!-- add a line per day: date, what was done, what's next -->
- 2026-11-02: uv package set up in `services/api`, repo pushed to GitHub (SSH). Next: quote math + pytest.
- 2026-11-03: wrote `docs/plan.md` with week 1 daily steps. Next: Tue tasks.
