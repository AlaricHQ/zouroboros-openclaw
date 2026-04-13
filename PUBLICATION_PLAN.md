# Zouroboros → OpenClaw Publication Plan

**Goal:** Publish 4 standalone Zouroboros packages to OpenClaw's ecosystem (ClawHub + npm) to drive awareness. Users discover Zouroboros through OpenClaw skills → want the full experience → get a Zo Computer.

**Strategy:** Standalone npm packages + ClawHub skills. Each package works independently on any Linux box with Node. Transpiled to JS for maximum OpenClaw compatibility (Node 24/22.16+ is their primary runtime). Branding points back to Zouroboros and Zo Computer.

**Decisions (locked 2026-04-13):**
- **GitHub split:** canonical upstream in `github.com/marlandoj/Zouroboros`, OpenClaw-facing distribution in `github.com/AlaricHQ/*`
- **License:** MIT (matches OpenClaw convention)
- **Runtime:** Transpile TypeScript → JS for Node compatibility (OpenClaw majority runs Node, not Bun)
- **MCP servers:** Included — future-proofs for OpenClaw's upcoming native MCP support
- **Pricing:** All open-source, no paywall. Funnel to Zo Computer for the full experience.

---

## Package Overview

| # | Package | ClawHub Slug | npm Package | Portability | Effort |
|---|---------|-------------|-------------|-------------|--------|
| 1 | Swarm Decision Gate | `zouroboros-swarm-gate` | `zouroboros-swarm-gate` | 100% | Trivial |
| 2 | Autoloop | `zouroboros-autoloop` | `zouroboros-autoloop` | 90% | Light |
| 3 | Memory System | `zouroboros-memory` | `zouroboros-memory` | 70% | Medium |
| 4 | ZouroBench | `zouroboros-bench` | `zouroboros-bench` | 65% | Medium |

**Published npm state (verified 2026-04-13):**
- `zouroboros-autoloop@1.0.0`
- `zouroboros-memory@4.0.0`
- `zouroboros-bench@1.0.0`
- `zouroboros-swarm-gate@1.0.1`

---

## Exact Org / Repo Structure

### Canonical upstream: `marlandoj/Zouroboros`

This remains the source of truth for:

- Core framework code
- Zo-native integrations and runtime assumptions
- Full-system documentation
- Research, benchmarks, and architecture decisions
- Shared logic that may be exported into portability packages

Recommended top-level ownership:

- `marlandoj/Zouroboros`
  - canonical monorepo
  - houses the original implementations
  - owns architectural direction
  - owns changelog for the full framework

Recommended directories in the canonical repo:

- `packages/`
  - core Zo-native packages and framework modules
- `portable/` or `exports/openclaw/`
  - extraction layer for code intended to be mirrored into OpenClaw-facing repos
- `docs/`
  - full Zouroboros docs
- `docs/portability/`
  - what ports cleanly, what is Zo-only, and why

### OpenClaw-facing distribution: `AlaricHQ`

This org should exist to make the OpenClaw story obvious and low-friction without confusing users about what is first-party Zo runtime versus portable tooling.

Recommended repos under `AlaricHQ`:

1. `AlaricHQ/zouroboros-openclaw`
   - umbrella compatibility monorepo
   - contains the four standalone packages
   - primary repo linked from npm and ClawHub
   - contains OpenClaw-specific install docs, examples, screenshots, and release notes

2. `AlaricHQ/zouroboros-openclaw-examples`
   - copy-pasteable OpenClaw example projects
   - example `.mcp.json`
   - example skill wiring
   - example memory + benchmark setup
   - example autoloop project layouts

3. `AlaricHQ/.github`
   - shared issue templates
   - discussion templates
   - release checklist
   - security/contact policy

Optional later:

4. `AlaricHQ/zouroboros-docs`
   - only if docs outgrow the code repo
   - avoid this initially; keep docs near the packages first

