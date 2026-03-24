# optimize-agents-md

Codex skill for reviewing and restructuring `AGENTS.md`, `CLAUDE.md`,
`.cursorrules`, and related AI instruction files.

It separates project memory, working norms, progressive context, and agent
definitions; removes duplication; and produces Codex-friendly output that still
works well as general markdown guidance.

## Install

Copy this folder into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R optimize-agents-md ~/.codex/skills/
```

Or clone this repo directly there:

```bash
git clone https://github.com/mokk43/optimize-agents-md.git ~/.codex/skills/optimize-agents-md
```

Restart Codex after installing.

## Use

Example prompts:

- `optimise AGENTS.md`
- `optimize AGENTS.md`
- `improve this AGENTS.md`
- `convert this CLAUDE.md to AGENTS.md`

## Contents

- `SKILL.md`: trigger metadata and workflow
- `agents/openai.yaml`: Codex UI metadata
- `references/`: templates and supporting reference material

## License

MIT
