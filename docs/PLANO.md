# Plano — skill customizada `opossum-fjord-spec-driven`

> Este documento é o **handoff versionado** para a continuação do trabalho dentro do Orca.
> Nada do que orienta as decisões vive fora deste repo (portabilidade 100% — mesma regra do
> supreme-broccoli). A execução da Fase B acontece num worktree/terminal do Orca, mas o plano
> viaja com o clone.

## Objetivo

Criar uma skill para opencode que mescla **tlc-spec-driven** (base) com a **metodologia do
supreme-broccoli** (frontend-first, checkpoints, espelhos de docs em `.specs/`, portabilidade,
push imediato após autorização).

## Repos (Fase A — concluída)

| Repo | Visibilidade | Caminho local | Orca repo id |
| --- | --- | --- | --- |
| `hasgbr/opossum-fjord-spec-driven` | público | `C:\Users\anaal\orca\opossum-fjord-spec-driven` | `6b1da693-bca1-40aa-a137-3129fbcb5802` |
| `hasgbr/jolly-rhinoceros` | privado | `C:\Users\anaal\orca\jolly-rhinoceros` | `08313540-2fde-4ed7-b30f-248ee689ae16` |

## Decisões de design (fechadas com o usuário)

1. **Genérica/stack-agnostic**, com **modo frontend-first sempre perguntando** ao usuário por feature quando aplicável.
2. **Artefatos híbridos, tudo em `.specs/`**: estrutura tlc (`STATE.md` + `features/[feature]/`) + espelhos (CHANGELOG/TEST-RESULTS/CHECKLIST-MANUAL/ADRs). Sem `docs/` paralela no projeto consumidor.
3. **Checkpoints opcionais/recomendados**, **decididos com o usuário no início do projeto** (registrados como decisão em `.specs/STATE.md`).
4. **Nome**: `opossum-fjord-spec-driven` (repo público). Sandbox: `jolly-rhinoceros` (privado).
5. **Licença**: CC-BY-4.0 (herdada do tlc-spec-driven, com attribution ao autor original `felipfr`).

## Estrutura-alvo do repo da skill

```
opossum-fjord-spec-driven/
├── skills/opossum-fjord-spec-driven/
│   ├── SKILL.md                    # frontmatter + regras mescladas
│   ├── LICENSE                     # CC-BY-4.0
│   ├── references/                 # 12 herdados + 3 novos
│   │   ├── specify.md  discuss.md  design.md  tasks.md
│   │   ├── implement.md  validate.md  memory.md  lessons.md
│   │   ├── sub-agents.md  coding-principles.md  context-limits.md  code-analysis.md
│   │   ├── frontend-first.md       # NOVO — ciclo FE mock→validar→BE real→testes+docs
│   │   ├── checkpoints.md          # NOVO — cadência ~2 sub-etapas (recomendada, decisão inicial)
│   │   └── repo-docs.md            # NOVO — espelhos em .specs/ (CHANGELOG/TEST-RESULTS/CHECKLIST/ADRs)
│   └── scripts/                    # validate_spec.py validate_tasks.py check_commit.py validate_state.py lessons.py
├── docs/
│   └── PLANO.md                    # este arquivo
├── README.md                       # como instalar/usar (npx skills add)
└── .gitignore
```

