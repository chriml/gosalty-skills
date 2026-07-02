# GoSalty Skills Repository

## Purpose

This repository publishes reusable skills for agents that interact with GoSalty product, partner, and agent workflows.

## Repository Rules

- Keep each skill focused on one GoSalty interaction job.
- Keep `SKILL.md` concise; move detailed domain context into `references/` and link it from `SKILL.md`.
- Use imperative instructions and concrete triggers in skill descriptions.
- Do not duplicate skill folders across agent-specific directories. Keep canonical skills at the repository root and expose them through symlinks in `.agents/skills/` and `.claude/skills/`.
- Do not add secrets, private credentials, production data, or user-specific local settings.

## Validation

- After changing a skill, run:

```bash
python3 /Users/saltychris/.codex/skills/.system/skill-creator/scripts/quick_validate.py <skill-folder>
```

- If that local validator is unavailable, manually check that each changed `SKILL.md` has YAML frontmatter with `name` and `description`, and that referenced files exist.

## Review Guidelines

- Treat stale GoSalty workflow guidance as a product risk.
- Check that agent-facing mutation, publishing, or destructive workflows require explicit confirmation.
- Check that skill descriptions are specific enough for implicit invocation without being broad catch-alls.
