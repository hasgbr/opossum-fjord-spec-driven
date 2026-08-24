---
name: opossum-fjord-spec-driven
description: Feature planning and implementation that merges tlc-spec-driven (4 adaptive phases: Specify, Design, Tasks, Execute; auto-sized depth; EARS requirements; atomic Conventional Commits; deterministic validation scripts; independent Verifier; lessons layer) with the supreme-broccoli methodology (frontend-first, frequent user checkpoints, documentation mirrors in .specs/, full portability, immediate push after authorization). Writes testable requirements, atomic tasks, and requirement traceability. Stack-agnostic and tool-agnostic. Use when (1) planning features, (2) implementing with verification and atomic commits, (3) validating an implementation against a spec. Triggers on "specify feature", "discuss feature", "design", "tasks", "implement", "validate", "verify work", "UAT", "record decision", "pause work", "resume work". Do NOT use for pure architecture decomposition analysis or standalone technical design documents.
license: CC-BY-4.0
metadata:
  author: Felipe Rodrigues (original tlc-spec-driven) - github.com/felipfr; merged with supreme-broccoli methodology
  version: 1.0.0
---

# Opossum Fjord - Spec-Driven Development

Plan and implement features with precision. Granular tasks. Clear dependencies. Right tools. Zero ceremony.

Merges the **tlc-spec-driven** discipline (spec-driven, deterministic gates, independent Verifier, lessons layer) with the **supreme-broccoli** methodology (frontend-first, frequent checkpoints, `.specs/` documentation mirrors, portability, push-right-after-authorization).

```
┌──────────┐   ┌──────────┐   ┌─────────┐   ┌─────────┐
│ SPECIFY  │ → │  DESIGN  │ → │  TASKS  │ → │ EXECUTE │
└──────────┘   └──────────┘   └─────────┘   └─────────┘
   required      optional*      optional*     required

* Agent auto-skips when scope doesn't need it
```

## Critical Rules (read before acting)

**Loading this skill's files.** Reference files live under `references/` in this skill's own directory (where this `SKILL.md` resides). Resolve them relative to the skill directory - never the workspace root - and load them through the active skill by name; never assume a fixed install path. When a step tells you to read a reference, **read it completely (to EOF)** before acting - never act on a partial/truncated read.

**Running this skill's scripts.** Every `scripts/*.py` shipped with this skill lives under that same skill directory. Resolve the skill directory first, then invoke `python3 <skill-dir>/scripts/<name>.py ...`. Never run `python3 scripts/...` from the consuming project root - that looks for a project-local `scripts/` tree that is not this skill. Project data under `.specs/` is still read/written relative to the project root (pass `--root` when the cwd is elsewhere). Below, `<skill-dir>` means the directory that contains this `SKILL.md`.

**Execution contract - every task, non-negotiable (holds even if you do not open the reference files):**

