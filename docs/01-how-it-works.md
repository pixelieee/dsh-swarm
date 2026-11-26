# How it works

The pattern is **not a plugin**. It is a personal **skill** plus DSH's built-in multi-agent primitives.

## Layers

| Layer | Responsibility |
|---|---|
| Skill (`team-leader-mode`) | Behaviour: when to switch, role boundaries, delegation template, reporting cadence, verification, stop/correct rules. |
| DSH runtime | Subagent sessions with independent context, shared filesystem, async messages, completion notifications, `wait_agent`, background jobs, sandbox. |
| Lead agent | Keeps short turns: triage → delegate → wait → verify → report → decide. |

## The loop

```
user request
  → lead triages (code / docs / infra / bug / decision)
  → delegates self-contained instructions to subagents
      (background, goal, steps, constraints, acceptance, report format)
  → subagents execute in isolated worktrees / ports / profiles
  → subagents send results back (message + evidence paths)
  → lead verifies evidence (tests, gates, diffs, screenshots)
  → lead reports to user (conclusion first, short)
  → user decides; loop continues
```

## Why the lead must not execute everything

If the lead runs every long task itself, its context fills with logs and code, decisions become slower, and the user loses visibility. Delegation keeps the lead's context small and its report cadence fast.

## Why subagents must be isolated

Parallel agents editing the same worktree is the fastest way to corrupt work. Give each one its own worktree/branch (and port/profile when relevant), and state the write scope explicitly.

## Why `wait_agent` and short turns matter

An agent turn ends when the model stops calling tools. If the model chains a hundred tool calls in one turn, the user sees nothing for a long time. `wait_agent` and "report after each result" split one long block into many short visible turns — this is the real fix for "the agent went silent".

## Minimum viable skill

The two rules that matter most:

1. The lead only talks, decides, delegates, verifies; direct execution is the exception.
2. Every subagent report must carry evidence; the lead verifies before reporting "done".
