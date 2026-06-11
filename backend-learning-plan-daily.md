# Backend Learning Plan — Day-by-Day Edition

**Stack:** Python · FastAPI · MongoDB · SvelteKit · AI-assisted development
**You:** 5 yrs frontend/UX (SvelteKit, JS/TS)
**Schedule:** 4 sessions/week × ~2.5 hrs (= ~10 hrs/wk). Day 5 each week is an optional catch-up/overflow slot — use it, don't feel guilty about needing it.
**Duration:** 18 weeks of content. Realistically budget **18–20 weeks** — life happens; the buffer is part of the plan, not a failure of it.


**The one rule:** don't start a new week until the current week's Build works *and you understand every line.* If a week takes 6 days, take 6 days.

---

# PHASE 0 — Python fundamentals (Weeks 1–4)

## Week 1 — Syntax, tooling, basics

**Day 1 — Environment + first contact**
- Install Python 3.13, `uv`, VS Code + Python extension (+ Ruff extension).
- Create a project with `uv init`, run a hello-world with `uv run`.
- Official Python Tutorial §3–4: numbers, strings, control flow. Play in the REPL constantly.

**Day 2 — Functions and control flow**
- Tutorial §4 (cont.): functions, default/keyword args, `*args`/`**kwargs`.
- Note the JS differences as you hit them: truthiness, `is` vs `==`, no `let/const`, indentation-as-syntax, scoping (LEGB — no block scope!).
- Exercise: rewrite 3 small JS utility functions you know by heart in Python.

**Day 3 — Collections, first file reading**
- Lists, dicts, tuples, sets — the core four. Tutorial §5.
- Read a CSV with the `csv` module; loop, filter, print.
- AI tip: ask Claude "what would a JS dev get wrong about Python dicts/lists?" — then quiz yourself.

**Day 4 — Build: property CLI v1**
- CLI script: load a CSV of properties, filter by city/price via `argparse` or `sys.argv`, sort, print a table.
- You wrote every line. No AI-generated code this phase — AI explains, you type.

**Day 5 (optional)** — Catch-up; skim Tutorial §1–2 and anything that felt shaky.

## Week 2 — Data structures in depth, files, modules

