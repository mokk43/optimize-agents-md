# optimize-agents-md

Codex skill for reviewing, restructuring, and migrating `AGENTS.md`,
`CLAUDE.md`, `.cursorrules`, and related AI instruction files.

It is designed to turn bloated or mixed-purpose instruction files into a
clearer structure with:

- project memory
- working norms
- progressive context
- Codex-friendly agent guidance

The skill is Codex-first, but it keeps the generated markdown usable for other
AI coding tools when needed.

## What It Does

Given an existing instruction file, the skill:

1. Reads the current files before proposing changes
2. Flags structural problems such as mixed concerns, vague rules, and token bloat
3. Proposes a cleaner `AGENTS.md`
4. Optionally generates supporting agent definitions and a `CLAUDE.md` shim
5. Asks before writing files

## Install

Clone directly into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/mokk43/optimize-agents-md.git ~/.codex/skills/optimize-agents-md
```

Or copy an existing checkout there:

```bash
mkdir -p ~/.codex/skills
cp -R optimize-agents-md ~/.codex/skills/
```

Restart Codex after installing.

## Example Prompts

- `optimise AGENTS.md`
- `optimize AGENTS.md`
- `improve this AGENTS.md`
- `review this CLAUDE.md and convert it to AGENTS.md`
- `clean up our .cursorrules and make it Codex-friendly`

## Repo Layout

- `SKILL.md`: trigger metadata and workflow
- `agents/openai.yaml`: Codex UI metadata
- `references/agent-templates.md`: agent archetypes and Codex naming guidance
- `references/canonical-structure.md`: reference structure for optimized `AGENTS.md`
- `references/examples.md`: before/after examples

## Notes

- The skill prefers Codex-native role names such as `default`, `worker`, and
  `explorer` when the target repo is clearly Codex-first.
- It preserves more tool-agnostic wording when migration compatibility matters.

## License

MIT
