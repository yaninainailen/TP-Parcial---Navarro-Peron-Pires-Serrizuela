# Respuesta completa — Caso control A

> Transcripción completa de la respuesta renderizada por ChatGPT anónimo el 2026-09-06.

# Corrección — Caso-Ej-2-control

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | Existen ambos prompts, pero no hay herramienta/conector real ni supervisión L0–L4 definida. |
| Proceso documentado | Insuficiente | 8/25 | DECISIONES.md solo dice: “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” |
| Formato y reproducibilidad | Insuficiente | 5/15 | Están todos los archivos/carpetas obligatorios, pero solo hay 2 corridas y corrida_1.md no tiene fecha; además las salidas no mantienen el mismo formato. |
| Análisis económico | Insuficiente | 5/15 | README.md afirma “El modelo no es caro”, pero no presenta ningún costo numérico propio. |
| Gobierno y riesgo | Insuficiente | 5/15 | system_prompt.md solo indica “Si es un reclamo, avisame”; no define sistemas/datos, permisos, tratamiento concreto de errores ni quién firma. |

## Puntaje total: 33/100

## Justificación por dimensión

**Sistema completo y funcionando:** Hay un objetivo básico en system_prompt.md (“atiende consultas de clientes de una tienda de ropa online”) y están presentes system_prompt.md y user_prompt.md. Sin embargo, no aparece ninguna herramienta o conector real usado en las corridas, ni supervisión humana con niveles L0–L4 y responsable de firma; por eso no alcanza Bueno y queda en Insuficiente.

**Proceso documentado:** DECISIONES.md existe, pero solo registra que se “Probó el prompt varias veces” y que “No hubo grandes problemas”. No hay dos iteraciones concretas, antes/después ni una falla real documentada, por lo que corresponde Insuficiente.

**Formato y reproducibilidad:** La estructura obligatoria está completa, pero hay solo dos corridas en lugar de tres y corrida_1.md no contiene fecha. Además, las salidas no tienen un formato estructurado consistente: la primera usa “Categoría / Respuesta sugerida” y la segunda es prosa libre. La combinación de huecos supera el nivel Bueno.

**Análisis económico:** README.md solo afirma “El modelo no es caro así que se puede usar tranquilamente para esto”. No hay costo por corrida calculado a partir de tokens reales, proyección a escala ni justificación de modelo, por lo que queda en Insuficiente.

**Gobierno y riesgo:** El único control explícito es “Si es un reclamo, avisame” en system_prompt.md; en corrida_2.md incluso aparece “Habría que avisarle a un humano antes de mandar nada, supongo.” No se especifican sistemas/datos y permisos, procedimiento ante fallos, revisión humana definida ni quién firma el resultado.

## Señales de alerta

⚠️ Posible caso de trabajo tramposo detectado

- README.md afirma que el agente “Anda bien” y que “contesta los mensajes que le paso”, pero corrida_2.md muestra una salida en formato diferente a corrida_1.md, contradiciendo la reproducibilidad del comportamiento.
- README.md afirma que “El modelo no es caro”, pero no existe ningún dato numérico que permita verificar esa afirmación.
- Las dos corridas disponibles no respaldan una demostración completa del sistema: falta la tercera corrida requerida y no se observa ninguna herramienta real en uso.

## Sugerencia concreta de mejora

Agregar una tercera corrida y rehacer las tres con entrada, fecha y salida estructurada idéntica, incorporando en el prompt una herramienta real y una política L0–L4; luego documentar en DECISIONES.md dos iteraciones con sus salidas problemáticas y calcular el costo por corrida usando los tokens efectivamente observados.
