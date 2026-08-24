# .specs/ Documentation Mirrors

**Goal**: Keep a complete, versioned, portable documentation trail inside `.specs/` — no separate
`docs/` tree in the consuming project. Merged from the supreme-broccoli discipline
(`docs/05-etapas-executadas.md`, `docs/10-testes.md`, `docs/15-checklist-testes-manuais.md`,
`docs/adr/`).

## Principle

Everything that guides decisions, records progress, or captures outcomes lives in `.specs/` and
travels with `git clone`. Nothing depends on state outside the repo. Before declaring "everything
is documented", audit for real: confirm `.specs/` reflects the current state AND that nothing
relevant lives only outside the repo (session notes, assistant memory, temporary plan files).

## The mirrors (all under `.specs/`)

| File / dir          | Content (inspired by supreme-broccoli)                | When updated                                    |
| ------------------- | ----------------------------------------------------- | ----------------------------------------------- |
| `CHANGELOG.md`      | Changelog per phase/feature, `YYYY-MM-DD` (`05-etapas-executadas.md`) | end of each phase (same commit)     |
| `TEST-RESULTS.md`   | Test outcomes per module/feature, runner + command + result (`10-testes.md`) | end of each phase / after validation |
| `CHECKLIST-MANUAL.md` | **Living** manual-validation checklist; revisit/expand proactively when a feature ships or a gap is found (`15-checklist-testes-manuais.md`) | continuously; end of each phase |
| `ADRs/AD-NNN.md`    | Architecture decision records (`docs/adr/`)           | when a significant decision is made            |

## Rules

1. **Mandatory per phase, same commit.** Updating the mirrors is not optional and is never deferred
   to a later commit. When a phase ends (and when the feature ends), the mirrors are updated in the
   same commit as the phase work.
2. **No parallel `docs/` tree.** In the consuming project, all documentation lives under `.specs/`.
   (This skill's own repo may keep a `docs/PLANO.md` for its own build plan — that is the skill's
   repo, not a consuming project.)
3. **CHECKLIST-MANUAL.md is a living document.** Revisit/expand it proactively when a new feature is
   delivered or a test gap is found — do not wait for the user to ask. It complements automated
   tests: the manual pass is where human judgment (UI flows, visual design) matters.
4. **CHANGELOG leads with what changed, plainly.** Dates in `YYYY-MM-DD`; group by phase/feature;
   note incidents and their resolutions.
5. **TEST-RESULTS is concrete.** For each module/feature: file, runner, command, and result
   (`✅ N/N passing`), plus what the tests cover.
6. **ADR format** follows the memory-layer decisions style: Status / Contexto / Decisão / Por quê /
   Consequências (or the tlc AD-NNN format in `STATE.md`). Each ADR gets its own file under
   `ADRs/`.

## Frontend-first note

In a frontend-first project, `CHECKLIST-MANUAL.md` is where the FE-mock browser validation is
recorded (both the mock pass and the real-integration pass). See [frontend-first.md](frontend-first.md).

## Push

Because the mirrors live in `.specs/`, they are versioned like any code. Once a push is authorized
(see SKILL.md / [implement.md](implement.md)), the mirrors travel with the commit they accompany.
