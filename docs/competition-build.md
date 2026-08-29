# Competition build

Use this playbook for a time-boxed competition prototype.

## Sequence

1. **Grill (0–10 minutes).** Resolve only the exact demo goal, judge-visible wow moment, smallest end-to-end flow, mandatory requirements, available data/API/model/assets, and cuttable scope. Ask one question at a time with a recommended default. Inspect the repository before asking questions it can answer. At minute 10, stop grilling: choose the fastest reasonable default and move on.
2. **Tickets (10–15 minutes).** Run `/to-tickets`. Create 3–6 independent vertical slices with acceptance criteria, dependencies, and clear ownership. Keep shared-file ownership explicit; skip `/to-spec` unless the feature cannot be implemented without it.
3. **Build (15–60 minutes).** Start every unblocked ticket with `/implement <ticket>`. Prefer 3–5 parallel workers. Each worker owns only its ticket, avoids unrelated refactors and active workers’ files, runs the fastest relevant validation, commits its changes, and reports status, commit SHA, validation, changed surfaces, and integration notes. While workers run, advance integration, shared skeleton, mock data, demo instructions, and newly unblocked work.
4. **Integrate and ship (50–110 minutes).** Have the primary demo flow standing by minute 60. Stop waiting for optional work. Integrate accepted commits continuously, run the fastest useful checks after each wave, and exercise the primary demo flow. Protect one convincing end-to-end path over secondary features.
5. **Buffer (110–120 minutes).** Feature-freeze at minute 115. Spend the buffer only on demo blockers, deployment fixes, and visible polish.

## Scope decisions

When behind schedule, cut in this order: animation and cosmetic polish, secondary screens, persistence, invisible edge cases, auth, backend that can be mocked, optional integrations, then abstractions. Prefer a static client-side prototype and mocked behavior when a real backend is not essential to judging. Never place secrets in client code.

## Operating rules

Choose the fastest, simplest, easiest-to-demo, easiest-to-deploy, and easiest-to-revert option for low-risk ambiguity. Ask only when the choice affects the problem, mandatory rules, credentials/cost/security, or repository visibility. Keep accepted work committed and pushed. Do not silently make a private repository public.

For worker isolation and integration, use isolated branches/worktrees when practical; the orchestrator owns `main`. Integrate by commit SHA and verify the main demo after integration. The build is complete only when the core judge-visible flow works end to end, mandatory requirements are met, accepted work is committed, and the shipped prototype is checked.
