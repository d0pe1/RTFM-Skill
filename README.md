# RTFM

An agent skill for infrastructure work: IaC, CI/CD and system administration. It makes the agent work from current, version-correct docs instead of its memory, and deliver changes as reusable code that is proven to work.

1. **Pin**: find the tool, version and platform actually in use.
2. **Read**: installed tool's own help first, then versioned official docs, changelogs and upgrade guides.
3. **Diff**: compare against what it remembers; surface deprecated patterns instead of silently copying or "modernizing" them.
4. **Converge, converge, verify**: ship the change as an idempotent script / IaC; apply, apply again expecting zero changes, then verify the *running* system.
5. **Cite**: sources for non-obvious choices, command + output for every check claimed, anything unverified named specifically.

## Install

Copy `rtfm/SKILL.md` and `rtfm/references/` into your agent's skills directory, e.g.

- OpenCode: `~/.config/opencode/skills/rtfm/`
- Claude Code: `~/.claude/skills/rtfm/`

## Evals

`rtfm/evals/evals.json` holds the behaviour test prompts and assertions. Over three rounds (6 prompts, with vs. without the skill), pass rates were about 94–100% with the skill and about 60% without it.
