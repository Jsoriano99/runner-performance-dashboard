# Memoria del TFG (LaTeX)

Basada en la plantilla de TFG de la Escuela Politécnica Superior de Córdoba (Eduardo Cañete Carmona,
v3.13, Overleaf «Plantilla TFG - EPSC UCO»). Historial de la plantilla en `__control_versiones.txt`.

## Compilar

Necesitas una distribución LaTeX (TeX Live o MiKTeX) y Python con `pygments` (lo usa `minted` para el
código).

```bash
cd memoria-latex
latexmk        # genera __memoria.pdf
latexmk -c     # limpia auxiliares
```

La configuración está en `latexmkrc` (pdflatex con `-shell-escape` + glosarios). El PDF y los
auxiliares no se versionan.

## Ficheros principales

| Fichero | Contenido |
| --- | --- |
| `__memoria.tex` | Documento principal: orden de las partes y capítulos |
| `_datos_proyecto.tex` | Título (ES/EN), autor, director, grado |
| `0_resumen_y_abstract.tex` | Resumen y palabras clave en español e inglés |
| `1_introduccion.tex`, `2_objetivos.tex`, … | Capítulos |
| `referencias.bib` | Bibliografía |
| `lista_acronimos.tex` | Acrónimos |
| `EJEMPLOS_LATEX.tex` | Chuleta de la plantilla (no se incluye en el PDF) |

## DNI del Anexo V

El repositorio es público: el DNI **no** se escribe en ningún fichero versionado. Crea
`memoria-latex/_datos_privados.tex` (está en `.gitignore`) con:

```latex
\newcommand{\DNI}{12345678X}
```

La primera vez, compila con `latexmk -g` para que lo detecte. Sin ese fichero aparece `[DNI]`.

## Requisitos de contenido (Reglamento 24/2024 TFG EPSC)

- Título, resumen y palabras clave en español **y en inglés** (art. 9.1).
- Declaración de originalidad (Anexo V) en la primera página (art. 9.2 y 18).
- Estructura y extensión según la guía docente (art. 9.1 y 9.4).

Convenciones de redacción: `.claude/skills/memoria-latex/SKILL.md`.
