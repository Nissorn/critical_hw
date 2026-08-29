# Design

When does design run in a competition build, and what bar must the demo clear.
The craft itself lives in the `/impeccable` skill — invoke it for any frontend
surface work. This doc decides when to reach it and what it must deliver here.

## When design runs

Design is not a phase; it rides the build timeline from [`competition-build.md`](competition-build.md).

1. **Grill (0–10).** Nail the wow moment in visual terms: what a judge sees in
   the first seconds of the demo. Ask what the prototype must look like, not
   just what it must do.
2. **Tickets (10–15).** Frontend tickets carry a visual target. Design
   ownership stays with the owning worker — parallel workers, no shared
   designer bottleneck.
3. **Build (15–60).** Workers invoke `/impeccable` for every surface their
   ticket touches: screens, components, forms, landing page, dashboard, states.
4. **Integrate (50–110).** After each integration wave, run one `/impeccable`
   critique or polish pass over the primary flow before the demo stands at 60.
5. **Buffer (110–120).** Polish only: `/impeccable` for hierarchy, motion,
   micro-interactions, contrast. Feature-freeze at 115.

## The visual bar

The demo must read as finished on first glance: strong visual hierarchy,
consistent spacing, type, and color, and no default-browser look. Bland is
judge-invisible — the wow moment has to sell visually before anyone interacts.
Concentrate design effort on the primary flow; secondary surfaces get baseline
only. When behind schedule, cosmetic polish is cut first (see
[`competition-build.md`](competition-build.md) scope decisions), so never let
design block a demo blocker fix.

## Consistency

Workers design their own surfaces, so integrated screens can clash. Settle a
tiny shared token set — spacing, type, color — at ticket time; the orchestrator
owns it and keeps it in the shared skeleton.
