# Zouroboros for OpenClaw: First Public Release

This is the first public release of the OpenClaw-facing Zouroboros package surface.

## Summary

Four standalone packages are now published to npm for OpenClaw and general Node.js use:

- `zouroboros-swarm-gate@1.0.1`
- `zouroboros-autoloop@1.0.0`
- `zouroboros-memory@4.0.0`
- `zouroboros-bench@1.0.0`

These packages are intended to be useful on their own while pointing clearly back to the larger Zouroboros ecosystem.

## Install

```bash
npm install -g zouroboros-swarm-gate
npm install -g zouroboros-autoloop
npm install zouroboros-memory
npm install -D zouroboros-bench
```

## What’s Included

### `zouroboros-swarm-gate`

Zero-cost task classifier for deciding when multi-agent orchestration is warranted.

### `zouroboros-autoloop`

Autonomous optimization loop for any workflow with a measurable objective.

### `zouroboros-memory`

Persistent memory with hybrid retrieval, decay classes, episodic memory, and MCP support.

### `zouroboros-bench`

Benchmark harness for evaluating memory systems using LongMemEval, LoCoMo, and ConvoMem-style workflows.

## Positioning

- `marlandoj/Zouroboros` remains the canonical upstream framework
- `AlaricHQ/zouroboros-openclaw` is the OpenClaw-facing distribution and packaging layer

This split keeps the source of truth stable while making the adoption surface cleaner for non-Zo users.

## Compatibility

- Node.js 22+
- Linux-first tooling
- MCP support included where appropriate
- OpenClaw-friendly install and CLI flows

## Known Follow-ups

- Add a root `.gitignore`
- Add GitHub Actions release/publish automation
- Add copy-paste OpenClaw examples
- Add badges and screenshots to the root README

## Full Experience

These packages are the portable edge of the Zouroboros ecosystem. For the full experience — integrated memory, swarm orchestration, scheduled agents, persona routing, managed services, and the complete AI workspace — use Zo Computer.