**Fonte dos arquivos herdados:** `C:\Users\anaal\.cache\agent-skills\skills\tlc-spec-driven\`
(contém `SKILL.md`, `references/` com 12 arquivos, `scripts/` com 5 Python). Ver também o repo
origem público: `tech-leads-club/agent-skills` → `packages/skills-catalog/skills/(development)/tlc-spec-driven/`.

## Estrutura `.specs/` no projeto consumidor

```
.specs/
├── STATE.md             # Decisions (AD-NNN) + Handoff
├── LESSONS.md + lessons.json       # lições (script-owned)
├── CHANGELOG.md         # NOVO — espelho por fase (inspirado em docs/05-etapas-executadas.md)
├── TEST-RESULTS.md      # NOVO — espelho de testes (inspirado em docs/10-testes.md)
├── CHECKLIST-MANUAL.md  # NOVO — roteiro de validação manual, vivo (inspirado em docs/15-checklist-testes-manuais.md)
├── ADRs/                # NOVO — AD-001.md... (inspirado em docs/adr/)
└── features/[feature]/  # spec.md context.md design.md tasks.md validation.md
```

## Regras novas na skill (mesclagem)

1. **Frontend-first mode (perguntar por feature).** Ao dimensionar o escopo, perguntar se o repo tem
   dupla camada de dados (`DataClient`/Mock/Http) ou se a feature é user-facing. Se sim → ciclo
   FE mock → validar no navegador → BE real + adapter → testes+espelhos. Se não → tlc puro.
2. **Checkpoints (recomendada, decidida no início).** Na primeira interação do projeto, perguntar se
   quer checkpoints a cada ~2 sub-etapas; se sim, parar e devolver o controle; nunca encadear etapa
   sem checkpoint. Fixar como decisão em `.specs/STATE.md`.
3. **Espelhos de docs em `.specs/` (obrigatório por fase, mesmo commit).** CHANGELOG/TEST-RESULTS/
   CHECKLIST-MANUAL/ADRs atualizados ao fim de cada fase — não é opcional.
4. **Portabilidade reforçada.** Tudo versionado em `.specs/`; nada de estado fora do clone; auditar
   antes de declarar "tudo documentado".
5. **Git push.** Mantém blast radius do tlc (push exige go-ahead), mas uma vez autorizado, push segue
   o commit imediatamente (commit local-only não conta como feito).

## Fase B — Etapas de execução (dentro do Orca)

> **Status: Fase B CONCLUÍDA (2026-08-24)** — todas as etapas executadas, commits + push feitos.

### Etapa 1 — Esqueleto ✅
Criar `skills/opossum-fjord-spec-driven/SKILL.md` (frontmatter + regras mescladas), `README.md`
(instalação via `npx skills add <repo> --skill opossum-fjord-spec-driven`), `LICENSE` (CC-BY-4.0,
attribution `felipfr`), `.gitignore`. Commit + push.

### Etapa 2 — References herdados (12) ✅
Copiar de `C:\Users\anaal\.cache\agent-skills\skills\tlc-spec-driven\references\` e adaptar:
- `implement.md` — integrar checkpoints (referencia `checkpoints.md`) e espelhos (`repo-docs.md`).
- `validate.md` — adicionar UAT no navegador quando frontend-first.
- `memory.md` — registrar decisões iniciais do projeto (checkpoints on/off, frontend-first on/off).
- `tasks.md` — matrix de testes referencia `.specs/TEST-RESULTS.md` e `.specs/CHECKLIST-MANUAL.md`.
- Demais 8: copiar sem alteração (specify, discuss, design, lessons, sub-agents, coding-principles, context-limits, code-analysis).

Commit + push.

### Etapa 3 — References novos (3) ✅
- `frontend-first.md` — ciclo FE mock → validar navegador → BE real + adapter → testes+espelhos.
  Stack-agnostic (ex.: interface + 2 implementações, `mock`/`http`, chaveada por env).
- `checkpoints.md` — cadência ~2 sub-etapas, decidida no início do projeto, registrada em STATE.md.
- `repo-docs.md` — espelhos em `.specs/`: CHANGELOG, TEST-RESULTS, CHECKLIST-MANUAL (vivo), ADRs.

Commit + push.

### Etapa 4 — Scripts (5) ✅
Copiar de `C:\Users\anaal\.cache\agent-skills\skills\tlc-spec-driven\scripts\`. Adaptar
`validate_state.py` se necessário para cobrir os novos artefatos. Testar cada script localmente
(`python3 <script> --help` ou validações de exemplo). Commit + push.
- Adaptado: `validate_state.py` ganhou audit **soft** (WARNING não-bloqueante) dos mirrors
  (CHANGELOG/TEST-RESULTS/CHECKLIST-MANUAL) quando a feature passa — mantém a semântica do gate.

### Etapa 5 — Validação E2E no sandbox (`jolly-rhinoceros`) ✅
Rodar a skill numa feature pequena: Specify → checkpoint → Execute → Verifier → espelhos atualizados.
Confirmar scripts determinísticos e lessons layer. Documentar resultado em `.specs/` do sandbox.
- Feature `todo` exercitada: `validate_spec.py` 0/0, `validate_tasks.py` 0/0, `validate_state.py`
  PASS + mirrors presentes (0 WARNING), `lessons.py` init/add/status/list OK (L-001 candidata).
- Resultado commitado e pushado em `hasgbr/jolly-rhinoceros` (`.specs/`).

### Etapa 6 — Instalação + finalização ✅
- `npx skills add <repo> --skill opossum-fjord-spec-driven` (popula `~/.agents/skills/` + `.skill-lock.json`).
- Atualizar `docs/PLANO.md` (status das etapas) e README final. Commit + push.
- Instalado em `~\.agents\skills\opossum-fjord-spec-driven` (universal + symlink Claude Code).
  Nota: PromptScript não suporta instalação global — esperado, não é falha da skill.

## Ordem de checkpoints (Fase B) — todos concluídos

- CP2 — Etapa 1–2 (esqueleto + herdados) ✅
- CP3 — Etapa 3–4 (novos + scripts) ✅
- CP4 — Etapa 5–6 (E2E + instalação) ✅

## Referências úteis

- Skill original local: `C:\Users\anaal\.cache\agent-skills\skills\tlc-spec-driven\`
- Skill original no GitHub: `tech-leads-club/agent-skills` → `packages/skills-catalog/skills/(development)/tlc-spec-driven/`
- Instalação de skills: `npx skills add <repo> --skill <nome>` (vercel-labs/skills; popula `~/.agents/skills/`)
- Metodologia do supreme-broccoli: `C:\Users\anaal\orca\supreme-broccoli\CLAUDE.md` + `docs/`