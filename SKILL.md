---
name: optimize-agents-md
description: >-
  Analyze and optimize or optimise existing AGENTS.md, CLAUDE.md, .cursorrules,
  and related AI instruction files for coding assistants. Separates project
  memory, working norms, progressive context, and agent definitions; removes
  duplication; generates tool-agnostic markdown plus Codex-friendly agent
  guidance. Use when the user asks to optimize AGENTS.md, optimise AGENTS.md,
  improve, review, convert, migrate, or clean up AI instruction files.
---

# Optimize AGENTS.md / CLAUDE.md

Analyze an existing project's AI instruction files and produce an optimized,
well-structured output following industry best practices for context engineering.

## Workflow

Two phases executed sequentially. Always read the existing file(s) before
proposing any changes. Present proposed changes and ask for user confirmation
before writing any files.

```
Phase 1: Analyze  -->  Phase 2: Optimize & Generate
```

---

## Phase 1: Analyze

### 1.1 Locate and read

Search the project for all AI instruction files and read their contents:

- `AGENTS.md` (root, subdirectories)
- `CLAUDE.md` (root, `~/.claude/`)
- `.cursorrules`
- `.cursor/rules/*.mdc`
- `.agents/` and `.claude/agents/` directories

Use `rg` / `rg --files` for discovery and parallelize independent reads when it
helps reduce latency.

Simultaneously scan the codebase to infer project identity:

- Dependency manifests: `package.json`, `pyproject.toml`, `Cargo.toml`,
  `go.mod`, `pom.xml`, `Gemfile`, `composer.json`
- Top-level directory listing (2 levels deep)
- README.md if it exists (first 100 lines)
- CI config (`.github/workflows/`, `Makefile`, `Dockerfile`)

Extract: language(s), framework(s), build commands, test commands, lint commands.

If a Codex-specific setup exists, also inspect:

- `agents/openai.yaml`
- `~/.codex/skills/` only when the user's request is about installed skills
- existing `AGENTS.md` sections that describe review, explorer, or worker flows

### 1.2 Classify every section

Walk through each heading/paragraph in the existing file(s) and tag it with
exactly one category:

| Tag | Category | Belongs in |
|-----|----------|------------|
| PM  | Project Memory | Top-level AGENTS.md, first half |
| WN  | Working Norms | Top-level AGENTS.md, second half |
| PC  | Progressive Context | Subdirectory AGENTS.md or docs/ reference |
| AD  | Agent Definition | `.agents/agents/` or top-level Agent Definitions section |
| DUP | Duplicate | Remove; already covered elsewhere |
| IRR | Irrelevant/Stale | Remove or flag for user review |

### 1.3 Detect anti-patterns

Flag these issues with severity:

