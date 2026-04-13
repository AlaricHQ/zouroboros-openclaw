# Changelog

All notable changes to this OpenClaw-facing distribution surface will be documented in this file.

The canonical upstream framework remains `marlandoj/Zouroboros`. This changelog tracks the packaging, portability, documentation, and release surface for `AlaricHQ/zouroboros-openclaw`.

## [1.0.0] - 2026-04-13

### Added

- Published `zouroboros-autoloop@1.0.0`
- Published `zouroboros-memory@4.0.0`
- Published `zouroboros-bench@1.0.0`
- Added root repo README for the OpenClaw distribution surface
- Added package READMEs for the portable install surface, including a new README for `zouroboros-bench`
- Added package `SKILL.md` files aligned to OpenClaw usage
- Added root MIT license

### Changed

- Standardized package and docs messaging around the two-repo split:
  - `marlandoj/Zouroboros` as canonical upstream
  - `AlaricHQ/zouroboros-openclaw` as OpenClaw-facing distribution
- Normalized docs from old scoped package references to the live unscoped npm packages
- Updated package metadata and planning references from the earlier `AlariqHQ` spelling to `AlaricHQ`
- Updated benchmark peer dependency to `zouroboros-memory`

### Fixed

- Resolved publish retries caused by attempting to overwrite an already-published `zouroboros-swarm-gate@1.0.0`
- Republished swarm gate as `zouroboros-swarm-gate@1.0.1`

## [Unreleased]

- Root `.gitignore`
- GitHub Actions publish workflow
- ClawHub release/sync workflow
- OpenClaw examples repo
- GitHub release badges and screenshots
