# How to Read a Repo Quickly

This is a guide for efficiently indexing an unfamiliar codebase.
Use it when you're not sure where to start or what to prioritize reading.

---

## Step 1: Get the File Tree (30 seconds)

```bash
find <path> -type f -not -path '*/node_modules/*' -not -path '*/.git/*' \
  -not -path '*/vendor/*' -not -path '*/__pycache__/*' \
  -not -path '*/dist/*' -not -path '*/build/*' \
  | sort | head -300
```

From the tree alone, you can usually identify:
- Language (`.py`, `.ts`, `.go`, `.rs`, `.java`, etc.)
- Framework (presence of `manage.py` → Django; `nest-cli.json` → NestJS; etc.)
- Architecture style (flat files → simple app; `src/controllers` + `src/services`
  + `src/repositories` → layered architecture)
- Approximate size and complexity

---

## Step 2: Read the README

Always read `README.md` first if it exists. Even bad READMEs usually contain:
- The one-line description (what the service does)
- Setup instructions (how to run it)
- Maybe: architecture notes, API links, runbook links

If there's a `docs/` directory, scan it: `ls docs/` — read any file with a
name like `architecture`, `overview`, `design`, `api`.

---

## Step 3: Read the Manifest File

| File | What it tells you |
|------|-------------------|
| `package.json` | Service name, scripts (`npm run dev`?), main deps |
| `pyproject.toml` / `setup.py` / `requirements.txt` | Python deps, entry points |
| `pom.xml` / `build.gradle` | Java deps, group ID (often the org) |
| `go.mod` | Module name, Go version, external deps |
| `Cargo.toml` | Crate name, Rust version, deps |
| `Gemfile` | Ruby deps, Rails version |

Key things to note from dependencies:
- Web framework (Express, FastAPI, Spring Boot, Gin, etc.)
- ORM/DB client (Prisma, SQLAlchemy, Hibernate, GORM, etc.)
- Queue client (Bull, Celery, kafka-go, etc.)
- Auth library (Passport, PyJWT, spring-security, etc.)
- Notable external services (Stripe, Twilio, SendGrid → tells you what the
  service does)

---

## Step 4: Find the Entry Point

| Framework | Entry point |
|-----------|------------|
| Node/Express | `index.js`, `server.js`, `app.js`, or `src/main.ts` |
| NestJS | `src/main.ts` |
| Python/FastAPI | `main.py`, `app/main.py` |
| Django | `manage.py` + `settings.py`, then look at `urls.py` |
| Go | `cmd/*/main.go` or `main.go` |
| Rust | `src/main.rs` |
| Java/Spring | Class with `@SpringBootApplication` |
| Ruby/Rails | `config/routes.rb` + `app/controllers/` |

The entry point tells you: how the server starts, what middleware is applied,
how routes are registered, what background jobs are started.

---

## Step 5: Find the Routes / API Definition

| Clue | Where to look |
|------|--------------|
| Express | `routes/`, `src/routes/`, or `.use('/api', ...)` in `app.js` |
| FastAPI | `@app.get(...)` decorators, or `routers/` |
| NestJS | `*.controller.ts` files |
| Django | `urls.py` files |
| Rails | `config/routes.rb` |
| Go/Gin | `router.GET(...)` calls |
| OpenAPI spec | `openapi.yaml`, `swagger.json`, `api-spec.yaml` |
| gRPC | `.proto` files |

Read route definitions to get a full list of endpoints before reading the
handler code. This gives you the "menu" of the service.

---

## Step 6: Find the Data Model

| ORM/Tool | Where to look |
|----------|--------------|
| Prisma | `prisma/schema.prisma` |
| Sequelize | `models/` directory |
| SQLAlchemy | `models.py` or `models/` |
| Django ORM | `models.py` in each app |
| Hibernate | Classes with `@Entity` |
| GORM | Structs with `gorm:"..."` tags |
| Mongoose | `schemas/` or `models/` with `Schema(...)` |
| Raw SQL | `migrations/` or `schema.sql` |
| TypeORM | `*.entity.ts` files |

Reading the schema/models is almost always faster than reading migration files —
unless you want to understand the *evolution* of the schema.

---

## Framework Cheat Sheet

### Express / Node
- Routes: `router.get/post/put/delete`
- Middleware: `app.use(...)`
- Auth: look for `passport`, `jwt.verify`, `req.user`

### FastAPI / Python
- Routes: `@router.get/post/...` decorators
- Models: Pydantic classes (these are the request/response shapes)
- DB: `Session` dependency injected into route handlers

### NestJS
- Modules → Controllers → Services (strict layering)
- `@Injectable()` = a service
- `@Controller('/path')` = a route group
- DTOs = request/response shapes (`*.dto.ts`)

### Django
- `views.py` = controllers
- `models.py` = DB models (also the ORM)
- `urls.py` = routing
- `serializers.py` (DRF) = API request/response shapes

### Rails
- `routes.rb` = all routes
- `app/controllers/` = request handlers
- `app/models/` = DB models + business logic (often)
- `app/serializers/` = API response shapes

### Go (common patterns)
- `cmd/` = entry points
- `internal/` = private packages
- `pkg/` = public packages
- Handler → Service → Repository is common layering

---

## Red Flags to Note

When reading a repo, flag these for the learner:

- **No README** — learner should know docs are sparse
- **No tests** — worth calling out
- **Massive files** (>500 lines) — complex, worth slowing down on
- **TODO/FIXME/HACK comments** — highlight interesting ones
- **Hardcoded secrets** (hopefully not!) — call out as a risk
- **Deprecated dependencies** — worth noting
- **README says X but code does Y** — explicitly flag the inconsistency

---

## When the Codebase is Huge

If `find` returns 500+ files:
1. Focus on the `src/` or main source directory only
2. Read only files that match high-signal patterns (controllers, models,
   main entry points)
3. Tell the learner: "This is a large codebase — I'm focusing on the core
   paths. There's a lot more to explore once you have the fundamentals."
4. Consider adding an extra chapter: "Deep Dives" for areas you couldn't
   fully cover