- CRITICAL: Mixed project facts and behavioral rules in same section
- CRITICAL: Agent descriptions duplicating AGENTS.md content instead of referencing it
- HIGH: File exceeds 200 lines (token bloat)
- HIGH: Missing build/test commands (agents can't verify their work)
- HIGH: Vague rules ("write good code") that aren't testable
- MEDIUM: No directory map (agents guess where to look)
- MEDIUM: Progressive context dumped at top level
- MEDIUM: Tool-specific format that locks out other AI tools
- LOW: No agent definitions (missed opportunity for specialization)
- LOW: Inconsistent terminology across sections

### 1.4 Identify agent opportunities

Based on the project structure, determine which sub-agent archetypes apply:

- **Planner**: Always applicable. In Codex output, prefer `default` as the
  primary planning/architecture role label unless the user explicitly wants
  vendor-neutral names.
- **Executor/Coder**: One per major tech domain (e.g., `vue3-coder`,
  `fastapi-coder`, `react-coder`). In Codex output, prefer `worker` wording for
  bounded implementation roles.
- **Reviewer**: Always applicable. In Codex output, prefer review workflow
  guidance over presenting Reviewer as a first-class tool type.
- **Explorer**: Always applicable. In Codex output, prefer `explorer` as the
  literal role name when describing read-only analysis agents.

Read [references/agent-templates.md](references/agent-templates.md) for the role archetypes and
template structure.

---

## Phase 2: Optimize & Generate

### 2.1 Restructure the top-level file

Read [references/canonical-structure.md](references/canonical-structure.md) for the reference template.

Generate an optimized `AGENTS.md` following these rules:

1. Project Memory sections first (orientation for the agent)
2. Working Norms section second (behavioral constraints)
3. Agent Definitions section third (optional, for projects using sub-agents)
4. Total file under 200 lines
5. Every rule must be specific, testable, and enforceable
6. Build/test/lint commands must be exact and copy-pasteable
7. No duplication -- use references (`See docs/architecture.md`) for deep context
8. Format must be tool-agnostic markdown (works with Cursor, Claude Code,
   Codex, Gemini, etc.)

### 2.2 Extract progressive context

For any content tagged PC in Phase 1:

- If it belongs to a specific subdirectory, create/update a nested `AGENTS.md`
  in that subdirectory
- If it's architectural documentation, reference it via a pointer in the
  top-level file rather than inlining it

### 2.3 Generate agent definitions

For each identified sub-agent opportunity from Phase 1.4:

1. Read the matching template from [references/agent-templates.md](references/agent-templates.md)
2. Inject project-specific values (tech stack, framework, conventions)
3. Set model tier: Planner = highest capability, Executor = mid-tier,
   Reviewer = fast, Explorer = fast
4. Set tool restrictions: Reviewer = Read/Grep/Glob only (no Write/Edit);
   Planner = Read/Write/Grep/Glob (writes specs, not code);
   Executor = all tools
5. Add cooperation rules: Planners cooperate with Explore agents;
   Executors receive feedback from Reviewers; Reviewers escalate
   architectural issues to Planners
6. Each agent description must reference `AGENTS.md` for project patterns,
   never duplicate them
7. Each agent description stays under 8000 characters

When adapting the output for Codex, map generic archetypes to Codex concepts:

- Planner -> `default` sub-agent used for higher-judgment planning tasks
- Explorer -> `explorer` sub-agent for codebase analysis
- Executor/Coder -> `worker` sub-agent for bounded implementation tasks
- Reviewer -> read-only review instructions; in Codex this is usually a review
  workflow or a constrained `default`/`explorer` pass, not a separate tool class

In user-facing `AGENTS.md` output for Codex-first repos, prefer these labels
directly in the final file:

- `default` for planning, architectural judgment, and general high-context work
- `worker` for bounded implementation tasks
- `explorer` for read-only codebase investigation
- `review` or `code review workflow` for read-only validation guidance

Use Planner / Executor / Reviewer / Explorer wording only when preserving an
existing cross-tool convention is clearly more important than Codex alignment.

Write agent files to `.agents/agents/[agent-name].md`.

### 2.4 Generate CLAUDE.md shim

If the project previously used `CLAUDE.md` as its primary file, or if Claude
Code compatibility is needed, generate a minimal `CLAUDE.md`:

```markdown
# Project Instructions

All project instructions, conventions, and agent definitions live in AGENTS.md.

See @AGENTS.md
```

### 2.5 Present results

Output to the user in this order:

1. **Issues Found**: List from Phase 1.3 with severities
2. **Proposed Structure**: Table of contents of the new AGENTS.md
3. **Full Optimized AGENTS.md**: Complete file content
4. **Sub-Agent Definitions**: Each agent file with its content
5. **CLAUDE.md Shim**: If applicable
6. **Migration Notes**: What changed and why; any manual follow-ups needed

Ask for user confirmation before writing any files.

When presenting findings in Codex, lead with concrete issues and keep the
summary brief. Use precise file references when discussing existing files.

### 2.6 Write files (after confirmation)

Write in this order:

1. `AGENTS.md` (project root)
2. `CLAUDE.md` shim (project root, if applicable)
3. `.agents/agents/[name].md` for each sub-agent
4. Subdirectory `AGENTS.md` files for progressive context
5. Symlinks or pointers for tool-specific directories if requested

---

## Key Principles

These principles govern every decision in the optimization:

1. **Minimum viable onboarding context** -- Include only what an agent needs
   upfront to enter the codebase safely. Not maximal context.
2. **Single source of truth** -- AGENTS.md is the authority. Agent definitions
   reference it, never duplicate it.
3. **Concern separation** -- Project memory, working norms, and progressive
   context are distinct categories that must not be mixed.
4. **Token efficiency** -- Every line must earn its token cost. Verbose
   explanations of obvious things waste context window.
5. **Planner -> Executor -> Reviewer hierarchy** -- Clear role boundaries
   with appropriate model tiers and tool restrictions.
6. **Progressive disclosure** -- Deep details stay close to the code they
   describe, loaded on demand.
7. **Tool agnosticism** -- Output works across Cursor, Claude Code, Codex,
   Gemini CLI, and other AGENTS.md-compatible tools.
8. **Codex-native execution** -- In Codex, prefer `rg` for discovery,
   parallel reads for context gathering, and `apply_patch` for edits.

---

## Additional Resources

- Reference template: [references/canonical-structure.md](references/canonical-structure.md)
- Sub-agent templates: [references/agent-templates.md](references/agent-templates.md)
- Before/after examples: [references/examples.md](references/examples.md)
