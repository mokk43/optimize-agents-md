# Before/After Optimization Examples

## Example 1: Bloated CLAUDE.md to Lean AGENTS.md

### Before (CLAUDE.md -- 350+ lines, mixed concerns)

```markdown
# CLAUDE.md

This is a Next.js 15 project with TypeScript. We use Tailwind CSS for styling.
The project uses PostgreSQL with Prisma ORM.

When you write code, always make sure to follow best practices and write clean
code. Use meaningful variable names and add comments where necessary. We believe
in writing maintainable and scalable code that follows industry standards.

## Database Schema

The users table has the following columns:
- id (uuid, primary key)
- email (varchar, unique, not null)
- name (varchar, not null)
- created_at (timestamp, default now())
- updated_at (timestamp)
- role (enum: admin, user, moderator)
- ... [50 more lines of schema details]

## API Routes

All API routes are in src/app/api/. Each route should handle errors properly.
Use try-catch blocks and return appropriate status codes.

POST /api/users - creates a user
GET /api/users - lists users with pagination
GET /api/users/:id - gets a single user
... [40 more lines of route documentation]

## Component Guidelines

Components should be small and reusable. Use React Server Components by
default. Only use 'use client' when you need interactivity.

Always use the cn() utility for conditional classnames.

Here's how our Button component works:
... [30 lines of component implementation details]

## Git Workflow

We use conventional commits. Run tests before committing.
Don't push directly to main. Create feature branches.

## Environment Variables

DATABASE_URL=postgresql://...
NEXTAUTH_SECRET=...
... [15 lines of env var documentation]

## Testing

We use Vitest and Testing Library. Run `pnpm test` to run tests.
Always write tests for new features.

## Random Notes

- The caching layer uses unstable_cache from Next.js
- We had a bug with middleware once, be careful with edge runtime
- John prefers functional components
- The old API was in /pages/api but we migrated
... [30 more lines of scattered notes]
```

### Issues Found

| # | Issue | Severity |
|---|-------|----------|
| 1 | Project facts and behavioral rules mixed in same sections | CRITICAL |
| 2 | Full database schema inlined (progressive context) | HIGH |
| 3 | Complete API documentation inlined (progressive context) | HIGH |
| 4 | File exceeds 300 lines | HIGH |
| 5 | Vague rules: "follow best practices", "write clean code" | HIGH |
| 6 | Environment variables with potential secrets | CRITICAL |
| 7 | Scattered personal notes mixed with project conventions | MEDIUM |
| 8 | Component implementation details at top level | MEDIUM |
| 9 | No directory map | MEDIUM |
| 10 | Cursor/Claude-only, no multi-tool compatibility | LOW |

### After (AGENTS.md -- 120 lines, clean separation)

```markdown
# MyApp

## Overview

Full-stack web application for [purpose]. Server-rendered Next.js SPA with
REST API and PostgreSQL persistence.

## Tech Stack

- **Language**: TypeScript 5.x
- **Framework**: Next.js 15 (App Router)
- **Database**: PostgreSQL 16 via Prisma ORM
- **Styling**: Tailwind CSS 4.x with cn() utility
- **Auth**: NextAuth.js v5
- **Testing**: Vitest + Testing Library
- **Package Manager**: pnpm 9.x

## Architecture

```
src/
├── app/           # Next.js App Router pages and API routes
│   ├── api/       # REST API endpoints
│   └── (routes)/  # Page routes with layouts
├── components/    # Reusable UI components (React Server Components default)
├── lib/           # Shared utilities, db client, auth config
├── services/      # Business logic layer (called by API routes)
├── types/         # Shared TypeScript types and Zod schemas
└── tests/         # Test files mirroring src/ structure
```

Server Components by default. Add 'use client' only for interactive components.
All data access goes through the services/ layer.

## Commands

```bash
pnpm install              # Install dependencies
pnpm dev                  # Dev server on localhost:3000
pnpm test                 # Run all tests
pnpm lint                 # ESLint + Prettier check
pnpm build                # Production build
pnpm prisma migrate dev   # Run pending migrations
```

## Naming Conventions

- **Files**: kebab-case (user-profile.tsx, create-user.ts)
- **Components**: PascalCase (UserProfile, CreateUserForm)
- **Functions**: camelCase, verb-first (getUserById, validateEmail)
- **Types/Interfaces**: PascalCase (CreateUserDto, UserRole)
- **Database**: snake_case tables and columns (user_profiles.created_at)
- **API Routes**: kebab-case paths (/api/user-profiles)

## Code Patterns

- Conditional classnames via cn() from lib/utils
- Error responses use AppError class from lib/errors
- All Zod schemas co-located with their route in a schema.ts file
- Server actions over API routes for mutations when possible

---

## Working Norms

- Ask before refactoring or changing public API contracts
- Prefer minimal diffs -- change only what the task requires
- Run `pnpm test` and `pnpm lint` before declaring work complete
- Do not add dependencies without approval
- Do not rewrite unrelated code
- Conventional commits: type(scope): description
- Feature branches, never push directly to main

---

## Progressive Context

- `src/app/api/AGENTS.md` -- API design rules, error handling patterns
- `src/components/AGENTS.md` -- Component conventions, prop patterns
- `docs/schema.md` -- Full database schema documentation
- `docs/specs/` -- Implementation specs from planner agents
```

