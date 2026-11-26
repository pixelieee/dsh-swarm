# Why agents go silent mid-work (and how to fix it)

## Symptom

You ask an agent to do something non-trivial. It immediately disappears into a long tool loop and only comes back minutes later with a wall of text. Codex-based agents often feel chattier: they post short updates between tool calls.

## Mechanism

1. A model turn can contain many tool calls. **Tool execution itself produces no user-visible message.**
2. What the user sees is assistant text. If the model does not emit "commentary" text between tool calls, the UI stays silent.
3. Coding agents like Codex are prompted (and trained) to give short updates while working, and their UI streams those commentary messages.
4. A default harness prompt without that requirement nudges the model to "finish the job, then summarize" → silence.

So the difference is **prompting + turn design**, not model capability, and not the harness's streaming engine.

## The fix

| Tactic | Effect |
|---|---|
| Require progress reporting in the skill/prompt ("report after each result", "if >2 min without progress, say what you are waiting for") | The model emits visible updates |
| Split work into short turns (delegate, wait, report, delegate) | More natural checkpoints |
| Use async waiting primitives (`wait_agent`) instead of one giant tool loop | User sees each result as it lands |
| Fixed status format (e.g. a 4-column table + max 3 lines) | Updates stay short and scannable |
| "No change" replies when idle | Avoids both silence and spam |

## Anti-patterns

- "Do everything, then answer" prompts for long tasks.
- One monster tool call that hides all intermediate state.
- Reporting only when the work is 100% complete.

## Checklist for a skill author

- [ ] Role boundary: who executes vs. who decides.
- [ ] Report cadence: when to speak, what format, max length.
- [ ] Delegation template: background, goal, steps, constraints, acceptance, report format.
- [ ] Verification rule: evidence required before saying "done".
- [ ] Idle rule: say "no change" rather than going quiet.
