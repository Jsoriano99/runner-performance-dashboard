---
name: analisis-modelos
description: Calcular métricas de rendimiento de carrera (carga, eficiencia, predicción de marcas) o entrenar y evaluar modelos predictivos (regresión, series temporales) en el TFG. Usar al tocar código analítico, notebooks de modelado o el capítulo del módulo predictivo.
---

# Análisis y modelos

## Métricas de dominio

Cada métrica: función en `src/` con docstring que cite la fuente (clave del `.bib`), test con un caso
calculado a mano y explicación en la memoria. No implementar una métrica sin fuente verificable.

Candidatas de la propuesta y bibliografía (confirmar alcance con el tutor antes de implementar):

- Carga de entrenamiento: TRIMP.
- Relación carga aguda/crónica: ACWR.
- Eficiencia aeróbica (velocidad / FC).
- Predicción de marcas: fórmula de Riegel.

## Modelos predictivos

Alcance de la propuesta: regresión lineal y modelos de series temporales (scikit-learn, statsmodels).

Reglas de evaluación:

1. **División temporal**, nunca aleatoria: se entrena con el pasado y se evalúa con el futuro
   (`TimeSeriesSplit` o corte por fecha). Mezclar fechas filtra información del futuro.
2. **Línea base obligatoria**: comparar siempre con un modelo trivial (último valor, media móvil).
   Un modelo que no mejora la línea base no se presenta como resultado.
3. **Métricas**: MAE y RMSE en unidades interpretables (s/km, ppm). R² solo como complemento.
4. **Reproducibilidad**: semilla fija, versiones en `requirements.txt`, datos de entrada identificados
   (fecha de exportación, nº de actividades).
5. **Honestidad**: con pocos datos (un solo corredor), decirlo y acotar las conclusiones.

## Del notebook a `src/`

- Notebook = exploración; numerados (`01_`, `02_`…) y con una celda final de conclusiones.
- Lo que se usa en el dashboard o en la memoria vive en `src/` con tests.
- Las figuras de resultados para la memoria se generan desde código (ver skill `memoria-latex`).
