# vs. dsh-superagent (event-driven autonomous agents)

DeepSeek's `dsh-superagent-prototype` (`deepseek-ai/dsh-superagent-prototype`) is a **DSH bundle**: a proactive coding agent for one GitHub repository. It listens to GitHub (and optionally Feishu), opens work items, works in its own session/directory, answers in the original conversation (e.g. with a PR), and shows a web panel of what needs a human.

It is **not** a replacement for this playbook; they solve different halves of the same problem.

| Dimension | This playbook (`dsh-swarm`) | dsh-superagent |
|---|---|---|
| Trigger | Human conversation, real time | External events (GitHub/Feishu) + team rules |
| Goal | Interactive collaboration with fast, evidence-based reporting | Team-level autonomy with minimal human attention |
| Form | Skill + built-in subagent primitives | DSH bundle (panel, authorization model, deployment) |
| Orchestration | Lead decides live | Work-item state machine |
| Shared base | DSH multi-session, isolated directories, tool execution | same |

## When to use which

- **Interactive work** (a human is in the loop right now, decisions are needed): use the leader/skill pattern.
- **Asynchronous work** (a request arrives as an issue/event, nobody wants to babysit): use an event-driven bundle like dsh-superagent.
- **Both**: the bundle creates work items; the leader skill governs how agents collaborate inside each item.

## The principle both share

From the superagent docs: *"take as little human attention as possible, and give as much value as possible."* This playbook adds the interactive counterpart: when a human **is** present, make every turn count — decide, delegate, verify, report.
