# GoSalty Skills

Codex skills for working with GoSalty product, partner, and agent workflows.

## Skills

- `gosalty-discovery-agent`: turns product ideas, stories, and vague GoSalty tasks into rider-centered discovery briefs and implementation-ready next steps.
- `gosalty-external-agent`: guides external AI agents that interact with GoSalty data, partner tools, event workflows, and Convex surfaces.

## Use

The canonical skill folders live at the repository root. This repo also exposes
them through the current discovery paths for Codex and Claude Code:

- Codex/ChatGPT-style agents: `.agents/skills/`
- Claude Code: `.claude/skills/`

Invoke them by name:

```text
Use $gosalty-discovery-agent to refine this event discovery story.
Use $gosalty-external-agent to add a partner event tool.
```

You can also copy or symlink the root skill folders into a user-level skills
directory if you want them available outside this repository.

These skills are intentionally scoped to GoSalty-specific interaction patterns. Backend implementation work should still follow the target repository's local instructions.
