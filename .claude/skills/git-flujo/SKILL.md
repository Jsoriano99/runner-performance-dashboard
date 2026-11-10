---
name: git-flujo
description: Convenciones de ramas, commits y pull requests del repositorio del TFG. Usar al crear una rama, hacer un commit, abrir un PR o fusionar a main.
---

# Flujo de Git

## Ramas

- `main`: siempre estable; la memoria compila y los tests pasan.
- Una rama por tarea, creada desde `main` actualizada:
  `<tipo>/<descripcion-corta>` → `feat/lector-fit`, `docs/capitulo-antecedentes`, `chore/paso-0`.
- Ramas cortas: se fusionan en el mismo día o en pocos días.

## Commits (Conventional Commits, en español)

```
<tipo>(<ámbito opcional>): <resumen en imperativo, ≤ 72 caracteres>

<cuerpo opcional: qué y por qué, no cómo>
```

| Tipo | Uso |
| --- | --- |
| `feat` | Funcionalidad nueva |
| `fix` | Corrección de un error |
| `docs` | Memoria, README, ADR |
| `test` | Tests nuevos o corregidos |
| `refactor` | Cambio de código sin cambiar comportamiento |
| `chore` | Configuración, dependencias, estructura |
| `data` | Scripts de preparación de datos (nunca los datos) |

Ámbitos habituales: `memoria`, `ingesta`, `modelo-datos`, `dashboard`, `analisis`, `agentes`.

Ejemplos: `feat(ingesta): leer actividades FIT a DataFrame homogéneo`,
`docs(memoria): redactar capítulo de objetivos`.

Un commit = un cambio con sentido propio. Código y su parte de la memoria pueden ir juntos si
describen el mismo avance.

## Antes de cada commit

- `ruff check . && ruff format --check .` limpio y `pytest` en verde (cuando haya código).
- `git status`: ningún fichero de `data/`, ningún PDF ni auxiliar de LaTeX, ningún `.env`.
- Notebooks sin salidas con datos personales.

## Pull requests

- Título = resumen del cambio en formato commit.
- Descripción: qué cambia, por qué, cómo se ha comprobado, y qué parte de la memoria se ha actualizado.
- Se fusiona con *squash* a `main` y se borra la rama.
