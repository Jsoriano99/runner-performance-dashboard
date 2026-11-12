# ADR 0004 — Ingesta desacoplada mediante adaptadores; primera fuente: exportación de adidas Running

- Estado: aceptada
- Fecha: 12/11/2026

## Contexto

El autor solo tiene registros de entrenamiento en adidas Running (antes Runtastic); no tiene datos en
Strava ni en Garmin Connect, que eran las fuentes previstas al definir el stack (ADR 0001).

- adidas cerró su API pública el 30/06/2025, así que la única vía es la exportación manual de los
  datos de la cuenta.
- Esa exportación entrega ficheros JSON (resumen de cada sesión y series de GPS y, si se registró,
  frecuencia cardiaca), no FIT ni TCX.
- Se quiere evitar problemas de conversión entre formatos y poder cambiar de reloj o de plataforma
  en el futuro sin rehacer el sistema.

## Decisión

1. **Patrón adaptador en la ingesta.** Cada fuente de datos tiene su propio adaptador en `src/`: una
   función pura que recibe la ruta de la exportación y devuelve DataFrames con el **esquema canónico**
   del proyecto (definido en la skill `datos-actividades`: unidades SI, `timestamp` en UTC, columnas
   ausentes omitidas).
2. **Solo el adaptador conoce el formato de origen.** Las conversiones (épocas en milisegundos,
   unidades, nombres de campos) se hacen una única vez dentro del adaptador. El modelo de datos, las
   métricas y el dashboard trabajan solo con el esquema canónico.
3. **Primer adaptador: exportación JSON de adidas Running**, leída con pandas (`pd.read_json`,
   `pd.json_normalize`), sin librerías de terceros ni conversores externos.
4. Cambiar de fuente (por ejemplo, un reloj que exporte FIT) supone escribir un adaptador nuevo con
   sus tests; el resto del flujo no cambia.

## Alternativas descartadas

- **Convertir la exportación de adidas a FIT con una herramienta externa** (Tredict, Runalyze):
  añade un paso manual y una dependencia ajena al proyecto, y la conversión puede alterar o perder
  campos sin que quede trazado en el código.
- **Usar los GPX de la exportación**: la frecuencia cardiaca y la cadencia, si existen, van en
  extensiones XML con espacios de nombres propios de cada fabricante; más frágil que el JSON original.
- **API de adidas Running**: cerrada desde el 30/06/2025.
- **Leer el JSON de adidas directamente desde el modelo y las métricas, sin capa de adaptación**:
  más rápido al principio, pero acopla todo el sistema a un formato no documentado que puede cambiar.

## Consecuencias

- El modelo entidad-relación se diseña sobre el esquema canónico y puede cerrarse sin esperar a la
  exportación.
- El esquema del JSON de adidas no está documentado: hay que explorarlo en un notebook antes de
  escribir el adaptador, y el adaptador debe tener tests con ficheros sintéticos en `tests/fixtures/`.
- `fitparse` y `gpxpy` (ADR 0001) dejan de ser necesarios para la primera fuente. Se mantienen en
  `requirements.txt` hasta revisar la exportación real; si no hacen falta, se retiran al cerrar la fase.

## Pendiente de validar con la exportación real

- Qué campos trae de verdad (frecuencia cardiaca, cadencia, altitud, frecuencia de muestreo). Sin
  frecuencia cardiaca no se puede calcular el TRIMP clásico y habría que revisar las métricas.
- Cómo se obtienen datos de varios corredores para la vista de entrenador (la exportación es de un
  solo usuario). Se decidirá en una ADR aparte.
