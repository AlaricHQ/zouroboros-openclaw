# Zouroboros for OpenClaw

[![CI](https://github.com/AlaricHQ/zouroboros-openclaw/actions/workflows/ci.yml/badge.svg)](https://github.com/AlaricHQ/zouroboros-openclaw/actions/workflows/ci.yml)
[![Examples](https://img.shields.io/badge/examples-runnable-blue)](https://github.com/AlaricHQ/zouroboros-openclaw-examples)
[![npm memory](https://img.shields.io/npm/v/zouroboros-memory)](https://www.npmjs.com/package/zouroboros-memory)
[![npm swarm-gate](https://img.shields.io/npm/v/zouroboros-swarm-gate)](https://www.npmjs.com/package/zouroboros-swarm-gate)
[![npm autoloop](https://img.shields.io/npm/v/zouroboros-autoloop)](https://www.npmjs.com/package/zouroboros-autoloop)
[![npm bench](https://img.shields.io/npm/v/zouroboros-bench)](https://www.npmjs.com/package/zouroboros-bench)

OpenClaw-compatible Zouroboros packages, published as standalone npm modules and designed to work outside Zo Computer. This repo is the public distribution surface for OpenClaw users; the canonical upstream framework remains `marlandoj/Zouroboros`.

## Repo Split

- `marlandoj/Zouroboros` — canonical upstream, core framework, Zo-native integrations
- `AlaricHQ/zouroboros-openclaw` — OpenClaw-facing packaging, install docs, examples, release surface

## Packages

| Package | npm | Best For |
|---------|-----|----------|
| Swarm Gate | `zouroboros-swarm-gate` | Deciding when a task actually needs multi-agent orchestration |
| Autoloop | `zouroboros-autoloop` | Autonomous optimization against a measurable metric |
| Memory | `zouroboros-memory` | Persistent memory, hybrid retrieval, MCP-based memory tools |
| Bench | `zouroboros-bench` | Benchmarking memory systems with LongMemEval, LoCoMo, and ConvoMem |

## Install

```bash
npm install -g zouroboros-swarm-gate
npm install -g zouroboros-autoloop
npm install zouroboros-memory
npm install -D zouroboros-bench
```

## Choose Your Starting Point

- Want persistent memory in OpenClaw: start with `zouroboros-memory`
- Want a zero-cost routing decision first: start with `zouroboros-swarm-gate`
- Want an optimization loop around one measurable metric: start with `zouroboros-autoloop`
- Want to compare memory quality with repeatable runs: start with `zouroboros-bench`

## Quick Start

### 1. Swarm Gate

```bash
npx zouroboros-swarm-gate "build a REST API with auth, tests, and deploy"
```

### 2. Autoloop

```bash
autoloop --program ./program.md --executor "openclaw ask"
```

### 3. Memory

```bash
npx zouroboros-memory init
npx zouroboros-memory store --entity user --key preference --value "dark mode" --decay permanent
npx zouroboros-memory search "dark mode"
```

### 4. Bench

```bash
npx zouroboros-bench --benchmarks longmemeval --limit 50
```

## OpenClaw

These packages are intended to be easy to install from npm and straightforward to wire into OpenClaw skills and MCP configs. The examples repo should carry the copy-paste project setups; this repo should keep the package-level docs and release notes close to the code.

## Examples

Runnable starters live at `https://github.com/AlaricHQ/zouroboros-openclaw-examples`.

- `examples/memory-openclaw` — local MCP wiring, seed data, sample `SKILL.md`
- `examples/swarm-gate` — direct vs swarm task routing examples
- `examples/autoloop-hello` — real target file, tests, and metric extraction
- `examples/bench-local` — seeded local benchmark fixture for `zouroboros-memory`

## When To Use Zo Computer Instead

Use these packages when you want portable pieces. Use Zo Computer when you want the full integrated system: persistent memory, swarm orchestration, scheduled agents, persona routing, hosted services, and a managed AI workspace.

## License

MIT
