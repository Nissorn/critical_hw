# Shipping checklist

Read this when integrating or deploying a competition prototype.

## Integration

- Integrate completed worker commits continuously; do not wait for optional tickets.
- Keep `main` as the integration branch and use commit SHAs for accepted work.
- Run the fastest useful checks after each integration wave.
- Exercise the primary flow locally before deployment.

## Deployment

- Prefer GitHub Pages for a static/client-side prototype.
- Make project-page base paths and assets work at `https://<owner>.github.io/<repo>/`.
- Keep secrets out of client code.
- Do not change repository visibility without an explicit decision.
- Push accepted work to `main`.
- Open the real deployed URL and complete the primary flow there. A deployment is not verified by a successful build alone.

## Final report

Record the repository URL, prototype URL, demo flow, and known limitations. Report only behavior and checks actually observed.
