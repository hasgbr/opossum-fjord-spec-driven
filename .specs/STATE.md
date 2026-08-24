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
- **Phase / Task**: Fase B, Etapas 1–4 concluídas (esqueleto + 12 references herdados + 3 novos + 5 scripts). Próximo: commit+push das Etapas 1–4, depois Etapa 5 (E2E no sandbox `jolly-rhinoceros`) e Etapa 6 (instalação + finalização).
- **Completed**: Fase A completa. Fase B — Etapa 1 (SKILL.md, README.md, LICENSE CC-BY-4.0, .gitignore), Etapa 2 (12 references herdados; adaptados implement/validate/memory/tasks), Etapa 3 (3 novos: frontend-first, checkpoints, repo-docs), Etapa 4 (5 scripts copiados; validate_state.py com audit soft de mirrors) — todos escritos, aguardando commit+push.
- **In-progress** (file:line): `skills/opossum-fjord-spec-driven/` (conteúdo das Etapas 1–4 escrito, aguardando commit)
- **Next step**: commitar as Etapas 1–4 e, com autorização, fazer push; em seguida Etapa 5 (E2E no sandbox `jolly-rhinoceros`) e Etapa 6 (`npx skills add`, atualizar `docs/PLANO.md` + README, commit+push). Ver `docs/PLANO.md`.
- **Blockers**: nenhum
- **Uncommitted files**: `skills/opossum-fjord-spec-driven/**` (todo o conteúdo das Etapas 1–4)
- **Branch**: main