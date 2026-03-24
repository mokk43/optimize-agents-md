# Canonical AGENTS.md Structure

Reference template for an optimized AGENTS.md file. Adapt section content to
the actual project -- remove sections that don't apply, add project-specific
ones as needed. Target: under 200 lines total.

---

# [Project Name]

## Overview

[1-3 sentences: what the system is, who it serves, primary runtime/language.]

## Tech Stack

- **Language**: [e.g., TypeScript 5.x, Python 3.12]
- **Framework**: [e.g., Next.js 15, FastAPI 0.115]
- **Database**: [e.g., PostgreSQL 16 via Prisma ORM]
- **Styling**: [e.g., Tailwind CSS 4.x]
- **Testing**: [e.g., Vitest, pytest]
- **Package Manager**: [e.g., pnpm 9.x, uv]

## Architecture

```
repo-root/
├── src/              # Application source
│   ├── components/   # Reusable UI components
│   ├── lib/          # Shared utilities and helpers
│   ├── routes/       # Page routes / API endpoints
│   └── stores/       # State management
├── tests/            # Test files mirroring src/ structure
├── docs/             # Architecture docs, ADRs
│   └── specs/        # Implementation specs (planner output)
├── scripts/          # Build and automation scripts
└── .agents/          # AI agent definitions
    └── agents/       # Sub-agent configs
```

[1-2 sentences on primary architectural pattern: e.g., "Server-rendered SPA
with REST API. All data access goes through the service layer."]

## Commands

```bash
# Install dependencies
[exact install command]

# Development server
[exact dev command]

# Run all tests
[exact test command]

# Lint and format
[exact lint command]

# Build for production
[exact build command]
```

## Naming Conventions

- **Files**: [e.g., kebab-case for components, camelCase for utilities]
- **Functions/Methods**: [e.g., camelCase, verb-first: getUserById]
- **Classes/Types**: [e.g., PascalCase: UserService, CreateUserDto]
- **Constants**: [e.g., UPPER_SNAKE_CASE: MAX_RETRY_COUNT]
- **Database**: [e.g., snake_case tables and columns]
- **API Endpoints**: [e.g., kebab-case paths: /api/user-profiles]

## Code Patterns

- [Pattern 1: e.g., "All API errors use the AppError class from lib/errors.ts"]
- [Pattern 2: e.g., "State is managed via Pinia stores, one store per domain"]
- [Pattern 3: e.g., "Async operations use Result<T, E> pattern, not try/catch"]
- [Anti-pattern: e.g., "Never import from internal module paths; use barrel exports"]

---

## Working Norms

These are behavioral constraints for AI agents working in this codebase.

- Ask before making large refactors or changing public APIs
- Prefer minimal diffs -- change only what's needed for the task
- Run tests before declaring work complete
- Do not introduce new dependencies without explicit approval
- Do not rewrite unrelated code to "clean it up"
- Explain tradeoffs before choosing between multiple valid approaches
- Commit messages follow conventional commits: type(scope): description
- [Add project-specific norms here]

---

## Agent Definitions

Sub-agent configurations live in `.agents/agents/`. Available agents:

| Agent | Role | Model Tier | Tools |
|-------|------|------------|-------|
| `default` | Planning and architecture | Highest | Read, Write, Grep, Glob |
| `worker` | Implementation | Mid-tier | All |
| `explorer` | Read-only codebase analysis | Fast | Read, Grep, Glob |

All agent descriptions reference this AGENTS.md as the single source of truth
for project patterns and conventions. Agent descriptions must not duplicate
content from this file.

In Codex-first repos, prefer these role names directly. If the project must stay
portable across multiple agent tools, add a short note mapping `default` to
Planner, `worker` to Executor/Coder, and `explorer` to Explorer.

`default` agents write specs to `docs/specs/[feature-name]/` when a planning
artifact is needed. `worker` agents implement from those specs. Review guidance
should stay read-only and escalate architectural concerns back to `default`.

---

## Progressive Context

Deeper context lives closer to the code it describes:

- `src/components/AGENTS.md` -- Component conventions and patterns
- `src/routes/AGENTS.md` -- API design rules and middleware patterns
- `docs/architecture.md` -- System design decisions and ADRs
- `docs/specs/` -- Implementation specs written by planner agents

---

## Adaptation Notes

When generating from this template:

1. **Remove unused sections** -- If the project has no database, drop that
   from Tech Stack. If it's a library, drop Routes/API sections.
2. **Add domain-specific sections** -- A data pipeline project might need a
   "Data Flow" section. A mobile app might need "Platform Constraints."
3. **Keep it under 200 lines** -- If a section grows beyond 10 lines, it
   probably contains progressive context that should be extracted.
4. **Validate commands** -- Every command in the Commands section must
   actually work when copy-pasted in a fresh clone.
5. **Test the naming conventions** -- Scan the codebase to verify the
   conventions listed actually match the existing code. Don't prescribe
   conventions that contradict the codebase.
