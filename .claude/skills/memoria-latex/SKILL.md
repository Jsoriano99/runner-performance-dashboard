---
name: memoria-latex
description: Redactar o modificar la memoria del TFG en memoria-latex/ (capítulos, figuras, tablas, citas, compilación). Usar siempre que se toque cualquier .tex o .bib de la memoria.
---

# Memoria del TFG en LaTeX

La memoria vive en `memoria-latex/` y usa la plantilla de TFG de la EPS de Córdoba. Se escribe **a la
vez** que se desarrolla: cada sesión con avances deja la memoria al día.

## Requisitos obligatorios (Reglamento 24/2024 TFG EPSC)

- Título, resumen y palabras clave en español **y** en inglés (art. 9.1). El título sin caracteres
  añadidos.
- Declaración de originalidad (Anexo V) en la primera página (art. 9.2 y 18).
- Estructura y extensión según la guía docente (art. 9.1 y 9.4). No cambiar el orden de las partes
  fijas de la plantilla (portada, Anexo V, resumen/abstract, índices).

## Dónde va cada cosa

| Contenido | Capítulo |
| --- | --- |
| Contexto, motivación, estructura del documento | Introducción |
| Objetivos O1–O5 aprobados en la propuesta | Objetivos |
| Wearables, plataformas, métricas (TRIMP, ACWR, Riegel…), herramientas | Antecedentes / estado del arte |
| Fuentes de datos, formatos, requisitos | Análisis y requisitos |
| Arquitectura, modelo de datos, ADRs relevantes | Diseño |
| Ingesta, dashboard | Implementación |
| Métricas, modelos, validación | Módulo analítico y predictivo |
| Pruebas funcionales, precisión, usabilidad | Evaluación |
| Registro por sesión | Anexo «Diario de desarrollo» |

Cada capítulo en su propio `.tex`, incluido desde el fichero principal con `\input`.

## Estilo de redacción

- Español, registro académico, **impersonal** («se ha implementado», no «he implementado»).
- Frases cortas; un párrafo, una idea. Sin adornos ni marketing.
- Cifras con unidades y separador decimal coma en el texto (3,5 km); `siunitx` si la plantilla lo carga.
- Términos en inglés sin traducción habitual en cursiva la primera vez (*dashboard*, *wearable*) y
  definidos como acrónimo si se repiten.
- Las decisiones se justifican: qué alternativas había y por qué se eligió una (enlazar con el ADR).

## Figuras, tablas y código

- Toda figura y tabla con `\caption` y `\label` (`fig:`, `tab:`, `lst:`, `eq:`, `cap:`) y referenciada
  en el texto con `\ref` antes de aparecer.
- Gráficas generadas por código: guardarlas en `memoria-latex/figuras/` como PDF vectorial desde un
  script o notebook versionado; nunca capturas de pantalla de gráficas.
- Capturas del dashboard: PNG en `memoria-latex/figuras/capturas/`, sin datos personales visibles.
- Listados de código cortos (≤ 25 líneas) y solo cuando aportan; el resto se referencia al repositorio.

## Citas y bibliografía

- Todas las referencias en el `.bib` de la plantilla. **No inventar** autores, años, DOIs ni páginas: si
  no se puede verificar la fuente, no se cita y se deja `% TODO: buscar fuente`.
- Clave de cita: `apellidoAÑOpalabra` (p. ej. `riegel1981athletic`).
- Cada afirmación no trivial del estado del arte lleva cita.

## Compilar y comprobar

```bash
cd memoria-latex
latexmk -pdf <principal>.tex
```

Antes de dar por terminado un cambio: compila sin errores, sin referencias `??` y sin citas
indefinidas (revisar el `.log`). No subir el PDF ni los auxiliares.

## Marcadores

- `% TODO:` para lo pendiente de redactar; `% REVISAR:` para lo que debe validar el tutor.
- No dejar texto de ejemplo de la plantilla en capítulos ya redactados.
