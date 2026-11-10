---
name: diario-sesion
description: Cerrar una sesión de trabajo del TFG dejando constancia de lo hecho en el diario de desarrollo de la memoria, en la tarea de Notion y en git. Usar al terminar cada sesión o cuando el usuario diga "cerramos", "fin de sesión" o similar.
---

# Cierre de sesión

Objetivo: que cualquier persona (el tutor, el propio autor dentro de un mes, otro agente) pueda saber
qué se hizo, qué se decidió y qué queda, sin leer el historial de chat.

## Pasos

1. **Resumen de la sesión** (3–6 líneas): qué se ha hecho, decisiones tomadas y por qué, problemas
   encontrados, qué queda pendiente.
2. **Diario de desarrollo** — añadir una fila al anexo «Diario de desarrollo» de la memoria
   (`memoria-latex/`), la más reciente al final:

   ```latex
   10/11/2026 & Definición del proyecto & Stack, convenciones y skills; ADR 0001--0003. \\
   ```

   Fecha `dd/mm/aaaa` · tarea · resultado y decisiones (con referencia a ADR o commit si aplica).
3. **Memoria**: si lo hecho cambia algún capítulo, actualizarlo ahora (skill `memoria-latex`). Lo que
   no dé tiempo, marcarlo con `% TODO:` en su sitio.
4. **Notion**: en la base de datos «Tareas», actualizar la tarea de la sesión (proyecto TFG):
   `Estado` (En curso / Hecha) y `Notas` con el resumen corto y los siguientes pasos.
5. **Git**: commit de lo pendiente siguiendo `git-flujo` y push de la rama.
6. **Siguiente paso**: proponer al usuario la siguiente tarea concreta, sin empezarla.

## No hacer

- No inventar avances ni resultados: solo lo que se ha hecho y comprobado en la sesión.
- No reescribir entradas anteriores del diario; si algo cambió, se añade una entrada nueva.