1. Tests derive from the spec's acceptance criteria and assert spec-defined outcomes - they never mirror the implementation.
2. The gate must pass (tests pass) before a task is done - the test runner decides, not self-assessment.
3. One atomic commit per task. Mark the task complete in `tasks.md` (and update spec traceability when used) **before** that commit, and include those updates in the same commit. Never batch tasks; never weaken, skip, or delete tests to make them pass.
4. After the LAST task, a fresh **Verifier always runs automatically** (author ≠ verifier) - spec-anchored outcome check + discrimination sensor. It is never optional and never prompted. See Sub-Agent Delegation.
5. **Blast radius:** approving a spec or tasks authorizes local implementation and local commits only. `git push`, force-push, deploy, production DB changes, and other remote / externally visible / destructive operations require an explicit go-ahead for that action.
6. **Immediate push after authorization:** once the user authorizes a push, `git push` follows the commit immediately - a commit that exists only locally does NOT count as done. (Same blast-radius rule as tlc: push still needs a go-ahead; but a single go-ahead authorizes the commit's push.) See [repo-docs.md](references/repo-docs.md).

**Deterministic gates run before human review - not from memory.** The structural gates for the spec and tasks are enforced by scripts in this skill's `scripts/` directory, so they cannot silently drift when the model forgets a step:

- Before confirming a spec: `python3 <skill-dir>/scripts/validate_spec.py <spec-path-or-feature>` (closure gate: EARS-shaped ACs, filled assumptions, well-formed requirement IDs, required sections).
- Before presenting tasks for approval: `python3 <skill-dir>/scripts/validate_tasks.py <tasks-path-or-feature>` (granularity smell, diagram-vs-`Depends on` parity within a phase, no forward-phase dependency, every task carries `Tests` + `Gate`).
- On each commit: `python3 <skill-dir>/scripts/check_commit.py --message "<msg>"` (Conventional Commits). Optionally wire it as a git `commit-msg` guard (git only, no agent dependency) - see [implement.md](references/implement.md).
- Before declaring a feature done: `python3 <skill-dir>/scripts/validate_state.py <feature>` (completion gate: the Verifier's `validation.md` exists, its verdict is filled to PASS, and it cites `file:line` evidence - a missing, FAIL, placeholder, or evidence-free report fails). The closing step of Execute runs this automatically, the same way the lessons layer runs at distillation; it is not a manual step.

A non-zero exit means STOP and fix before proceeding. Skip a script only when no code-execution tool is available; then perform the same checks by reading the artifact.

**Before Execute:** read [implement.md](references/implement.md) completely and run `<skill-dir>/scripts/validate_tasks.py`; if a formal `tasks.md` packs into more than one task-budgeted batch (> ~8 tasks), present the sub-agent offer first (see Sub-Agent Delegation).

## Project Kickoff: initial decisions (first interaction)

On the **first interaction with a new project**, before any feature work, ask the user and fix the answers as decisions in `.specs/STATE.md` (see [memory.md](references/memory.md) and [checkpoints.md](references/checkpoints.md)):

1. **Checkpoints on/off** (recommended: on). Cadence of ~2 sub-steps per checkpoint; when on, pause and return control to the user at each checkpoint - never chain sub-step after sub-step without one. Decide with the user; it is recommended, not mandatory.
2. **Frontend-first mode on/off.** Whether to ask, per feature, if the feature is user-facing / has a dual data layer, and if so apply the FE mock → validate → BE real cycle (see [frontend-first.md](references/frontend-first.md)).

These are recorded as project decisions (e.g. `AD-00X`) in `.specs/STATE.md` and honored for the whole project unless superseded.

## Auto-Sizing: The Core Principle

**The complexity determines the depth, not a fixed pipeline.** Before starting any feature, assess its scope and apply only what's needed:

| Scope       | What                     | Specify                                                 | Design                                          | Tasks                         | Execute                                               |
| ----------- | ------------------------ | ------------------------------------------------------- | ----------------------------------------------- | ----------------------------- | ----------------------------------------------------- |
| **Small**   | ≤3 files, one sentence   | One-liner spec (inline)                                 | Skip                                            | Skip                          | Implement + verify inline                             |
| **Medium**  | Clear feature, <10 tasks | Spec (brief)                                            | Skip - design inline                            | Skip - tasks implicit         | Implement + verify                                    |
| **Large**   | Multi-component feature  | Full spec + requirement IDs                             | Architecture + components                       | Full breakdown + dependencies | Implement + verify per task                           |
| **Complex** | Ambiguity, new domain    | Full spec + [discuss gray areas](references/discuss.md) | [Research](references/design.md) + architecture | Breakdown + phase plan        | Implement + [interactive UAT](references/validate.md) |

**Rules:**

- **Specify and Execute are always required** - you always need to know WHAT and DO it
- **Design is skipped** when the change is straightforward (no architectural decisions, no new patterns)
- **Tasks is skipped** when there are ≤3 obvious steps (they become implicit in Execute)
- **Discuss is triggered within Specify** when the agent detects ambiguous gray areas that need user input, or when the feature has any implicit-requirement dimension present (persistence/state, external calls, auth, payments, concurrency, state transitions)
- **Interactive UAT is triggered within Execute** only for user-facing features with complex behavior
- **Frontend-first mode** adds a per-feature question: is this feature user-facing (or does it have a dual data layer)? If yes → apply the FE mock → validate → BE real cycle. See [frontend-first.md](references/frontend-first.md).

**Safety valve:** Even when Tasks is skipped, Execute ALWAYS starts by listing atomic steps inline (see [implement.md](references/implement.md)). If that listing reveals >5 steps or complex dependencies, STOP and create a formal `tasks.md` - the Tasks phase was wrongly skipped.

## .specs Structure (hybrid: tlc + documentation mirrors)

```
.specs/
├── STATE.md            # Project memory: Decisions log (AD-NNN) + Handoff snapshot
├── LESSONS.md          # Self-improving lessons playbook (rendered by scripts/lessons.py - do not hand-edit)
├── lessons.json        # Canonical lessons state (machine-owned)
├── CHANGELOG.md        # MIRROR - changelog per phase/feature (inspired by supreme-broccoli docs/05-etapas-executadas.md)
├── TEST-RESULTS.md     # MIRROR - test outcomes per feature/phase (inspired by supreme-broccoli docs/10-testes.md)
├── CHECKLIST-MANUAL.md # MIRROR - living manual-validation checklist (inspired by supreme-broccoli docs/15-checklist-testes-manuais.md)
├── ADRs/               # MIRROR - AD-001.md... architecture decision records (inspired by supreme-broccoli docs/adr/)
└── features/           # Feature specifications
    └── [feature]/
        ├── spec.md         # Requirements with traceable IDs
        ├── context.md      # User decisions for gray areas (only when discuss is triggered)
        ├── design.md       # Architecture & components (only for Large/Complex)
        ├── tasks.md        # Atomic tasks with verification (only for Large/Complex)
        └── validation.md   # Verifier report: PASS/FAIL, per-AC evidence, sensor result, diff range
```

**Create artifacts lazily.** Write each file only when its phase actually produces content - never scaffold empty `context.md`, `design.md`, or `tasks.md` up front. An empty file signals a phase happened when it did not; absence is the correct state for a skipped phase. The deterministic validators (`scripts/validate_spec.py`, `scripts/validate_tasks.py`, `scripts/check_commit.py`, `scripts/validate_state.py`) ship inside this skill's own `scripts/` directory, alongside `lessons.py`.

**Documentation mirrors are mandatory, not optional** (see [repo-docs.md](references/repo-docs.md)): at the end of each phase (and feature), update `.specs/CHANGELOG.md`, `.specs/TEST-RESULTS.md`, `.specs/CHECKLIST-MANUAL.md`, and add ADRs for decisions - in the same commit as the phase. There is no parallel `docs/` tree in the consuming project; everything lives under `.specs/`.

## Workflow

**New feature:**

1. (Kickoff) decide checkpoints + frontend-first → record in STATE.md
2. Specify → (Design) → (Tasks) → Execute (depth auto-sized)

**Checkpoints:** if checkpoints are on, pause and return control to the user every ~2 sub-steps (see [checkpoints.md](references/checkpoints.md)).

**Resume work:**

1. Read `.specs/STATE.md` (Handoff + Decisions).
2. Reconcile Handoff against git (`branch`, `status --porcelain`, recent commits) and `tasks.md` - evidence wins over a stale snapshot. Full procedure: [memory.md](references/memory.md).
3. Propose the reconciled next step before writing code.

## Context Loading Strategy

**On-demand load (only what the current task needs):**

- `.specs/STATE.md` - Decisions section (read at Design, re-read on resume); Handoff section (read on resume only)
- confirmed lessons - load at Specify and Design via `python3 <skill-dir>/scripts/lessons.py list --status confirmed` ([lessons.md](references/lessons.md)); confirmed only, never candidates
- spec.md (when working on a specific feature)
- context.md (when designing or implementing from user decisions)
- design.md (when implementing from design)
- tasks.md (when executing tasks)

**Never load simultaneously:**

- Multiple feature specs
- Multiple architecture docs

**Target:** <40k tokens total context
**Reserve:** 160k+ tokens for work, reasoning, outputs
**Monitoring:** Display status when >40k (see [context-limits.md](references/context-limits.md))

## Sub-Agent Delegation

**Trigger:** count total tasks. If the feature packs into more than one task-budgeted batch (> ~8 tasks) → offer sub-agents; if it fits a single batch (≤ ~8 tasks) → execute inline.

**Offer-then-confirm** - never auto-spawn. The user must accept before any sub-agent is dispatched.

**One worker per task-budgeted batch (~7 tasks, whole phases):** Phases stay the semantic/dependency unit; a **batch** is the execution unit - one or more *consecutive whole phases* packed to ~7 tasks. Walk phases in order, accumulate whole phases into the current batch until it reaches the budget, then start the next - **never split a phase** across workers. ~20 tasks → ~3 workers; scales linearly (40 → ~6). Each worker executes all its tasks in order (implement → gate → atomic commit), then reports a compact summary (tasks done, commit hashes, test counts, deviations). Batches run sequentially - a batch never starts until the previous one reports all tasks complete. Workers never spawn further sub-agents.

**Verifier (always-on, never prompted):** After the final task is committed, the orchestrator dispatches a fresh Verifier sub-agent automatically - regardless of phase count. Validation never requires a user prompt; it is the closing step of Execute. **Author ≠ verifier**: the Verifier re-derives coverage independently using evidence-or-zero; it does not inherit the author's mental model. The Verifier: (1) performs a **spec-anchored outcome check** - confirms each test's asserted value matches the spec-defined expected outcome, flags spec-precision gaps; (2) runs a **discrimination sensor** - injects behavior-level faults in an isolated scratch (temp worktree or file copies - never `git stash`), confirms tests kill them, discards the scratch and verifies real-tree porcelain matches the pre-sensor baseline; surviving mutants become fix tasks; (3) writes `.specs/features/[feature]/validation.md` (PASS/FAIL, per-AC evidence, sensor result, diff range); (4) returns a compact verdict + ranked gap list to the orchestrator in chat. Gaps become fix tasks; the fix→re-verify loop is bounded to 3 iterations before escalating. (5) **distills lessons** - turns each grounded failure (surviving mutant, spec-precision gap, failed AC, SPEC_DEVIATION) into a reusable project-local lesson via `<skill-dir>/scripts/lessons.py`; a clean PASS records nothing (see [lessons.md](references/lessons.md)).

**Model tier per role (only if the harness supports choosing a model per sub-agent).** Match the reasoning cost to the work instead of paying top-tier reasoning for boilerplate. A batch worker on a mechanical, low-ambiguity phase (entities, config, wiring, straightforward CRUD) runs on a faster/cheaper tier; a worker on a core-domain or high-ambiguity phase, and the Design phase itself, runs on a high-reasoning tier; the Verifier runs on a mid-to-high tier because it does adversarial reasoning and designs mutations. This is a portable recommendation: if the harness cannot set a per-sub-agent model, ignore it. Full rubric in [sub-agents.md](references/sub-agents.md).

**Standalone fallback:** Without sub-agents, run `validate.md` as an independent fresh-eyes pass after the final commit - including the spec-anchored check and discrimination sensor.

Full mechanics (worker payload, compact summary format, failure handling, context sizing, model tier, Verifier report format): [sub-agents.md](references/sub-agents.md).

## Commands

**Feature-level (auto-sized):**
| Trigger Pattern | Reference |
|----------------|-----------|
| Specify feature, define requirements | [specify.md](references/specify.md) |
| Discuss feature, capture context, how should this work | [discuss.md](references/discuss.md) |
| Design feature, architecture | [design.md](references/design.md) |
| Break into tasks, create tasks | [tasks.md](references/tasks.md) |
| Implement task, build, execute | [implement.md](references/implement.md) |
| Validate, verify, test, UAT, walk me through it | [validate.md](references/validate.md) |

**Methodology extensions:**
| Trigger Pattern | Reference |
|----------------|-----------|
| Frontend-first cycle, FE mock first, dual data layer | [frontend-first.md](references/frontend-first.md) |
| Checkpoints, pause every ~2 sub-steps, return control | [checkpoints.md](references/checkpoints.md) |
| .specs documentation mirrors, CHANGELOG/TEST-RESULTS/CHECKLIST/ADR | [repo-docs.md](references/repo-docs.md) |

**Memory:**
| Trigger Pattern | Reference |
|----------------|-----------|
| Record decision, this is a project-level decision | [memory.md](references/memory.md) |
| Pause work, end session, I need to stop | [memory.md](references/memory.md) |
| Resume work, continue, pick up where we left off | [memory.md](references/memory.md) |
| Load lessons, what have we learned, apply past lessons | [lessons.md](references/lessons.md) |
| Record lesson, distill lessons (auto-runs after validation) | [lessons.md](references/lessons.md) |

## Knowledge Verification Chain

When researching, designing, or making any technical decision, follow this chain in strict order. Never skip steps.

```
Step 1: Codebase → check existing code, conventions, and patterns already in use
Step 2: Project docs → README, docs/, inline comments, `.specs/STATE.md` (Decisions)
Step 3: Context7 MCP → resolve library ID, then query for current API/patterns
Step 4: Web search → official docs, reputable sources, community patterns
Step 5: Flag as uncertain → "I'm not certain about X - here's my reasoning, but verify"
```

**Rules:**

- Never skip to Step 5 if Steps 1-4 are available
- Step 5 is ALWAYS flagged as uncertain - never presented as fact
- **NEVER assume or fabricate.** If you cannot find an answer, say "I don't know" or "I couldn't find documentation for this". Inventing APIs, patterns, or behaviors causes cascading failures across design → tasks → implementation. Uncertainty is always preferable to fabrication.

## Portability

Everything that guides decisions lives inside the consuming project's `.specs/` (versioned with the clone). Nothing depends on state that does not travel with `git clone`. Before declaring "everything is documented", audit for real: confirm `.specs/` reflects the current state **and** that nothing relevant lives only outside the repo (session notes, assistant memory, temporary plan files). See [repo-docs.md](references/repo-docs.md).

## Output Behavior

**Do the work; do not narrate the machinery.** Produce the right artifact for the phase instead of announcing the phase ("I will now run the Specify phase"). The user judges the output, not a play-by-play of the process. This keeps the flow from reading as robotic.

**Match effort to the work.** Lightweight steps (feature-level checks, validation, mechanical tasks) do not need top-tier reasoning; heavy steps (complex design, ambiguous features) do. If the harness lets you pick a model per sub-agent, apply the tier rubric in [sub-agents.md](references/sub-agents.md); otherwise proceed and simply invest more care on the heavy steps. Mention this once per session at most, and only if it helps; skip it for an experienced user.

**Write generated artifacts in a plain, decided voice.** Specs, ADRs, validation reports, commit messages, and chat summaries follow the writing rules in [coding-principles.md](references/coding-principles.md): lead with the verdict, state decisions definitively, cut filler and mechanical hedging.

## Code Analysis

Use available tools with graceful degradation. See [code-analysis.md](references/code-analysis.md).
