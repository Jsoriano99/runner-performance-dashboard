# Runner Performance Dashboard

Trabajo Fin de Grado — Grado en Ingeniería Informática, Escuela Politécnica Superior de Córdoba (UCO).

**Desarrollo de un dashboard inteligente para la gestión basada en datos: aplicación al rendimiento de corredores**

- Autor: Jorge Soriano Pijuan
- Tutor: Javier Pérez Barea
- Tipo de TFG: aplicado

## Qué es

Un dashboard interactivo que integra datos de entrenamiento de corredores (Strava / Garmin: ritmo,
frecuencia cardíaca, distancia, potencia, cadencia), detecta patrones de evolución y estima la
progresión del rendimiento con modelos de regresión y series temporales.

## Estructura del repositorio

```
memoria-latex/    Memoria del TFG en LaTeX (plantilla EPSC)
src/              Código de la aplicación (paquete Python)
tests/            Pruebas automáticas (pytest)
notebooks/        Exploración y prototipos (Jupyter)
data/             Datos locales — NO se versionan
docs/adr/         Registro de decisiones de arquitectura
.claude/skills/   Skills de agente (Claude Code y opencode)
AGENTS.md         Convenciones del proyecto para agentes y personas
```

## Puesta en marcha

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements-dev.txt
```

## Estado

Fase 0 — definición del proyecto (stack, convenciones, memoria). Ver [docs/adr](docs/adr).

| Fase | Semanas | Contenido |
| --- | --- | --- |
| 0 | — | Definición: stack, convenciones, plantilla de la memoria |
| 1 | 1–3 | Requisitos, datasets y modelo de datos |
| 2 | 4–6 | Dashboard base e integración de fuentes |
| 3 | 7–8 | Módulo analítico y predictivo |
| 4 | 9–10 | Validación, pruebas de usuario y documentación |
| 5 | 11–12 | Memoria final y defensa |

El alcance de cada fase se revisa con el tutor antes de pasar a la siguiente.
