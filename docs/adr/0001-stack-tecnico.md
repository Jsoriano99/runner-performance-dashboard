# ADR 0001 — Stack técnico

- Estado: aceptada
- Fecha: 10/11/2026

## Contexto

La propuesta aprobada prevé Python (Pandas, Plotly, Scikit-learn, Dash), con React, Power BI o
Streamlit como alternativas. El TFG dura unas 12 semanas, lo desarrolla una sola persona y el tutor ha
pedido ajustar el alcance al tiempo disponible.

## Decisión

| Capa | Elección |
| --- | --- |
| Lenguaje | Python 3.12 |
| Entorno y dependencias | `venv` + `pip` con `requirements.txt` / `requirements-dev.txt` |
| Ingesta | `fitparse`, `gpxpy` |
| Persistencia | SQLite con SQLAlchemy (migrable a PostgreSQL) |
| Análisis y predicción | pandas, scikit-learn, statsmodels |
| Interfaz | Dash + Plotly |
| Calidad | pytest, ruff |
| Memoria | LaTeX (latexmk), ver ADR 0002 |

## Alternativas descartadas

- **FastAPI + React**: separación backend/frontend más clásica, pero exige dos lenguajes y dos
  despliegues; se estima en 2–3 semanas más. No aporta valor a los objetivos del TFG.
- **Power BI**: rápido para prototipar pero cerrado, no versionable en git y con integración limitada de
  modelos propios.
- **Streamlit**: más simple que Dash, pero menos control sobre el layout y las interacciones entre
  gráficos (filtros cruzados, callbacks), necesarias para el objetivo O1.
- **uv / Poetry**: más modernos que pip, pero el autor trabaja con pip/venv y no aportan nada decisivo
  a un proyecto de este tamaño.
- **PostgreSQL desde el inicio**: innecesario para un solo usuario local; SQLAlchemy permite migrar sin
  reescribir el modelo.

## Consecuencias

- Todo el sistema en un solo lenguaje: menos fricción y un único conjunto de tests.
- El modelo relacional (objetivo O5) se define con SQLAlchemy y queda documentado en la memoria.
- Las versiones se fijan con `==` al cerrar cada fase para que los resultados sean reproducibles.
