# ADR 0004 — Ingesta desacoplada mediante adaptadores; fuente única: entrenos del Apple Watch (exportación de Apple Health)

- Estado: aceptada
- Fecha: 12/11/2026 (revisada el mismo día, antes de fusionar, al disponer de la exportación de Apple Health)

## Contexto

El autor no tiene datos en Strava ni en Garmin Connect, las fuentes previstas al definir el stack
(ADR 0001). Sus carreras están registradas en varias aplicaciones que vuelcan sus sesiones en la app
Salud de Apple. Un inventario preliminar de la exportación de Salud (12/11/2026) muestra:

| Origen (`sourceName`) | Carreras | Frecuencia cardiaca | Otros datos |
| --- | --- | --- | --- |
| Apple Watch (entreno nativo) | 71 | FC media de la sesión en 70; las lecturas individuales vienen agregadas en la exportación | Dinámica de carrera en ~39, ruta GPX en 40, 31 en cinta |
| adidas Running | 42 | Lecturas cada ~5 s | Sin ruta en Salud |
| Nike Run Club | 10 | Lecturas cada ~3 s | — |
| Strava | 5 | Escasa | Ruta GPX |

- Total: 128 carreras entre 03/2023 y 11/2026, con 17 solapes temporales: la misma salida registrada a
  la vez por el reloj y por otra aplicación.
- adidas cerró su API pública el 30/06/2025; su exportación de cuenta solo cubre las sesiones de su app.
- Se quiere evitar problemas de conversión entre formatos, evitar datos inconsistentes y poder cambiar
  de reloj o de plataforma en el futuro sin rehacer el sistema.

## Decisión

1. **Patrón adaptador en la ingesta.** Cada fuente de datos tiene su propio adaptador en `src/`: una
   función pura que recibe la ruta de la exportación y devuelve DataFrames con el **esquema canónico**
   del proyecto (skill `datos-actividades`: unidades SI, `timestamp` en UTC, columnas ausentes omitidas).
2. **Solo el adaptador conoce el formato de origen.** Conversiones de unidades, fechas y nombres de
   campos se hacen una única vez dentro del adaptador. El modelo de datos, las métricas y el dashboard
   trabajan solo con el esquema canónico.
3. **Primer adaptador: exportación de Apple Health.** `export.xml` se lee en streaming con la librería
   estándar (`xml.etree.ElementTree.iterparse`), porque el fichero ocupa cerca de 1 GB; las rutas de
   `workout-routes/*.gpx` se leen con `gpxpy`. Sin conversores externos.
4. **Fuente única: solo los entrenos registrados por el Apple Watch.** El adaptador se queda con los
   `<Workout>` de tipo `HKWorkoutActivityTypeRunning` cuyo origen es el reloj y descarta los escritos por
   otras aplicaciones (adidas Running, Nike Run Club, Strava). Así todas las sesiones se miden con el
   mismo sensor y los mismos algoritmos, y no hace falta una regla de deduplicación.
5. Cambiar de fuente (por ejemplo, otro reloj que exporte FIT) supone escribir un adaptador nuevo con
   sus tests; el resto del flujo no cambia.

## Alternativas descartadas

- **Exportación de la cuenta de adidas Running (JSON)**: solo cubre 42 sesiones, sin ruta ni dinámica
  de carrera; formato no documentado y con espera para recibirla.
- **Combinar todos los orígenes de Salud con una regla de deduplicación**: más sesiones (unas 29 de
  adidas sin equivalente en el reloj, más las de Nike y Strava), pero mezcla sensores y algoritmos de
  cálculo distintos; las métricas dejarían de ser comparables entre sesiones y la regla de
  deduplicación sería difícil de justificar.
- **Usar solo las sesiones de adidas dentro de Salud**: FC más fina, pero menos sesiones y sin rutas
  ni potencia.
- **Convertir las exportaciones a FIT con herramientas externas** (Tredict, Runalyze): paso manual y
  dependencia ajena; la conversión puede alterar campos sin quedar trazada en el código.
- **Leer la exportación directamente desde el modelo y las métricas, sin capa de adaptación**: acopla
  todo el sistema a un formato concreto.

## Consecuencias

- Conjunto de partida: 71 carreras del reloj (03/2023–10/2026). Se pierden unas 29 sesiones de adidas y
  las de Nike y Strava: se acepta a cambio de la consistencia.
- La FC por muestra de los entrenos del reloj viene agregada en la exportación. Se puede calcular el
  TRIMP de Banister, que usa FC media y duración de la sesión, pero no métricas basadas en tiempo por
  zonas de FC (p. ej. TRIMP de Edwards). Hay que tenerlo en cuenta al elegir métricas.
- La exportación incluye datos de salud ajenos al TFG (registros clínicos en `export_cda.xml`,
  electrocardiogramas). Vive solo en `data/raw/apple_health/` y nunca entra en git.
- `gpxpy` se usa para las rutas. `fitparse` no hace falta para esta fuente: se revisa si se retira al
  cerrar la fase.
- El modelo entidad-relación se diseña sobre el esquema canónico.

## Pendiente

- Reproducir en `notebooks/` las cifras del inventario preliminar e identificar de forma robusta el
  origen «Apple Watch» (el `sourceName` incluye el nombre del dispositivo; usar también el atributo
  `device`).
- Comprobar con el tutor si 71 sesiones bastan para el módulo predictivo.
- Cómo se obtienen datos de varios corredores para la vista de entrenador. Se decidirá en una ADR aparte.
