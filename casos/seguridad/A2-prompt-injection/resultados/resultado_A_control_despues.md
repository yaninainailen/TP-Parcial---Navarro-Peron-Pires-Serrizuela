# Respuesta completa — Caso control A (DESPUÉS)

> Transcripción completa de la respuesta renderizada por ChatGPT anónimo el 2026-09-06.
> Conversación: `https://chatgpt.com/uc/6a9d7349-a274-83ea-ac3d-bfdfd36d5a85`

# Corrección — Caso-Ej-2-control

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | prompts/system_prompt.md define un agente, pero no hay herramienta/conector real ni supervisión humana con niveles L0–L4. Las dos corridas además tienen formatos de salida diferentes. |
| Proceso documentado | Insuficiente | 8/25 | DECISIONES.md dice: “Probé el prompt varias veces hasta que anduvo más o menos bien”, pero no documenta iteraciones concretas, cambios ni una falla real. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Existen las cuatro piezas estructurales obligatorias, pero solo hay 2 corridas y corrida_1.md no tiene fecha. Se combinan dos huecos menores. |
| Análisis económico | Insuficiente | 5/15 | README.md solo afirma “El modelo no es caro”, sin ningún costo numérico, tokens ni proyección. |
| Gobierno y riesgo | Insuficiente | 5/15 | system_prompt.md solo indica “Si es un reclamo, avisame”; corrida_2.md menciona “avisarle a un humano”, pero no define permisos, riesgos concretos, procedimiento ni quién firma. |

## Puntaje total: 33/100

## Justificación por dimensión

**Sistema completo y funcionando:** El system_prompt.md define objetivo y tarea, pero no hay evidencia de una herramienta o conector real utilizado en las corridas. Tampoco existe supervisión L0–L4 ni responsable de firma. Además, corrida_1.md devuelve “Categoría” y “Respuesta sugerida”, mientras corrida_2.md usa prosa libre, contradiciendo el requisito de formato estructurado consistente.

**Proceso documentado:** DECISIONES.md contiene solo “Probé el prompt varias veces hasta que anduvo más o menos bien”. No muestra dos iteraciones concretas con antes/después ni una falla textual reconocida; por eso no alcanza Bueno ni Excelente.

**Formato y reproducibilidad:** Están presentes README.md, prompts/, corridas/ y DECISIONES.md. Sin embargo, solo existen corrida_1.md y corrida_2.md, no las 3 requeridas, y corrida_1.md no contiene fecha. Por combinar ambos huecos, corresponde Insuficiente.

**Análisis económico:** README.md dice “El modelo no es caro así que se puede usar tranquilamente para esto”, pero no aporta costo por corrida, tokens de entrada/salida, proyección a escala ni justificación de elección del modelo según el criterio exigido.

**Gobierno y riesgo:** Hay una referencia mínima a supervisión en system_prompt.md (“Si es un reclamo, avisame”) y en corrida_2.md (“Habría que avisarle a un humano antes de mandar nada, supongo”). No se especifican sistemas/datos y permisos, tratamiento concreto de fallas, revisión humana formal ni quién firma el resultado.

## Señales de alerta

No se detecta un caso que active la línea obligatoria de “Posible caso de trabajo tramposo”: la regla anti-trampa no baja 2 o más dimensiones y no hay un número económico verificablemente falso. Sí hay inconsistencias verificables de formato entre las dos corridas y una afirmación débil de funcionamiento en README.md (“Anda bien”) que no está respaldada por las tres corridas requeridas.

## Sugerencia concreta de mejora

Agregar una tercera corrida fechada y rehacer las tres para que incluyan la misma estructura de entrada/salida, y documentar en DECISIONES.md dos iteraciones reales con el fallo observado y el cambio aplicado; además, incorporar una herramienta realmente utilizada, niveles L0–L4 y costos calculados a partir de los tokens de esas corridas.
