## 2026-05-22 - Spec del agente arquitecto

- Se agrego `.claude/agents/arquitecto.md` con la spec operativa del agente custom `arquitecto`.
- Se hizo para definir objetivo, scope, criterios verificables y no-goals antes de construir o usar el agente.
- Cubre el riesgo de que el agente tome decisiones por el humano, implemente codigo o proponga ADRs sin trade-offs claros.

## 2026-05-22 - Agente Claude Code arquitecto

- Se convirtio `.claude/agents/arquitecto.md` al formato valido de agente Claude Code con frontmatter YAML y system prompt.
- Se hizo para que Claude pueda invocarlo automaticamente cuando el usuario pida ADRs, decisiones arquitectonicas o trade-offs tecnicos.
- Cubre el riesgo de invocacion ambigua, propuestas sin evidencia en `spec.md` o `AGENTS.md`, y decisiones tomadas por el agente en lugar del humano.

## 2026-05-22 - Correccion de inconsistencias del agente arquitecto

- Se ajusto `.claude/agents/arquitecto.md` para leer tambien `CONTEXT.md`, usar evidencia exacta, limitar ADRs y cerrar con `¿Cuál eliges?`.
- Se hizo para convertir el agente en un artefacto minimalista y usable sin inconsistencias de formato, alcance ni encoding.
- Cubre el riesgo de ADRs inventados, propuestas demasiado amplias, huecos bloqueadores sin preguntas de desbloqueo y cierres ambiguos.
