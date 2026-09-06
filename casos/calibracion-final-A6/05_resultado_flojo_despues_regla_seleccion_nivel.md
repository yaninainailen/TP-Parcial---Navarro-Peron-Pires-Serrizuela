# Resultado completo A6 — Caso flojo después de la regla de selección de nivel

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Caso: el mismo `casos/flojo/Caso-Ej-2` utilizado en la línea base A6, sin cambios.
- Configuración: la misma rúbrica de A6 y el mismo `system_prompt.md`, con el único agregado
  documentado en esta iteración.
- El nombre exacto del modelo, temperatura, seed y otros parámetros no fueron expuestos por la
  interfaz; no se infieren.

## Respuesta completa

# Corrección — Caso-Ej-2

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `system_prompt.md` y `user_prompt.md` están presentes, pero no hay ninguna herramienta o conector real. Además, solo hay 2 corridas y sus salidas no mantienen un formato estructurado común. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` existe, pero solo dice: “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” No documenta iteraciones concretas ni una falla real. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Existen `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, pero hay solo 2 corridas en vez de 3 y `corrida_1.md` no tiene fecha. Se combinan dos huecos menores. |
| Análisis económico | Ausente | 0/15 | `README.md` solo afirma “El modelo no es caro así que se puede usar tranquilamente para esto”, sin costos, tokens, proyección ni justificación del modelo. |
| Gobierno y riesgo | Ausente | 0/15 | No existe una sección de gobierno/riesgo. `system_prompt.md` solo indica “Si es un reclamo, avisame”, sin permisos, riesgos, procedimiento concreto, revisión humana ni responsable que firme. |

## Puntaje total: 23/100

## Justificación por dimensión

**Sistema completo y funcionando:** `system_prompt.md` define un agente de atención y
`user_prompt.md` plantea una entrada de cliente, pero no hay herramienta o conector verificable.
Además, `corrida_1.md` devuelve “Categoría: Consulta de producto...” mientras `corrida_2.md` usa
una respuesta narrativa, por lo que no hay un formato estructurado consistente.

**Proceso documentado:** `DECISIONES.md` se limita a “Probé el prompt varias veces hasta que
anduvo más o menos bien”, sin especificar qué se probó, qué falló ni qué se modificó. Tampoco
reconoce una falla concreta del proceso.

**Formato y reproducibilidad:** La estructura básica está presente, pero `corridas/` contiene
únicamente `corrida_1.md` y `corrida_2.md`, no las 3 corridas exigidas. Además, `corrida_1.md`
carece de fecha, mientras `corrida_2.md` sí incluye “2026-08-28”.

**Análisis económico:** El `README.md` afirma que “El modelo no es caro”, pero no aporta ningún
número de costo propio. No hay tokens de entrada/salida, costo por corrida, proyección a escala
ni criterio explícito de elección del modelo.

**Gobierno y riesgo:** La única referencia relacionada está en `system_prompt.md`: “Si es un
reclamo, avisame”. No define qué sistemas o datos toca el agente, permisos, escenarios de riesgo,
acciones ante errores, controles humanos ni quién firma el resultado.

## Señales de alerta

No se detecta una condición de alerta anti-trampa de las definidas en la rúbrica. Sí se verifican
huecos de evidencia y reproducibilidad: el README afirma que el agente “anda bien”, pero el
repositorio solo contiene 2 corridas y no demuestra las capacidades exigidas por los niveles
superiores.

## Sugerencia concreta de mejora

Agregar una tercera corrida y rehacer las tres con el mismo formato estructurado, documentar en
`DECISIONES.md` al menos dos iteraciones con sus fallas y cambios concretos, e incorporar en el
repositorio un análisis de costos basado en tokens reales junto con una definición explícita de
supervisión L0–L4, permisos, riesgos y responsable de aprobación.
