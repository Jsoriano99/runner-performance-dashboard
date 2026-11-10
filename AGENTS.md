# AGENTS.md — Runner Performance Dashboard (TFG)

Convenciones del proyecto. Las leen Claude Code (vía `CLAUDE.md`), opencode y cualquier persona que
colabore. Si algo aquí contradice a una skill, manda este fichero.

## Contexto

TFG aplicado del Grado en Ingeniería Informática (EPS Córdoba, UCO). Autor: Jorge Soriano Pijuan.
Tutor: Javier Pérez Barea. Objetivo: dashboard que integra datos de entrenamiento de corredores,
detecta patrones y predice la progresión del rendimiento.

**Forma de trabajar: paso a paso.** El alcance se ajusta con el tutor en cada revisión. No se adelanta
trabajo de fases futuras ni se añaden funcionalidades no pedidas. Ante la duda, preguntar.

## Estructura

| Ruta | Contenido |
| --- | --- |
| `memoria-latex/` | Memoria del TFG en LaTeX (plantilla EPSC). Se escribe a la vez que el código |
| `src/` | Paquete Python de la aplicación |
| `tests/` | Pruebas con pytest; datos de prueba sintéticos en `tests/fixtures/` |
| `notebooks/` | Exploración. Lo que funciona se pasa a `src/` con tests |
| `data/` | Datos personales. **Nunca** se versionan |
| `docs/adr/` | Decisiones de arquitectura (una por fichero, numeradas) |
| `.claude/skills/` | Skills de agente |

## Stack (ver `docs/adr/0001-stack-tecnico.md`)

Python 3.12 · pip + venv · pandas · fitparse / gpxpy · SQLite + SQLAlchemy · scikit-learn ·
statsmodels · Dash + Plotly · pytest · ruff · LaTeX (latexmk).

## Comandos

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
ruff check . && ruff format --check .
pytest
cd memoria-latex && latexmk -pdf <principal>.tex
```

## Reglas

1. **Idioma:** código (nombres, funciones) en inglés; comentarios, docstrings, commits, documentación y
   memoria en español.
2. **Datos personales:** nada de `data/` entra en git, ni en notebooks con salidas que muestren
   coordenadas GPS reales. Limpiar salidas antes de commitear.
3. **Memoria viva:** cada avance relevante se refleja en `memoria-latex/` en la misma sesión
   (skill `memoria-latex`) y se anota en el diario de desarrollo (skill `diario-sesion`).
4. **Decisiones:** toda decisión técnica con alternativas reales se registra como ADR en `docs/adr/`.
5. **Git:** commits convencionales en español, una rama por tarea, PR a `main` (skill `git-flujo`).
6. **Calidad:** no se fusiona código sin tests que pasen y sin `ruff` limpio.
7. **Honestidad académica:** no inventar referencias, datos ni resultados. Cada cita de la memoria debe
   existir en el `.bib` y ser verificable.

## Skills disponibles

| Skill | Cuándo usarla |
| --- | --- |
| `memoria-latex` | Al escribir o modificar cualquier parte de la memoria |
| `diario-sesion` | Al cerrar una sesión de trabajo |
| `git-flujo` | Al crear ramas, commits o PR |
| `datos-actividades` | Al leer, transformar o guardar datos de actividades |
| `analisis-modelos` | Al calcular métricas o entrenar/evaluar modelos |
