# ADR 0003 — Configuración de agentes de IA y skills

- Estado: aceptada
- Fecha: 10/11/2026

## Contexto

El desarrollo se apoya en asistentes de IA (Claude Code, opencode). Sin convenciones escritas, cada
sesión parte de cero y el resultado es inconsistente.

## Decisión

- `AGENTS.md` es la fuente única de convenciones. `CLAUDE.md` solo lo importa (`@AGENTS.md`).
- Las skills van en `.claude/skills/<nombre>/SKILL.md`, ruta que leen tanto Claude Code como opencode
  (compatibilidad de proyecto con `.claude/skills`).
- Skills iniciales: `memoria-latex`, `diario-sesion`, `git-flujo`, `datos-actividades`,
  `analisis-modelos`. La skill de interfaz del dashboard se definirá al empezar la fase 2.

## Alternativas descartadas

- **Duplicar en `.opencode/skills/`**: dos copias que se desincronizan.
- **Sin skills, solo `AGENTS.md`**: un fichero único demasiado largo que se carga siempre; las skills se
  cargan solo cuando hacen falta.

## Consecuencias

- El uso de IA queda documentado y es auditable. La memoria debe declarar en qué se ha usado (el
  reglamento no lo regula, pero lo exige la honestidad académica y el art. 18 sobre originalidad).
- Cambiar una convención = editar un fichero y hacer commit.
