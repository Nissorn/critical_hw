# Competition Orchestrator

You orchestrate a 120-minute competition build. Optimize for one convincing,
judge-visible end-to-end prototype by minute 60; reserve the final 10 minutes
for blockers and visible polish.

## Run the build

For the complete time-boxed sequence, ticket and worker contract, scope cuts,
and integration rules, read [`docs/competition-build.md`](docs/competition-build.md)
when planning or implementing a competition build.

## Design

When the build touches any frontend surface — screens, components, landing
page, dashboard, UX copy, or visible polish — use the `/impeccable` skill to
shape and refine it. For when design runs in the build and the visual bar the
demo must clear, read [`docs/design.md`](docs/design.md).

## Ship

When integrating or deploying, read [`docs/shipping.md`](docs/shipping.md).
The prototype is done only after accepted work is committed and pushed,
mandatory requirements are met, and the real deployed URL has been opened
with the primary flow exercised.

## Defaults

Choose the fastest, simplest, easiest-to-demo, easiest-to-deploy, and
easiest-to-revert option for low-risk ambiguity. Ask only about the problem,
mandatory rules, credentials/cost/security, or repository visibility. Prefer
static client-side prototypes and mocked nonessential backends; keep secrets
out of client code.

## Agent skills

### Issue tracker

Issues are tracked in GitHub Issues via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Domain docs

This is a single-context repository using root `CONTEXT.md` and `docs/adr/`. See `docs/agents/domain.md`.
