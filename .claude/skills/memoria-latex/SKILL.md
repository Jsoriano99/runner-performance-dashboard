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

La estructura sigue las cinco partes de la propuesta aprobada. No añadir ni quitar capítulos sin
acordarlo con el usuario.

| Fichero | Capítulo | Contenido |
| --- | --- | --- |
| `1_introduccion.tex` | 1 | Contexto, motivación, propuesta, estructura de la memoria |
| `2_objetivos.tex` | 2 | Objetivo general y O1–O5 **literales de la propuesta** |
| `3_antecedentes.tex` | 3 | Plataformas, métricas (TRIMP, ACWR, Riegel…), técnicas, herramientas |
| `4_metodologia.tex` | 4 | Metodología incremental, cronograma (Gantt), recursos, uso de IA |
| `5_analisis_datos.tex` | 5 (parte 1) | Fuentes, formatos FIT/GPX, exploración, métricas, requisitos |
| `6_diseno_modelo_datos.tex` | 6 (parte 2) | Arquitectura, modelo E-R, esquema relacional, ingesta (O5) |
| `7_dashboard.tex` | 7 (parte 3) | Diseño de interfaz, implementación Dash, vistas (O1, O4) |
| `8_modulo_predictivo.tex` | 8 (parte 4) | Patrones, modelos, evaluación, alertas (O2, O3) |
| `9_evaluacion.tex` | 9 (parte 5) | Pruebas, precisión, usabilidad, discusión |
| `10_conclusiones.tex` | 10 | Conclusiones, cumplimiento de objetivos, trabajo futuro |
| `presupuesto.tex` | — | Horas de ingeniería (del diario), amortización, licencias |
| `anexo_diario.tex` | Anexo A | Diario de desarrollo (lo mantiene la skill `diario-sesion`) |
| `anexo_codigo.tex` | Anexo B | Enlace al repositorio y fragmentos clave |
| `anexo_manual_usuario.tex` | Anexo C | Manual de usuario del dashboard |

Cada sección pendiente tiene un `% TODO:` con lo que debe contener: al redactarla, sustituir el TODO.
Un `\cite{}` dentro de un comentario no cuenta: la referencia solo aparece en la bibliografía cuando
se cita en texto real.

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
latexmk        # usa latexmkrc: pdflatex -shell-escape (minted) + makeglossaries
latexmk -c     # borra auxiliares
```

- Fichero principal: `__memoria.tex`. Datos de portada y Anexo V: `_datos_proyecto.tex`.
- Capítulos nuevos: fichero `N_nombre.tex` con `\chapter{}` + `\label{ch:...}`, incluido con `\input` en
  `__memoria.tex` en el bloque «CAPÍTULOS».
- Acrónimos: `\newacronym` en `lista_acronimos.tex`, usar con `\gls{}` en el texto.
- `EJEMPLOS_LATEX.tex` es la chuleta de la plantilla (figuras, subfiguras, tablas, código, TikZ):
  consultarla, no incluirla en el PDF.
- El DNI nunca en git: va en `_datos_privados.tex` (ignorado).

Antes de dar por terminado un cambio: compila sin errores, sin referencias `??` y sin citas
indefinidas (revisar el `.log`). No subir el PDF ni los auxiliares.

## Marcadores

- `% TODO:` para lo pendiente de redactar; `% REVISAR:` para lo que debe validar el tutor.
- No dejar texto de ejemplo de la plantilla en capítulos ya redactados.