### Also Generated

**CLAUDE.md** (shim):
```markdown
# Project Instructions
All project instructions live in AGENTS.md.
See @AGENTS.md
```

**`.agents/agents/nextjs-planner.md`** -- Planner with Next.js App Router expertise
**`.agents/agents/nextjs-coder.md`** -- Executor with full-stack Next.js patterns
**`.agents/agents/code-reviewer.md`** -- Read-only reviewer

---

## Example 2: .cursorrules Conversion

### Before (.cursorrules -- Cursor-only)

```
You are an expert Python developer using FastAPI and SQLAlchemy.

Always use type hints. Use Pydantic v2 models for all request/response schemas.
Use async/await for all database operations.

When writing tests, use pytest with httpx AsyncClient.
Run tests with: pytest -xvs

Project structure:
app/
  api/       - API route handlers
  models/    - SQLAlchemy models
  schemas/   - Pydantic schemas
  services/  - Business logic
  core/      - Config, security, dependencies

Use conventional commits. Don't modify alembic migrations manually.
```

### After (AGENTS.md)

```markdown
# FastAPI Service

## Overview

REST API service built with FastAPI and SQLAlchemy 2.x. Async-first design
with PostgreSQL backend.

## Tech Stack

- **Language**: Python 3.12
- **Framework**: FastAPI 0.115
- **ORM**: SQLAlchemy 2.x (async)
- **Validation**: Pydantic v2
- **Migrations**: Alembic
- **Testing**: pytest + httpx AsyncClient
- **Package Manager**: uv

## Architecture

```
app/
├── api/        # Route handlers (thin controllers)
├── models/     # SQLAlchemy ORM models
├── schemas/    # Pydantic request/response schemas
├── services/   # Business logic (called by route handlers)
└── core/       # Config, security, dependency injection
```

Route handlers are thin: validate input via Pydantic, call service layer,
return response. All DB operations use async/await.

## Commands

```bash
uv sync                          # Install dependencies
uvicorn app.main:app --reload    # Dev server
pytest -xvs                      # Run tests
ruff check . && ruff format .    # Lint and format
alembic upgrade head             # Run migrations
```

## Naming Conventions

- **Files**: snake_case (user_service.py, create_user.py)
- **Functions**: snake_case, verb-first (get_user_by_id, validate_token)
- **Classes**: PascalCase (UserService, CreateUserSchema)
- **DB Tables**: snake_case plural (users, user_roles)

## Code Patterns

- Type hints on all function signatures and return values
- Pydantic v2 models for all API schemas (not dataclasses)
- Dependency injection via FastAPI Depends()
- Never modify Alembic migration files manually after generation

---

## Working Norms

- Run `pytest -xvs` before declaring work complete
- Conventional commits: type(scope): description
- Do not add dependencies without approval
- Prefer minimal diffs
```

---

## Example 3: Minimal AGENTS.md Enrichment

### Before (AGENTS.md -- 15 lines, too sparse)

```markdown
# AGENTS.md

This is a React app. Use TypeScript.

Run `npm test` for tests.
Run `npm start` for dev.

Don't break existing tests.
```

### Issues Found

| # | Issue | Severity |
|---|-------|----------|
| 1 | Missing tech stack details (React version, state management, styling) | HIGH |
| 2 | No directory map | HIGH |
| 3 | No naming conventions | MEDIUM |
| 4 | No code patterns documented | MEDIUM |
| 5 | Single vague working norm | HIGH |
| 6 | Missing lint command | MEDIUM |

### After

The skill would scan the codebase to infer:
- React 18 with Vite from `package.json`
- Redux Toolkit from dependencies
- CSS Modules from file patterns
- Jest + RTL from test config
- ESLint + Prettier from config files

Then generate a complete AGENTS.md following the canonical structure with
all inferred values filled in, working norms expanded, and sub-agent
definitions created for the React/TypeScript stack.
