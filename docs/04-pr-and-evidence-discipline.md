# PR and evidence discipline

These rules matter when many agents work on one repository at once.

## Stacked PRs

- Build from the bottom up: every PR's base is the previous layer's branch, so each PR shows only its own diff.
- Merge one layer, then sync the next: rebase/merge the new main (or the previous layer's new head) into the next PR, resolve conflicts, run gates, then merge.
- Never merge out of order; never "fix forward" on top of an unmerged base.
- After every merge, verify the next PR's diff does not include the previous layer's changes (stale base = polluted diff; fix the base pointer first).

## No patch layers

- If a correction belongs to layer N, put it into layer N — not into a new "fix" PR on top. Patch layers make review harder and history misleading.
- If a patch layer already exists, dissolve it: fold its hunks into the original layers, close the patch PR, and leave a mapping comment.

## Evidence-based acceptance

Before reporting "done", the lead verifies:

- gates: typecheck, boundaries, contract tests, unit tests;
- behavior: E2E or a real reproduction, with screenshots/logs;
- diff: the PR only contains what it claims;
- risk notes: temporary compatibility, security deviations, deferred work.

A message saying "fixed" is not evidence.

## Audit documents

- Keep audits, plans, benchmarks, and acceptance reports in a workspace folder **outside** the repository (e.g. `.bundle-reviews/`).
- Repository `docs/` holds only documentation that matches current production code.
- Record permanent product/technical decisions in a decision log (e.g. `00-master-plan.md`) so future agents cannot silently reverse them.

## Model-facing text is code

Tool names, skill documents, prompt snippets, and tests are all part of the model's world. When renaming a tool:

1. Update the tool catalog and descriptions.
2. Update **every** skill document and reference.
3. Update presentation labels and prompt text.
4. Update tests and static assertions.
5. Add a CI grep gate so old names cannot creep back after the alias window closes.
