# STATE

## Decisions

### AD-001

- **Decision**: A skill é genérica (stack-agnostic), com modo frontend-first ativado por pergunta ao usuário, por feature, quando aplicável.
- **Reason**: Preserva a portabilidade do tlc-spec-driven sem perder a metodologia frontend-first do supreme-broccoli; o usuário decide quando o ciclo se aplica.
- **Trade-off**: Requer uma pergunta extra por feature; em features backend-only/infra não se aplica.
- **Scope**: Todas as features planejadas pela skill.
- **Date**: 2026-08-24
- **Status**: active

### AD-002

- **Decision**: Artefatos híbridos (estrutura tlc + espelhos de changelog/testes/lições/ADR) vivem todos dentro de `.specs/`, sem `docs/` paralela no projeto consumidor.
- **Reason**: Mantém a portabilidade (tudo versionado no clone) e integra a disciplina de documentação do supreme-broccoli dentro do modelo do tlc.
- **Trade-off**: Concentra muita estrutura em uma única pasta raiz; exige disciplina para manter os espelhos atualizados por fase.
- **Scope**: Projetos consumidores da skill.
- **Date**: 2026-08-24
- **Status**: active

### AD-003

- **Decision**: A cadência de checkpoints (parar a cada ~2 sub-etapas para validação manual) é recomendada, não obrigatória, e é decidida com o usuário no início de cada projeto, registrada em STATE.md.
- **Reason**: O tlc já tem auto-sizing e UAT; os checkpoints fixos do supreme-broccoli são valiosos mas podem ser excessivos em projetos pequenos — o usuário decide.
- **Trade-off**: Depende de uma decisão explícita no início do projeto; sem ela, cai no fluxo padrão do tlc.
- **Scope**: Projetos consumidores da skill.
- **Date**: 2026-08-24
- **Status**: active

## Handoff

- **Feature**: construção da skill opossum-fjord-spec-driven (Fase B)
- **Phase / Task**: **Fase B CONCLUÍDA** — Etapas 1–6 executadas, commit + push feitos (repo público) e E2E documentada no sandbox `jolly-rhinoceros`.
- **Completed**: Fase A completa. Fase B — Etapa 1 (esqueleto), Etapa 2 (12 references herdados), Etapa 3 (3 novos), Etapa 4 (5 scripts + audit de mirrors), Etapa 5 (E2E no sandbox com feature `todo`), Etapa 6 (`npx skills add` → `~\.agents\skills\` + `.skill-lock.json`; PLANO/README atualizados).
- **In-progress** (file:line): nenhum — Fase B fechada.
- **Next step**: usar a skill em projetos consumidores. Nota conhecida: PromptScript não suporta instalação global (esperado).
- **Blockers**: nenhum
- **Uncommitted files**: `docs/PLANO.md` (status das etapas atualizado, aguardando commit+push)
- **Branch**: main