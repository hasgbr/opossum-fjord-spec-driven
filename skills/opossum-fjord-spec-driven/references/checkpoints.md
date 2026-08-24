# Checkpoints

**Goal**: Give the user frequent, bounded opportunities to validate progress. Implement ~2 small
sub-steps at a time, then stop and return control. Merged from the supreme-broccoli methodology.

## Decision (project kickoff)

Checkpoints are **recommended, not mandatory**, and are decided with the user on the **first
interaction of the project**. The decision is recorded as an AD entry in `.specs/STATE.md` (e.g.
`AD-00X` — checkpoints on/off) and honored for the whole project unless superseded. See
[memory.md](memory.md).

If the user declines, the flow falls back to the standard tlc flow (no fixed checkpoints).

## Cadence when ON

When implementing a plan with several numbered sub-steps:

- Implement **at most ~2 small sub-steps per checkpoint**, then **STOP and return control to the
  user** so they can validate (e.g. in the browser).
- **Never chain sub-step after sub-step without a checkpoint.**
- A single sub-step that is already large (aggregates many design items) satisfies a checkpoint by
  itself.
- Resume the next pair only after the user reports validation (or explicitly asks to continue).

## Rules

1. **Checkpoints are about returning control, not about code quality gates.** The deterministic
   gates (spec/tasks/commit/state validators) and the Verifier still run on their own schedule; a
   checkpoint is an additional user touchpoint, not a substitute.
2. **Record the cadence decision, not each checkpoint.** Only the on/off kickoff decision lives in
   `STATE.md`. Individual checkpoints do not each need an AD entry.
3. **Never skip a checkpoint because the work "is almost done".** If checkpoints are on, stop and
   hand back control at the cadence even near the end.
4. **A checkpoint is also a natural mirror point.** When handing control back, ensure `.specs/`
   mirrors (CHANGELOG, TEST-RESULTS, CHECKLIST-MANUAL) reflect the completed sub-steps, so the user
   validates against a documented state. See [repo-docs.md](repo-docs.md).

## Frontend-first alignment

In a frontend-first project, checkpoints map cleanly onto the cycle: validate the FE mock at a
checkpoint, then build the BE, then validate the integration at the next checkpoint. See
[frontend-first.md](frontend-first.md).
