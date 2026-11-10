# Memoria del TFG (LaTeX)

Basada en la plantilla genérica de TFG de la Escuela Politécnica Superior de Córdoba (Overleaf,
"Plantilla TFG EPSC UCO").

> Pendiente: importar el código fuente de la plantilla (Overleaf → Menu → Download → Source).

## Compilar

```bash
cd memoria-latex
latexmk -pdf <fichero-principal>.tex
```

Los ficheros auxiliares y el PDF no se versionan (ver `.gitignore`).

## Requisitos de contenido (Reglamento 24/2024 TFG EPSC)

- Título, resumen y palabras clave en español **y en inglés** (art. 9.1).
- Declaración de originalidad (Anexo V) en la primera página (art. 9.2 y 18).
- Estructura y extensión según la guía docente (art. 9.1 y 9.4).

Las convenciones de redacción están en `.claude/skills/memoria-latex/SKILL.md`.