**Day 1 — Comprehensions + slicing**
- List/dict/set comprehensions, generator expressions, slicing (`[::-1]` etc.), `enumerate`, `zip`, `sorted(key=...)`, lambdas.
- Exercise: rewrite every loop in your Week 1 script as a comprehension where it improves clarity (and notice where it doesn't).

**Day 2 — Files, pathlib, JSON**
- `pathlib.Path`, context managers (`with open(...)`), the `json` module, f-strings in depth.
- Convert your CSV pipeline to also read/write JSON.

**Day 3 — Modules and packages**
- `import` mechanics, packages, `__init__.py`, project layout conventions, `if __name__ == "__main__"`.
- Split your script into modules: `loaders.py`, `filters.py`, `report.py`, `main.py`.

**Day 4 — Build: property CLI v2**
- Group listings by city, compute summary stats (avg price, count), export a JSON summary report.
- AI tip: have Claude review the finished code for un-Pythonic patterns; rewrite the flagged parts yourself.

**Day 5 (optional)** — Catch-up; do a couple of Exercism Python exercises for fluency.

## Week 3 — OOP, type hints, errors

**Day 1 — Classes**
- Classes, `__init__`, methods, `@property`, `__repr__`/`__eq__`, class vs instance attributes.

**Day 2 — Dataclasses + inheritance**
- `@dataclass` (this is 90% of what you'll actually write), inheritance basics, when *not* to use classes.

**Day 3 — Type hints + exceptions (load-bearing day)**
- Type hints in depth: `list[str]`, `dict[str, int]`, `X | None`, `Optional`, `TypedDict`, generics basics. This is Pydantic/FastAPI's foundation — your TS experience maps almost 1:1.
- Exceptions: `try/except/else/finally`, raising, custom exception classes.
- Run `mypy` (or `uv run mypy .`) on your project and fix everything.

**Day 4 — Build: property CLI v3**
- Refactor around a `Listing` dataclass and a `ListingStore` class with typed methods. Proper error handling for bad files/missing fields.
- AI tip: ask Claude to add type hints to an *untyped copy* of your code, then diff against your own attempt.

**Day 5 (optional)** — Catch-up; skim Real Python's OOP article if classes still feel foreign.

## Week 4 — Async + HTTP (milestone week)

**Day 1 — Async concepts**
- `async`/`await`, the event loop, `asyncio.run`, `asyncio.gather`. Map it to what you know: like JS, but the loop only runs when *you* start it, and there's no implicit microtask queue — blocking sync code blocks everything.

**Day 2 — httpx**
- `httpx` sync first, then `AsyncClient`. Headers, params, JSON bodies, timeouts, raising for status.
- Fetch from a free public API (e.g. restcountries.com or a public real-estate/geo API).

**Day 3 — Build: async fetcher**
- Tool that fetches multiple API pages/endpoints concurrently with `asyncio.gather`, transforms the results into your `Listing` model, writes JSON.

**Day 4 — Milestone check**
- Finish + polish. Then the real test: **from a blank folder, no references**, write a small typed program that fetches an API async and writes results. If you can't, repeat what you couldn't recall — then move on.

**Day 5 (optional)** — Buffer. Phase 0 is the most common place to slip; use this day.

**✅ Phase 0 done when:** typed, multi-file, async Python comes out of your fingers without copying syntax.

---

# PHASE 1 — FastAPI (Weeks 5–8)

## Week 5 — FastAPI basics

**Day 1 — First app**
- `uv add "fastapi[standard]"`, first app, run with `fastapi dev` (or `uvicorn`). Explore the auto-generated `/docs` (Swagger UI) — this becomes your main testing tool.
- FastAPI tutorial: First Steps, Path Parameters.

**Day 2 — Params**
- Path params with types/validation, query params, optional params, enums for fixed choices.

**Day 3 — Pydantic models**
- Pydantic v2 `BaseModel`, field types, `Field()` constraints, nested models, `model_dump()`. Note how it's "runtime TypeScript interfaces."

**Day 4 — Build: Listings API v1**
- `GET /listings` (with query filters: city, min/max price) and `GET /listings/{id}` backed by your in-memory `ListingStore`.
- AI tip: have Claude explain any Pydantic v2 pattern you don't get — but write all models yourself.

**Day 5 (optional)** — Catch-up; reread First Steps with fresh eyes.

## Week 6 — Full CRUD, validation, errors

**Day 1 — Writing data**
- POST with request bodies, status codes (`201`, `204`), `response_model` and why response shape ≠ internal shape.

**Day 2 — Update + delete**
- PUT vs PATCH (and `model_dump(exclude_unset=True)` for partial updates), DELETE, `404` handling with `HTTPException`.

**Day 3 — Validation + custom errors**
- `Field` constraints (price ≥ 0, string lengths), custom validators, custom exception handlers, consistent error response shape.
- **Addition to original plan:** add basic pagination params now (`limit`/`offset` or `skip`) — every list endpoint needs them and Week 11/14 build on it.

**Day 4 — Build: hardened CRUD**
- Full CRUD on listings with validation and clean errors.
- AI tip: ask Claude for 20 nasty edge-case payloads (wrong types, boundary values, huge strings, missing fields); fire them via `/docs` and fix what breaks.

**Day 5 (optional)** — Catch-up.

## Week 7 — Structure, DI, middleware, CORS

**Day 1 — Dependency injection**
- `Depends()`: shared query params, "give me the store" dependencies, dependencies with `yield`. This pattern is the heart of FastAPI — take your time.

**Day 2 — Bigger applications**
- `APIRouter`, tags, the "Bigger Applications" project layout (`routers/`, `models/`, `dependencies.py`, `main.py`).

**Day 3 — Middleware + CORS**
- Middleware basics, `CORSMiddleware` configured properly (explicit origins, credentials). Your SvelteKit proxy/auth experience tells you exactly why this matters.
- **Addition:** skim lifespan events (`lifespan=`) — you'll need them for the DB connection in Week 11.

**Day 4 — Build: restructure**
- Reorganize the whole API into routers + dependencies + CORS config.
- AI tip: ask Claude to propose a folder structure, then *critique it* against the official Bigger Applications guide before adopting anything.

**Day 5 (optional)** — Catch-up.

## Week 8 — Auth (milestone week)

**Day 1 — Concepts + password hashing**
- Read the FastAPI Security tutorial intro + OAuth2 password flow.
- Hash passwords with **pwdlib (Argon2)** — *not* passlib; it's unmaintained and FastAPI's docs have moved on.
- Build `POST /users` (register) with hashed storage.

**Day 2 — JWT login**
- `POST /token` login endpoint, issue JWTs (`pyjwt`), expiry, `SECRET_KEY` from env (never hardcode — set the habit now).

**Day 3 — Protected routes**
- `OAuth2PasswordBearer`, a `get_current_user` dependency, protect the write endpoints; listings get an `owner` field.

**Day 4 — Milestone check**
- End-to-end via `/docs`: register → login → create listing → unauthorized write fails with `401`.
- **Addition:** 30 min reading on what you're deferring: refresh tokens, rate limiting, OWASP API Top 10. Just awareness — Week 13 picks up token handling properly.

**Day 5 (optional)** — Buffer.

**✅ Phase 1 done when:** you can stand up a structured, validated, JWT-protected CRUD API from a blank folder.

---

# PHASE 2 — MongoDB + data layer (Weeks 9–11)

## Week 9 — Mongo fundamentals

**Day 1 — Atlas + Compass**
- Create a free Atlas M0 cluster, connect with Compass, load the Atlas sample datasets.
- Start MongoDB University's **Python Developer Learning Path** (it uses PyMongo — exactly your stack).

**Day 2 — CRUD by hand**
- In `mongosh` and Compass: `insertOne/Many`, `find` with operators (`$gt`, `$in`, `$regex`), `updateOne` with `$set`, `deleteOne`. Projection and sort.

**Day 3 — University path units**
- Continue the learning path: connection strings, PyMongo CRUD labs.

**Day 4 — Build: intuition reps**
- Create your own `listings` collection and run every query your API will need, by hand, in the shell. If you can't write it in `mongosh`, you don't understand what the ODM will do for you.
- AI tip: ask Claude to explain embed-vs-reference using *your* listings domain (listings, users, saved searches, photos).

**Day 5 (optional)** — Catch-up on University units.

## Week 10 — Modeling, indexes, aggregation

**Day 1 — Embed vs reference**
- Schema design patterns; the rules of thumb (data accessed together lives together; unbounded arrays are a bug).

**Day 2 — Design your schema**
- Design the real listings + users schema on paper first. Decide what's embedded (photos? address?) and what's referenced (owner).

**Day 3 — Indexes**
- Single-field, compound, `explain()` to verify usage. Add indexes for every field you filter/sort on.

**Day 4 — Aggregation**
- `$match`, `$group`, `$sort`, `$project`, `$limit` — build "avg price per city" and "listing count per owner" pipelines.
- AI tip: have Claude review your schema for anti-patterns; make it justify each criticism against MongoDB's modeling docs.

**Day 5 (optional)** — Finish remaining University path units.

## Week 11 — Wire MongoDB into FastAPI (milestone week)

**Day 1 — Beanie setup**
- `uv add beanie` (Beanie ≥2.0 — runs on PyMongo Async; Motor is EOL and should appear nowhere in your code).
- `init_beanie` inside a FastAPI lifespan handler; convert `Listing` to a Beanie `Document`. Note the payoff: DB model and API model are both Pydantic.

**Day 2 — Reads**
- Replace in-memory GETs: `find`, `find_one`, `PydanticObjectId`, filters from query params, skip/limit pagination *at the database level*.

**Day 3 — Writes**
- `insert`, `save`/`set` updates, deletes. Wire your Week 10 indexes into the Document `Settings`.

**Day 4 — Milestone check**
- Users live in Mongo too; auth works against the DB. Restart the server — everything persists. Old TestDriven.io Beanie tutorials may still show Motor imports; treat that as a history lesson, not instructions.

**Day 5 (optional)** — Buffer.

**✅ Phase 2 done when:** the API reads/writes Atlas and survives a restart.

---

# PHASE 3 — Full-stack integration (Weeks 12–15)

*Your home turf. Lighter notes — these days you can self-direct.*

## Week 12 — Architecture + scaffold

- **Day 1:** Decide repo layout (monorepo with `/frontend` + `/backend` is fine). Scaffold SvelteKit. Env/secrets strategy on both sides (`.env` + `$env/static/private` vs FastAPI `pydantic-settings`).
- **Day 2:** Call FastAPI from SvelteKit `load` functions server-side (no CORS pain) — decide and document when you'd ever call it from the browser instead.
- **Day 3:** Listings list view reading from the live API.
- **Day 4:** Error/loading handling for API failures; write a `CLAUDE.md` for the repo with your conventions. From here on, Claude Code can scaffold boilerplate — you can now judge its output.

## Week 13 — Auth end-to-end

- **Day 1:** Pick the token strategy: store the JWT in an **httpOnly cookie set by a SvelteKit server route** (not localStorage). Build the login form + server action.
- **Day 2:** `hooks.server.ts` reads/validates the cookie, populates `locals.user`; protect routes; logged-in/out UI states.
- **Day 3:** Signup flow; token expiry handling (a short-lived access token + re-login is acceptable here; note refresh tokens as the production upgrade).
- **Day 4:** Ownership: users can create/edit/delete *their own* listings only — enforce it **in the API** (the frontend hiding a button is UX, not security).

## Week 14 — Core features

- **Day 1:** Search + filters: backend query params → frontend filter UI with URL state.
- **Day 2:** Pagination end-to-end (you built the API side in Weeks 6/11).
- **Day 3:** Create/edit forms with client-side validation mirroring your Pydantic rules; show server validation errors properly.
- **Day 4:** Detail views, image handling, bug-fix sweep.

## Week 15 — Polish (milestone week)

- **Day 1:** Loading/empty/error states everywhere.
- **Day 2:** Optimistic UI where it helps; config cleanup.
- **Day 3:** Accessibility + responsive pass (your easy mode).
- **Day 4:** Hand it to someone cold and watch them use it. Fix the friction you see. **Done when a stranger needs no explanation.**

---

# PHASE 4 — Production + deployment (Weeks 16–18)

## Week 16 — Testing

- **Day 1:** `pytest` fundamentals: test discovery, asserts, parametrize. Unit-test your pure logic.
- **Day 2:** FastAPI `TestClient`, fixtures, testing CRUD endpoints.
- **Day 3:** Auth tests (register/login/protected routes). DB strategy: point tests at a separate test database and wipe it between tests via a fixture — simpler and more honest than mocking Beanie.
- **Day 4:** Coverage of core flows. AI tip: now AI-drafting tests is legitimate — generate, then verify *each test actually asserts something meaningful* and watch it fail when you break the code.

## Week 17 — Docker + config

- **Day 1:** Dockerfile for FastAPI (official FastAPI-in-containers guide; multi-stage with `uv`).
- **Day 2:** `docker-compose`: backend + local Mongo for dev.
- **Day 3:** Environment-based config with `pydantic-settings`, structured logging, secrets via env only.
- **Day 4:** Full stack runs via `docker compose up` on a clean machine.

## Week 18 — Deploy + write-up (final milestone)

- **Day 1:** Deploy the backend to **Render (free tier)** pointed at Atlas. (Render free services sleep after 15 min idle — fine for a portfolio. If you want always-on, Railway Hobby at $5/mo is the upgrade. Fly.io no longer has a free tier.)
- **Day 2:** Deploy the SvelteKit frontend (Vercel/Netlify free, or Render); production env vars, CORS locked to the real origin.
- **Day 3:** GitHub Actions: lint (Ruff) + test on every push.
- **Day 4:** README/case study: architecture diagram, decisions and trade-offs, what you'd do next. This document is what gets you full-stack interviews.

**✅ Plan done when:** live URL, tests green in CI, documented.

---

# AFTER THE PLAN — the honest gaps (Weeks 19+)

These were missing from the original plan. None block the 18 weeks, but for *employable full-stack*, queue them next:

1. **SQL + PostgreSQL (the big one).** Most backend jobs are SQL-first; Mongo-only is a real CV gap. After Week 18: SQLBolt for query fluency, then rebuild the Listings data layer on Postgres + SQLModel (Pydantic-based, same author as FastAPI — minimal new concepts). ~3–4 weeks. The Udemy FastAPI course doubles as SQL exposure since it's SQLAlchemy-based.
2. **Background work:** FastAPI `BackgroundTasks`, then a real queue (e.g. ARQ or Celery) for emails/imports.
3. **Caching + rate limiting:** Redis basics, `slowapi`.
4. **Refresh tokens** done properly; password reset flow.
5. **Observability:** Sentry free tier on both ends; structured logs you can actually search.
6. **WebSockets** (FastAPI makes this easy) — live updates suit a listings app.

---

# RESOURCES BY PHASE (verified June 2026)

## Phase 0 — Python
| Resource | Type | URL |
|---|---|---|
| Official Python Tutorial (your spine) | Free | https://docs.python.org/3/tutorial/ |
| uv docs (env + packages) | Free | https://docs.astral.sh/uv/ |
| CS50P — Harvard (if you want structure/video) | Free | https://cs50.harvard.edu/python/ |
| Exercism Python track (drill fluency) | Free | https://exercism.org/tracks/python |
| Real Python — async walkthrough | Free article | https://realpython.com/async-io-python/ |
| httpx docs | Free | https://www.python-httpx.org/ |
| mypy cheat sheet (type hints) | Free | https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html |
| Real Python membership | Paid ~$25/mo | https://realpython.com/ |
| *Fluent Python*, 2nd ed. (depth, much later) | Paid book | https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/ |

## Phase 1 — FastAPI
| Resource | Type | URL |
|---|---|---|
| **Official FastAPI Tutorial** (primary spine) | Free | https://fastapi.tiangolo.com/tutorial/ |
| FastAPI Security section (Week 8) | Free | https://fastapi.tiangolo.com/tutorial/security/ |
| Pydantic v2 docs | Free | https://docs.pydantic.dev/latest/ |
| pwdlib (password hashing — replaces passlib) | Free | https://frankie567.github.io/pwdlib/ |
| FastAPI — The Complete Course 2026 (Roby/Darby, Udemy) | Paid ~$10–15 on sale | https://www.udemy.com/course/fastapi-the-complete-course/ |

*Udemy note: never pay sticker price — it cycles to ~$10–15 every few weeks. Also note the course is SQLAlchemy/SQL-based, which makes it double as your SQL gap-filler later, but it won't match your Mongo stack week-for-week.*

## Phase 2 — MongoDB
| Resource | Type | URL |
|---|---|---|
| **MongoDB University — Python Developer Learning Path** | Free + certificate | https://learn.mongodb.com/learning-paths/mongodb-python-developer-path |
| MongoDB data modeling docs | Free | https://www.mongodb.com/docs/manual/data-modeling/ |
| Beanie ODM docs | Free | https://beanie-odm.dev/ |
| PyMongo Async migration guide (why Motor is gone) | Free | https://www.mongodb.com/docs/languages/python/pymongo-driver/current/reference/migration/ |
| TestDriven.io — FastAPI + Beanie CRUD tutorial | Free article | https://testdriven.io/blog/fastapi-beanie/ |
| MongoDB Atlas free tier | Free | https://www.mongodb.com/cloud/atlas/register |

*TestDriven caveat: if the article still shows Motor imports, substitute Beanie 2.x / PyMongo Async per the Beanie docs.*

## Phase 3 — Full-stack
| Resource | Type | URL |
|---|---|---|
| SvelteKit docs (load functions, hooks, env) | Free | https://svelte.dev/docs/kit |
| FastAPI CORS guide | Free | https://fastapi.tiangolo.com/tutorial/cors/ |
| Lucia-learning: sessions/auth concepts for SvelteKit | Free | https://lucia-auth.com/ |

## Phase 4 — Testing, Docker, deploy
| Resource | Type | URL |
|---|---|---|
| FastAPI Testing docs | Free | https://fastapi.tiangolo.com/tutorial/testing/ |
| pytest docs | Free | https://docs.pytest.org/ |
| FastAPI in Containers (official Docker guide) | Free | https://fastapi.tiangolo.com/deployment/docker/ |
| Docker Get Started | Free | https://docs.docker.com/get-started/ |
| Render — deploy FastAPI guide | Free | https://render.com/docs/deploy-fastapi |
| GitHub Actions docs | Free | https://docs.github.com/en/actions |
| **TestDriven.io — TDD with FastAPI & Docker** (course) | Paid, one-time | https://testdriven.io/courses/tdd-fastapi/ |

## After the plan — SQL gap
| Resource | Type | URL |
|---|---|---|
| SQLBolt (interactive SQL basics) | Free | https://sqlbolt.com/ |
| PostgreSQL Tutorial | Free | https://www.postgresqltutorial.com/ |
| SQLModel docs (FastAPI author's ORM) | Free | https://sqlmodel.tiangolo.com/ |

---

# Milestone checklist

- [ ] **Week 4** — Typed async Python program that calls an API, written without references
- [ ] **Week 8** — Structured, validated, JWT-protected FastAPI CRUD API (pwdlib + Argon2)
- [ ] **Week 11** — API persisting to MongoDB Atlas via Beanie 2.x (PyMongo Async)
- [ ] **Week 15** — Polished full-stack SvelteKit + FastAPI + Mongo app a stranger can use
- [ ] **Week 18** — Tested, containerized, deployed on Render, CI green, documented
- [ ] **Weeks 19+** — SQL/Postgres rebuild of the data layer (the employability gap-filler)
