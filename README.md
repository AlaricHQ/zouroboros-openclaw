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

| Package | npm | What It Does |
|---------|-----|--------------|
| Memory | `zouroboros-memory` | Gives AI agents a durable memory backend with SQLite storage, hybrid retrieval, decay classes, episodic memory, cognitive profiles, and an MCP server for OpenClaw or any MCP-capable client. |
| Swarm Gate | `zouroboros-swarm-gate` | Adds a cheap mechanical decision layer before orchestration, scoring whether a task should stay direct, escalate to swarm execution, or remain a judgment call. |
| Autoloop | `zouroboros-autoloop` | Runs an autonomous optimize-measure-keep-or-revert loop around one target file and one numeric metric, inspired by Andrej Karpathy's `autoresearch` idea of repeated mechanical improvement. |
| Bench | `zouroboros-bench` | Runs repeatable memory benchmarks and reporting so you can compare retrieval quality with actual evaluation data instead of anecdotal impressions. |

## What Each Package Is For

### `zouroboros-memory`

Use this when you want durable memory outside Zo Computer. It gives OpenClaw a real memory backend instead of just ephemeral conversation context: facts, episodes, graph-linked entities, profile summaries, and MCP tools that other agents can query.

### `zouroboros-swarm-gate`

Use this when you want a cheap front door before orchestration. It decides whether a task is simple enough to handle inline or broad enough to justify a multi-agent workflow, which keeps users from over-swarming trivial work.

### `zouroboros-autoloop`

Use this when you have one file, one metric, and a repeatable experiment command. It is best for optimization problems like prompt tuning, strategy tuning, or small performance loops where every iteration can be judged mechanically. The core idea is very close to Andrej Karpathy's `autoresearch`: let the system keep proposing changes, run the experiment, and only keep what improves the metric.

### `zouroboros-bench`

Use this when you want to measure whether a memory system is actually good. It is the comparison harness for running benchmark datasets, generating reports, and checking whether changes improved memory quality or just felt better subjectively.

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

## Part of the Zouroboros Ecosystem

This package is part of the OpenClaw-facing distribution surface at `AlaricHQ/zouroboros-openclaw`. The canonical upstream framework lives at `marlandoj/Zouroboros`.

For the full experience — persistent memory, swarm orchestration, scheduled agents, persona routing, and self-healing infrastructure — get a [Zo Computer](https://zo-computer.cello.so/IgX9SnGpKnR).

## License

MIT
