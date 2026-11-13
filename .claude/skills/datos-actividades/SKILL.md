---
name: datos-actividades
description: Leer, limpiar, transformar o guardar datos de actividades de carrera (FIT, GPX, exportaciones de Strava/Garmin) en el TFG. Usar al tocar código de ingesta, el modelo de datos o cualquier fichero de data/.
---

# Datos de actividades

## Privacidad (no negociable)

- `data/` nunca se versiona. Comprobar `git status` antes de cada commit.
- Las coordenadas GPS identifican domicilios: no aparecen en notebooks commiteados, capturas de la
  memoria ni tests. Para tests, datos sintéticos en `tests/fixtures/`.
- En la memoria, los datos reales se muestran agregados o anonimizados.

## Fuentes y formatos

Arquitectura (ADR 0004): un adaptador por fuente en `src/`, función pura ruta → DataFrames con el
esquema canónico de abajo. Solo el adaptador conoce el formato de origen; el resto del sistema nunca
lee ficheros de una fuente concreta.

| Fuente | Formato | Notas |
| --- | --- | --- |
| **Exportación de Apple Health (fuente actual)** | `export.zip` → `export.xml` (~1 GB) + `workout-routes/*.gpx` | Solo `<Workout>` de carrera con origen Apple Watch (ADR 0004). Leer con `iterparse`, nunca entero. FC por muestra agregada: usar la FC media de `<WorkoutStatistics>`. Contiene registros clínicos y ECG: nunca fuera de `data/raw/` |
| Exportación de adidas Running | zip con `Sport-sessions/*.json` | Descartada como fuente (ADR 0004); API cerrada desde el 30/06/2025 |
| Exportación masiva Strava | `activities.csv` + `activities/*.fit.gz`, `*.gpx.gz`, `*.tcx.gz` | Formato según cómo se subió la actividad |
| Exportación Garmin Connect | `.fit` (dentro de zips) | Fuente más completa: FC, cadencia, potencia, dinámica de carrera |
| Descarga suelta | `.fit` / `.gpx` | Para pruebas rápidas |

Prioridad: FIT > TCX > GPX (FIT trae más campos y muestreo más regular).

## Esquema canónico de una muestra

Toda función de ingesta devuelve un DataFrame con estas columnas (las ausentes se omiten, no se
rellenan con ceros):

| Columna | Unidad | Origen / conversión |
| --- | --- | --- |
| `timestamp` | datetime UTC | — |
| `lat`, `lon` | grados | FIT: semicírculos × 180 / 2³¹ |
| `altitude_m` | m | FIT: `enhanced_altitude` si existe |
| `distance_m` | m acumulados | GPX: calcular con haversine |
| `speed_mps` | m/s | FIT: `enhanced_speed` si existe |
| `heart_rate_bpm` | ppm | GPX: extensión `TrackPointExtension/hr` |
| `cadence_spm` | pasos/min | FIT y GPX guardan **por pierna**: × 2 (+ `fractional_cadence` en FIT) |
| `power_w` | W | Solo con potenciómetro o reloj compatible |

Unidades SI en el código; ritmo (min/km) solo en la capa de presentación.

## Calidad de datos

Detectar y documentar, no ocultar:

- Pausas (huecos de tiempo grandes) y auto-pausa.
- Saltos de GPS (velocidades imposibles, p. ej. > 10 m/s corriendo).
- FC a 0 o fuera de rango fisiológico (< 30 o > 230 ppm).
- Actividades que no son carrera (filtrar por deporte).

Cada regla de limpieza: función con test y mención en la memoria (capítulo de análisis/requisitos).

## Código

- Ingesta en `src/` con funciones puras: ruta → DataFrame. Sin efectos secundarios ocultos.
- Lo exploratorio en `notebooks/`; cuando funciona, se pasa a `src/` con tests.
