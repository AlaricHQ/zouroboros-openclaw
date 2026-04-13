---
name: zouroboros-bench
description: Benchmark harness for AI memory systems. Evaluates LongMemEval, LoCoMo, and ConvoMem datasets against any memory backend via the zouroboros-memory CLI. Includes Mimir institutional-knowledge judge for catching architectural drift.
compatibility: Node.js 22+, OpenClaw Gateway
metadata:
  author: marlandoj.zo.computer
  org: AlaricHQ
---

## Usage

Install: `npm install zouroboros-bench zouroboros-memory`

### Run all benchmarks
```bash
npx zouroboros-bench --limit 50
```

### Run specific benchmark
```bash
npx zouroboros-bench --benchmarks longmemeval --limit 100 --judge
```

### Generate report
```bash
npx zouroboros-bench-report --runs ./data/runs/
```

## Environment Variables
- `ZOUROBOROS_MEMORY_CLI` — Path to memory CLI binary (default: `zouroboros-memory`)
- `ZOUROBOROS_MEMORY_DB` — SQLite DB path for benchmarks
- `OPENAI_API_KEY` — Required for GPT-4o judge
- `OLLAMA_URL` — Ollama URL for local LLM (default: `http://localhost:11434`)
