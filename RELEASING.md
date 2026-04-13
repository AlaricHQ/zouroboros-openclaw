# Releasing

This repo is the OpenClaw-facing distribution surface for Zouroboros packages.

## Surfaces

- GitHub repo: `AlaricHQ/zouroboros-openclaw`
- Examples repo: `AlaricHQ/zouroboros-openclaw-examples`
- Canonical upstream: `marlandoj/Zouroboros`
- npm packages:
  - `zouroboros-memory`
  - `zouroboros-swarm-gate`
  - `zouroboros-autoloop`
  - `zouroboros-bench`

## Normal Release Flow

1. Update package versions and package-level changelog/release notes as needed.
2. Confirm README links still point to:
   - `AlaricHQ/zouroboros-openclaw`
   - `AlaricHQ/zouroboros-openclaw-examples`
   - referral URL for Zo Computer CTA
3. Verify examples still match the published package surface.
4. Push `main`.
5. Run GitHub Actions:
   - `CI`
   - `Publish Packages` with `target=all` or a single package
6. Create a GitHub Release if you want a public milestone in the repo history.

## Preconditions

- `NPM_TOKEN` must exist in GitHub Actions secrets for `AlaricHQ/zouroboros-openclaw`
- package builds must pass locally or in CI
- examples should be runnable, not just documented

## Manual Checks

```bash
git status
git log -1 --oneline
```

Per package:

```bash
cd packages/swarm-gate && npm ci && npm run build && npm test --if-present
cd packages/autoloop && npm ci && npm run build
cd packages/memory && npm ci && npm run build && npm run typecheck --if-present
cd packages/bench && npm ci && npm run build && npm run typecheck --if-present
```

## Examples Sanity Checks

```bash
cd ../zouroboros-openclaw-examples/examples/memory-openclaw && npm install && npm run seed && npm run search
cd ../bench-local && npm install && npm run seed
cd ../autoloop-hello && npm test && node ./scripts/measure.mjs
```

## Notes

- `zouroboros-swarm-gate` installs the `swarm-gate` CLI binary
- `zouroboros-autoloop` installs the `autoloop` and `autoloop-mcp` binaries
- `zouroboros-memory` installs the `zouroboros-memory` and `zouroboros-memory-mcp` binaries
- `zouroboros-bench` installs `zouroboros-bench` and `zouroboros-bench-report`