### Why this split is the better long-term shape

- `marlandoj/Zouroboros` stays authoritative
- `AlaricHQ` becomes the adoption funnel for non-Zo users
- OpenClaw users see tools designed for their workflow, not a repo full of Zo-specific concepts they do not need yet
- You avoid fragmenting the core architecture while still cleanly separating brand surfaces
- The upgrade path is clear: standalone package users can move from `AlaricHQ` to the full `marlandoj/Zouroboros` ecosystem and then to Zo Computer

### What not to do

- Do **not** split the core implementation into two diverging frameworks
- Do **not** make OpenClaw repos the architectural source of truth
- Do **not** bury OpenClaw packages inside a Zo-only monorepo with no compatibility narrative
- Do **not** mix Zo operational code, secrets assumptions, or service-management concepts into the OpenClaw-facing package UX

---

## Wave 1: Quick Wins (swarm-gate + autoloop)

### Package 1: `zouroboros-swarm-gate`

**What it does:** Zero-cost (~2ms) mechanical classifier that scores whether a task warrants multi-agent swarm orchestration. Returns DIRECT / SUGGEST / SWARM / FORCE with a 0–1 confidence score and 7 weighted signals.

**Why OpenClaw users want this:** Anyone running multi-agent setups (CrewAI, AutoGen, or OpenClaw's own multi-persona) wastes tokens on tasks that don't need orchestration. This gate prevents that.

**Current state:** Single file, pure TypeScript, zero dependencies. Already 100% portable.

**Decoupling work:**
- [ ] Remove Bun shebang, add Node-compatible entry point
- [ ] Add `package.json` with npm publish config
- [ ] Write OpenClaw `SKILL.md` with frontmatter
- [ ] Add `--help` documentation
- [ ] Add README with usage examples and architecture diagram

**SKILL.md frontmatter:**
```yaml
---
name: zouroboros-swarm-gate
description: "Zero-cost task classifier (~2ms) that decides if a task needs multi-agent orchestration. 7 weighted signals, no API calls."
version: "1.0.0"
metadata:
  openclaw:
    emoji: "⚡"
    requires:
      bins: [node]
    homepage: https://github.com/AlaricHQ/zouroboros-openclaw
---
```

**CLI interface:**
```bash
# Via npx
npx zouroboros-swarm-gate "build a REST API with auth and tests"
npx zouroboros-swarm-gate --json "fix the typo on line 42"

# Exit codes: 0=SWARM, 2=DIRECT, 3=SUGGEST, 1=ERROR
```

---

### Package 2: `zouroboros-autoloop`

**What it does:** Autonomous single-metric optimization loop. Reads a `program.md` spec, edits a target file, runs an experiment, measures a metric, keeps improvements (git commit), reverts regressions (git reset). Loops until convergence or budget exhausted.

**Why OpenClaw users want this:** Prompt optimization, trading backtests, site performance tuning, model fine-tuning — any task with a clear numeric metric. Works with any LLM provider.

**Current state:** 90% portable. Core algorithm is standalone. Only needs path parameterization.

**Decoupling work:**
- [ ] Remove hard-coded `/home/workspace/` paths from MCP server
- [ ] Add `--script-dir` and `--results-dir` CLI args
- [ ] Make `ZO_AUTOLOOP_MCP_TOKEN` → `AUTOLOOP_MCP_TOKEN` (drop ZO_ prefix)
- [ ] Add `package.json` with npm publish config
- [ ] Write OpenClaw `SKILL.md`
- [ ] Add README with example `program.md` templates

**SKILL.md frontmatter:**
```yaml
---
name: zouroboros-autoloop
description: "Autonomous optimization loop: edit → experiment → measure → keep/revert → repeat. For prompt tuning, backtests, performance optimization."
version: "1.0.0"
metadata:
  openclaw:
    emoji: "🔄"
    requires:
      bins: [node]
      bins: [git]
    homepage: https://github.com/AlaricHQ/zouroboros-openclaw
---
```

---

## Wave 2: Flagship (zo-memory)

### Package 3: `zouroboros-memory`

**What it does:** Production-grade persistent memory with episodic/procedural recall, cognitive profiling, memory gate (should I store this?), decay classes, persona routing, Hebbian reinforcement, hybrid retrieval (FTS + vector + graph), conflict resolution, and cross-persona knowledge sharing.

**Why OpenClaw users want this:** OpenClaw's built-in memory is basic vector search. This is a generational leap — gate, decay, reinforcement, graph traversal, multi-hop reasoning. The OpenClaw community actively discusses memory quality problems.

**Current state:** 70% portable. Core logic is solid but tightly coupled to Zo environment.

**Decoupling work:**

**Phase A — Extract types (remove zouroboros-core dependency):**
- [ ] Copy `MemoryConfig`, `MemoryEntry`, `EpisodicMemory`, `TemporalQuery`, `DecayClass`, `CognitiveProfile` types into standalone `types.ts`
- [ ] Remove `import from 'zouroboros-core'` throughout

**Phase B — Config abstraction:**
- [ ] Create `MemoryConfig` loader that reads from:
  1. CLI flags (highest priority)
  2. `zouroboros-memory.json` config file
  3. Environment variables (renamed: `ZO_MEMORY_DB` → `ZOUROBOROS_MEMORY_DB`)
  4. Sensible defaults (`~/.zouroboros/memory/facts.db`)
- [ ] Consolidate 20+ scattered `process.env.ZO_*` reads into single config object
- [ ] Replace hard-coded `/home/workspace/.zo/` paths with `~/.zouroboros/` defaults

**Phase C — Ollama abstraction:**
- [ ] Make Ollama optional — FTS-only mode when no embedding server available
- [ ] Add `--no-vectors` flag for environments without Ollama
- [ ] Support configurable embedding endpoint (not just localhost:11434)

**Phase D — Remove executor history dependency:**
- [ ] `cognitive_profile` MCP tool reads `.swarm/executor-history.json`
- [ ] Make this an optional enrichment, not a hard requirement

**Phase E — Packaging:**
- [ ] Add `package.json` with npm publish config
- [ ] Dual entry: CLI (`zouroboros-memory`) + MCP server (`zouroboros-memory-mcp`)
- [ ] Write OpenClaw `SKILL.md`
- [ ] Write comprehensive README with architecture diagram
- [ ] Add quick-start guide (5-minute setup)

**SKILL.md frontmatter:**
```yaml
---
name: zouroboros-memory
description: "Production-grade AI memory: episodic recall, decay classes, memory gate, hybrid retrieval (FTS + vector + graph), cognitive profiling. Replaces basic vector search."
version: "1.0.0"
metadata:
  openclaw:
    emoji: "🧠"
    requires:
      bins: [node]
      bins: [sqlite3]
    primaryEnv: OLLAMA_URL
    install:
      - id: node-zouroboros-memory
        kind: node
        package: "zouroboros-memory"
        bins: [zouroboros-memory, zouroboros-memory-mcp]
        label: "Install Zouroboros Memory (npm)"
    homepage: https://github.com/AlaricHQ/zouroboros-openclaw
---
```

**CLI interface:**
```bash
# Store a fact
zouroboros-memory store "User prefers dark mode" --decay stable

# Search
zouroboros-memory search "user preferences" --limit 5

# Hybrid search (FTS + vector + graph)
zouroboros-memory hybrid "what does the user like" --limit 10

# Memory gate (should I store this?)
zouroboros-memory gate "The weather is nice today"
# Exit 0: store, Exit 2: skip

# MCP server (for OpenClaw integration)
zouroboros-memory-mcp --db ~/.zouroboros/memory/facts.db
```

---

## Wave 3: Ecosystem (ZouroBench)

### Package 4: `zouroboros-bench`

**What it does:** Benchmark harness for AI memory systems. Adapters for LongMemEval, LoCoMo, ConvoMem datasets. Mimir judge for semantic accuracy scoring. Three-stage evaluation: mechanical → semantic → consensus.

**Why OpenClaw users want this:** No standardized way to measure memory quality in OpenClaw. This gives the community a benchmark to compare memory implementations — and Zouroboros memory will score highest.

**Current state:** 65% portable. Tightly coupled to zo-memory CLI subprocess calls.

**Decoupling work:**
- [ ] Replace CLI subprocess calls (`bun memory.ts store`) with direct function imports from `@zouroboros/memory`
- [ ] Abstract database initialization into shared schema builder
- [ ] Standardize env vars (drop `ZO_` prefix)
- [ ] Make Mimir judge work with any memory database (accept schema as config)
- [ ] Add `package.json` with npm publish config
- [ ] Write OpenClaw `SKILL.md`
- [ ] Bundle seed datasets or provide download script

**SKILL.md frontmatter:**
```yaml
---
name: zouroboros-bench
description: "Benchmark harness for AI memory systems. LongMemEval, LoCoMo, ConvoMem adapters. Measure recall accuracy, not vibes."
version: "1.0.0"
metadata:
  openclaw:
    emoji: "📊"
    requires:
      bins: [node]
    install:
      - id: node-zouroboros-bench
        kind: node
        package: "zouroboros-bench"
        bins: [zouroboros-bench]
        label: "Install ZouroBench (npm)"
    homepage: https://github.com/AlaricHQ/zouroboros-openclaw
---
```

---

## Distribution Strategy

### Dual-channel: ClawHub + npm

Each package is published to **both**:

1. **npm** (`zouroboros-*`) — For programmatic use, CI/CD, and users who prefer npm
2. **ClawHub** (`zouroboros-*`) — For OpenClaw native discovery and installation

### Branding & Funnel

Every package README and SKILL.md includes:

```markdown
## Part of the Zouroboros Ecosystem

Zouroboros is a self-improving AI orchestration framework. These standalone
packages give you a taste of what's possible. For the full experience —
persistent memory, swarm orchestration, scheduled agents, persona routing,
and self-healing infrastructure — get a [Zo Computer](https://zo.computer).

Built by [@Xmarlandoj](https://x.com/Xmarlandoj)
```

### GitHub Repositories

- **Canonical upstream:** `github.com/marlandoj/Zouroboros`
- **OpenClaw distribution repo:** `github.com/AlaricHQ/zouroboros-openclaw`
- **Examples repo:** `github.com/AlaricHQ/zouroboros-openclaw-examples`
- OpenClaw repo is a packaging/distribution surface, not the architectural source of truth
- Each package has its own `package.json`, `SKILL.md`, and `README.md`
- CI publishes to npm + syncs to ClawHub on tag push from the OpenClaw distribution repo

---

## Immediate Next Step: Repo-side Publish Surface

Now that npm publish is complete, the next sensible wave is the public repo surface that OpenClaw users will actually encounter.

### 1. Install docs

Create a strong root README in `AlaricHQ/zouroboros-openclaw`:

- one-paragraph explanation of what Zouroboros is
- explicit distinction:
  - `marlandoj/Zouroboros` = canonical framework
  - `AlaricHQ/zouroboros-openclaw` = OpenClaw-compatible packages
- install matrix for all four packages
- "start here" recommendation by user type:
  - orchestration users → swarm-gate
  - optimization users → autoloop
  - memory users → zouroboros-memory
  - evaluators/researchers → zouroboros-bench
- one short section: "When you should use Zo Computer instead"

### 2. OpenClaw usage examples

Ship concrete examples, not just API docs:

- `examples/swarm-gate/basic-routing.md`
- `examples/autoloop/prompt-optimizer/`
- `examples/autoloop/backtest-loop/`
- `examples/memory/.mcp.json`
- `examples/memory/openclaw-skill.md`
- `examples/bench/memory-shootout/`

Each example should include:

- install command
- exact run command
- expected output
- common failure modes
- why the example matters

### 3. Package README polish

Every package README should be aligned to the actual publish surface:

- use unscoped npm names
- mention the GitHub repo as `AlaricHQ/zouroboros-openclaw`
- include OpenClaw-specific quick start near the top
- include a "Part of the Zouroboros ecosystem" section
- include "Full experience on Zo Computer" callout without overselling

Current gaps to close:

- `bench` is missing a README entirely
- `swarm-gate` and `autoloop` READMEs still reference scoped package names
- `memory` README still references scoped imports and old env naming in places

### 4. GitHub release polish

For the first public release in `AlaricHQ/zouroboros-openclaw`:

- tag the repo after README cleanup
- create release notes with:
  - package list
  - npm install commands
  - OpenClaw compatibility statement
  - MCP support statement
  - upgrade path to full Zouroboros / Zo Computer
- add badges:
  - npm version
  - license
  - Node version

### 5. Repo hygiene before public push

- add root `README.md`
- add root `LICENSE`
- add root `CHANGELOG.md`
- add root `.gitignore`
- add GitHub Actions publish workflow
- add ClawHub sync workflow or documented manual release path
- verify every package has:
  - `README.md`
  - `SKILL.md`
  - `LICENSE`
  - install instructions that match npm reality

---

## Release Timeline

| Wave | Packages | Target |
|------|----------|--------|
| Wave 1 | swarm-gate, autoloop | Week 1 |
| Wave 2 | memory | Weeks 2–3 |
| Wave 3 | bench | Week 4 |

### Launch Sequence Per Package

1. Decouple from Zo-specific dependencies
2. Add standalone `package.json`, `SKILL.md`, `README.md`
3. Test on clean Linux box (no Zo) with Node + Bun
4. Publish to npm (`npm publish --access public`)
5. Publish to ClawHub (`clawhub skill publish`)
6. Post announcement on X with demo
7. Cross-post to r/AI_Agents, OpenClaw Discord

---

## Marketing Angle

**Lead message:** "We built production-grade memory, optimization, and orchestration for our own AI system. Now it's available as standalone tools for OpenClaw."

**Key differentiators to emphasize:**
- **Memory gate** — Not everything should be stored. Our gate decides (no API cost).
- **Decay classes** — Facts expire. Permanent, stable, active, ephemeral.
- **Zero-cost task classification** — 2ms, no tokens, no API calls.
- **Autonomous optimization** — Set a metric, walk away, come back to improvements.
- **Benchmark your memory** — Stop guessing if your memory works. Measure it.

**Social proof:** "Zouroboros memory scores 65-70% on LongMemEval (production parity). ZouroBench custom benchmark: 97.8%."

---

## Decisions Log

| Question | Decision | Rationale |
|----------|----------|-----------|
| GitHub org | `AlaricHQ` | User's brand |
| License | MIT | Matches OpenClaw convention, maximum adoption |
| Runtime target | Node (transpiled JS) | OpenClaw Gateway runs Node 24/22.16+; majority of community uses Node |
| MCP servers | Include | Future-proofs for OpenClaw's upcoming native MCP; also usable with Claude Desktop today |
| Pricing | All open-source | Maximum reach; Zo Computer is the upsell for the full integrated experience |

## Build Toolchain

Each package uses:
- **Source:** TypeScript (developed in Bun as usual)
- **Build:** `tsup` or `tsc` → ES modules + CJS dual output
- **Target:** Node 22+ (ES2022)
- **Test:** Vitest (Node-native, no Bun dependency)
- **Publish:** `npm publish --access public` + `clawhub skill publish`
- **CI:** GitHub Actions on `AlaricHQ/zouroboros-openclaw` — test → build → publish on tag push
