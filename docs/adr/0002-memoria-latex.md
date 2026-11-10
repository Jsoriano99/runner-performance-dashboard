# ADR 0002 — Memoria en LaTeX dentro del repositorio

- Estado: aceptada
- Fecha: 10/11/2026

## Contexto

La memoria es documentación profesional que crece a la vez que el código. Debe poder editarse con
cualquier herramienta (editor, opencode, Claude Code, Overleaf) y cumplir el Reglamento 24/2024 de TFG
de la EPSC: título, resumen y palabras clave en español e inglés (art. 9.1), declaración de
originalidad (art. 9.2 y 18, Anexo V) y estructura y extensión según la guía docente (art. 9.1 y 9.4).

## Decisión

- La memoria vive en `memoria-latex/`, en el mismo repositorio que el código.
- Se parte de la plantilla genérica de TFG de la EPS de Córdoba publicada en Overleaf
  («Plantilla TFG EPSC UCO»), que ya incluye portada de la escuela y el Anexo V.
- Se compila con `latexmk`; el PDF y los auxiliares no se versionan.
- Las convenciones de redacción están en la skill `memoria-latex`.

## Alternativas descartadas

- **Word / Google Docs**: no versionables con diff legible ni editables por agentes de código.
- **Documento en línea (Notion, Claude Docs)**: cómodo para borradores, pero separado del código y sin
  control de formato final.
- **Plantilla propia desde cero**: más trabajo y riesgo de no cumplir el formato de la escuela.

## Consecuencias

- Cada cambio de la memoria queda en el historial de git junto al código que describe.
- Hay que tener una distribución LaTeX instalada (TeX Live / MiKTeX) o usar Overleaf sincronizado.
