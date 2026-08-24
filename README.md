# opossum-fjord-spec-driven

Uma skill para opencode que mescla **tlc-spec-driven** (base) com a **metodologia supreme-broccoli**
(frontend-first, checkpoints, espelhos de documentação em `.specs/`, portabilidade, push imediato
após autorização).

- **Fonte da metodologia/skill original:** `tlc-spec-driven` — `felipfr` (github.com/felipfr),
  licença **CC-BY-4.0**. Ver `skills/opossum-fjord-spec-driven/LICENSE`.
- **Fonte da metodologia supreme-broccoli:** `hasgbr/supreme-broccoli` (privado) — ciclo
  frontend-first, checkpoints de ~2 sub-etapas, espelhos de docs em `.specs/`.

## O que a skill faz

Planejamento e implementação de features com precisão: 4 fases adaptativas (Specify, Design, Tasks,
Execute), testes derivados do spec, commits atômicos (Conventional Commits), validadores
determinísticos em Python, Verifier independente (autor ≠ verificador), camada de lições
auto-melhorada — e, em cima disso, a disciplina do supreme-broccoli:

1. **Frontend-first** — quando a feature é user-facing / tem dupla camada de dados, ciclo
   FE (mock) → validar no navegador → BE real → testes + espelhos.
2. **Checkpoints** — cadência de ~2 sub-etapas, decidida no início do projeto.
3. **Espelhos de docs em `.specs/`** — CHANGELOG, TEST-RESULTS, CHECKLIST-MANUAL (vivo) e ADRs,
   atualizados por fase, no mesmo commit.
4. **Portabilidade** — tudo versionado em `.specs/`; nada de estado fora do clone.
5. **Push imediato** — uma vez autorizado, `git push` segue o commit na hora.

## Instalação

```bash
npx skills add hasgbr/opossum-fjord-spec-driven --skill opossum-fjord-spec-driven
```

Popula `~/.agents/skills/` + `.skill-lock.json`.

## Estrutura

```
skills/opossum-fjord-spec-driven/
├── SKILL.md                    # frontmatter + regras mescladas
├── LICENSE                     # CC-BY-4.0 (attribution felipfr)
├── references/                 # 12 herdados do tlc + 3 novos
│   ├── specify.md  discuss.md  design.md  tasks.md
│   ├── implement.md  validate.md  memory.md  lessons.md
│   ├── sub-agents.md  coding-principles.md  context-limits.md  code-analysis.md
│   ├── frontend-first.md       # NOVO
│   ├── checkpoints.md          # NOVO
│   └── repo-docs.md            # NOVO
└── scripts/                    # validate_spec.py validate_tasks.py check_commit.py validate_state.py lessons.py
```

## Licença

CC-BY-4.0 — com attribution ao autor original do `tlc-spec-driven`, **felipfr**
(github.com/felipfr). Ver `skills/opossum-fjord-spec-driven/LICENSE`.
