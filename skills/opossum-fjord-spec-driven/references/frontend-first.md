# Frontend-First Mode

**Goal**: When a feature is user-facing (or touches a dual data layer), validate the UI against a mock in the browser BEFORE building the real backend, then wire the backend against the contract the UI already validated. Stack-agnostic.

Merged from the supreme-broccoli methodology (see its CLAUDE.md and ADR 0012 - the dual `DataClient`).

## When to apply

At Specify/scope-sizing time, ask the user (when frontend-first mode is on for the project, per the
kickoff decision in `.specs/STATE.md`):

> Is this feature user-facing, or does it have a dual data layer (e.g. `DataClient`/Mock/Http)?

If **yes** → apply the frontend-first cycle below. If **no** (backend-only, infrastructure) → run the
plain tlc flow (no mock-first cycle).

## The cycle (per user-facing feature)

```
1. FE (mock):    UI + interactions against a mock data layer / fixtures - NO backend dependency.
2. Validation:   demonstrate in the browser; iterate with the user until approved.
                 This is a HARD GATE: backend work does not start until the FE mock is approved.
3. BE (real):    implement the real backend module + adapter (the "Http" implementation),
                 adding the corresponding method to the interface so parity holds.
4. Tests + docs: cover the feature (unit/integration), update .specs mirrors
                 (CHANGELOG, TEST-RESULTS, CHECKLIST-MANUAL, ADRs) in the same commit.
```

## Stack-agnostic pattern (the dual-layer invariant)

The pattern that makes frontend-first possible is a **stable interface** between the UI and data,
with **two implementations kept in parity**:

- An interface (e.g. `DataClient`) that the UI depends on - the UI **never** calls the network or a
  persistence layer directly.
- Two implementations of that interface, selected by an environment variable / config flag (e.g.
  `mock` vs `http`), never a conditional scattered through the UI:
  - **Mock** - in-memory fixtures + real domain logic (reuse the real business/calculation code,
    don't reimplement it in a simplified way).
  - **Http/real** - calls the real API/persistence.
- **Parity is an invariant:** a new method on the interface is implemented in BOTH adapters in the
  same commit. There is no "mock only for now" or "http only for now".
- Fixtures are a single source of truth for example data, shared by the mock adapter and the real
  seed, so the two never diverge.

This is the structural enabler: switching mock → http becomes a config change, not a rewrite. The
interface/contract is validated by the UI before the backend exists, so the backend implements
against a contract already proven in practice - not the other way around.

## Interaction with the rest of the skill

- **Checkpoints** (if on) naturally align with this cycle: validate the FE mock at a checkpoint, then
  build the BE, then validate integration at the next checkpoint. See [checkpoints.md](checkpoints.md).
- **Interactive UAT** in [validate.md](validate.md) covers the browser validation; the FE-mock gate
  is the frontend-first-specific addition (validate BEFORE building the backend).
- **Mirrors:** every phase of the cycle updates `.specs/` mirrors (CHANGELOG, TEST-RESULTS,
  CHECKLIST-MANUAL). See [repo-docs.md](repo-docs.md).
- The dual-layer decision itself is a project-level ADR candidate (architecture shape, parity
  invariant). Record it in `.specs/ADRs/`.
