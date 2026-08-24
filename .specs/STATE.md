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
- **Phase / Task**: Fase A concluída (repos criados + registrados no Orca + handoff versionado). Próximo: Fase B, Etapa 1 (esqueleto da skill).
- **Completed**: Fase A completa — repos `opossum-fjord-spec-driven` (público) e `jolly-rhinoceros` (privado) criados, clonados em `C:\Users\anaal\orca\`, registrados no Orca (ids no docs/PLANO.md), `docs/PLANO.md` + `.specs/STATE.md` escritos (ainda não commitados).
- **In-progress** (file:line): `docs/PLANO.md` (escrito, aguardando commit+push)
- **Next step**: continuar a Fase B dentro do Orca — Etapa 1 (esqueleto): criar `skills/opossum-fjord-spec-driven/SKILL.md`, `README.md`, `LICENSE`, `.gitignore`; commit + push. Ver `docs/PLANO.md`.
- **Blockers**: nenhum
- **Uncommitted files**: `docs/PLANO.md`, `.specs/STATE.md`
- **Branch**: main