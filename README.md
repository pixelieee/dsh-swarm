# dsh-swarm

**A supervised agent-swarm pattern for DeepSeek Harness: one lead agent, a swarm of subagents, evidence-based verification, and frequent reporting.**

> The name is a nod to OpenAI Swarm — here the swarm is *supervised*: a lead agent triages, delegates, verifies, and reports; subagents execute.

It ships a working skill, not just advice: install `skills/team-leader-mode/SKILL.md`, and the main agent becomes a lead that only triages, delegates, verifies, and reports.

Most agent frameworks optimize for a single agent doing more. This repository is about the other axis: **how to run many agents in parallel without losing the user's attention or trust**.

## What is inside

| Path | What it is |
|---|---|
| `skills/team-leader-mode/SKILL.md` | A DSH personal skill that turns the main agent into a lead: it only talks, decides, delegates and verifies. |
| `docs/01-how-it-works.md` | The mechanism: skill + DSH primitives (subagents, async messages, `wait_agent`, background jobs). |
| `docs/02-vs-superagent.md` | How this pattern differs from event-driven autonomous agents (e.g. DeepSeek's `dsh-superagent-prototype`). |
| `docs/03-mid-work-replies.md` | Why agents go silent mid-work, and what actually fixes it (prompting + turn design, not model capability). |
| `docs/04-pr-and-evidence-discipline.md` | Rules for stacked PRs, evidence-based acceptance, audit documents, and decision records. |

## Requirements & compatibility

- DeepSeek Harness with personal skills support (`~/.dsh/skills`).
- Subagent tools: `list_agents`, `send_message`, `wait_agent` (tested with `@deepseek-ai/dsh-subagent-codex 0.2.2-rc.1`).
- Recommended agent presets: **PTC** or the standard preset; avoid minimal/creative for strict engineering orchestration.
- The skill is plain text and is not pinned to a DSH version. If subagent tools are unavailable, the pattern degrades to a normal single-agent session; if a helper tool is missing, adjust the skill locally.

## Quick start (DeepSeek Harness)

1. Copy `skills/team-leader-mode/SKILL.md` to `~/.dsh/skills/team-leader-mode/SKILL.md`.
2. Start a session and tell the agent what you want: e.g. **"act as the team lead; delegate the work to subagents and report back"**. The skill describes the intent; no fixed phrase or keyword is required.
3. The lead agent will triage, delegate to subagents, wait for evidence, and report.

Recommended agent preset: **PTC** or the standard preset. Avoid minimal/creative presets for strict engineering orchestration.

## Principles

1. **The lead's attention is the scarce resource.** Subagents do the long work; the lead only makes decisions and verifies.
2. **Evidence over claims.** "Done" means gates green, tests passing, diffs checked — not a message saying it works.
3. **Short turns, frequent reports.** A long silent tool loop looks like a hung agent. Split work into short turns.
4. **Isolation by default.** Each subagent gets its own worktree, branch, port, and profile.
5. **No patch layers.** Corrections go back to the layer that should have had them.

## License

MIT
