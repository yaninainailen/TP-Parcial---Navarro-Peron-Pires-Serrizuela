# Resultado completo A6 — Caso flojo

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Entrada: configuración y caso congelados en `00_configuracion_y_expectativa_previa.md`.
- El nombre exacto del modelo, temperatura, seed y otros parámetros no fueron expuestos por la
  interfaz; no se infieren.

## Respuesta completa

# Corrección — Caso-Ej-2

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `prompts/system_prompt.md` y `prompts/user_prompt.md` existen, pero no hay herramienta o conector real; las dos corridas muestran formatos de salida distintos y no existe supervisión humana con niveles L0–L4. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` solo dice: “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” No documenta iteraciones concretas, cambios ni una falla real. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Están `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, pero solo hay 2 corridas y `corrida_1.md` no tiene fecha. Son dos huecos de reproducibilidad simultáneos. |
| Análisis económico | Ausente | 0/15 | `README.md` solo afirma: “El modelo no es caro así que se puede usar tranquilamente para esto”, sin costos numéricos, tokens, proyección ni justificación del modelo. |
| Gobierno y riesgo | Insuficiente | 5/15 | `system_prompt.md` indica “Si es un reclamo, avisame” y `corrida_2.md` menciona avisar a un humano antes de enviar algo, pero no se detallan sistemas/datos, permisos, tratamiento concreto de fallas ni quién firma. |

## Puntaje total: 28/100

## Justificación por dimensión

**Sistema completo y funcionando:** Los prompts existen, pero el contrato es incompleto: `system_prompt.md` solo indica clasificar y responder, sin formato de salida estructurado. No hay ninguna herramienta o conector real y las corridas no tienen el mismo formato: `corrida_1.md` usa “Categoría / Respuesta sugerida”, mientras `corrida_2.md` presenta texto libre. Tampoco hay supervisión L0–L4.

**Proceso documentado:** `DECISIONES.md` no contiene las dos iteraciones concretas exigidas ni un antes/después verificable. La única evidencia es “Probé el prompt varias veces” y “No hubo grandes problemas”, sin fallas textuales ni cambios documentados.

**Formato y reproducibilidad:** La estructura obligatoria está presente, pero hay solo `corrida_1.md` y `corrida_2.md`, en lugar de tres corridas. Además, `corrida_1.md` no registra fecha, mientras `corrida_2.md` sí indica 2026-08-28. La combinación de ambos huecos lleva a Insuficiente.

**Análisis económico:** No hay números propios. El README afirma únicamente que “El modelo no es caro”, pero no presenta tokens de entrada/salida, costo por corrida, proyección a escala ni criterio explícito para elegir el modelo más chico que hace bien la tarea.

**Gobierno y riesgo:** Hay una indicación básica de escalamiento de reclamos en `system_prompt.md` y `corrida_2.md` dice que “Habría que avisarle a un humano antes de mandar nada”. Sin embargo, no se especifican permisos, sistemas o datos afectados, qué procedimiento seguir ante distintos fallos ni quién tiene responsabilidad o firma final.

## Señales de alerta

No se detectan inconsistencias que activen la regla anti-trampa. Las deficiencias observadas son de cumplimiento y evidencia de la rúbrica, pero no hay una afirmación numérica verificablemente falsa ni una contradicción interna que haya obligado a bajar el nivel por la regla anti-trampa.

## Sugerencia concreta de mejora

Crear una tercera corrida y completar `DECISIONES.md` con dos iteraciones verificables (entrada/resultado antes, cambio realizado y resultado después), además de incorporar en el README un costo por corrida calculado a partir de tokens reales y una proyección semanal o anual.
