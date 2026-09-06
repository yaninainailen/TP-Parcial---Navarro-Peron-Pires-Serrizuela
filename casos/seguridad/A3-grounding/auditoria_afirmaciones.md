# A3 — Auditoría de afirmaciones del caso excelente

## Alcance

- Caso auditado: `casos/excelente/Caso-Ej-1/`.
- Archivos revisados: los 12 archivos del caso, incluidos los tres CSV de entrada y las tres corridas completas.
- Criterio: una afirmación está respaldada cuando la entrada, una política incluida o una herramienta/conector verificable permite sostenerla. Que el agente la haya escrito en una corrida no constituye respaldo independiente.
- Capacidades verificables: `prompts/system_prompt.md` solo declara la recepción de un CSV; `GOBIERNO.md` limita el acceso a lectura de esa planilla pegada manualmente y declara que no hay acceso a correo, pagos ni base de clientes. No hay herramientas verificables de inventario, catálogo, logística, pedidos, notificaciones o derivación.

## Resultados

| Corrida / archivo | Fragmento mínimo | Qué afirma | Evidencia que permitiría respaldarlo | ¿Existe? | Clasificación |
|---|---|---|---|---|---|
| Corrida 1, `corridas/corrida_1.md`, ticket 101 | “Vimos que tu pedido sigue en depósito” | Que el agente comprobó el estado actual del pedido. | Consulta verificable al tracking o a logística. | Parcial: la entrada dice que el seguimiento muestra ese estado, pero no hay herramienta para que el agente lo compruebe; “Vimos” sugiere verificación propia. | **AMBIGUA** |
| Corrida 1, `corridas/corrida_1.md`, ticket 101 | “más tiempo del habitual” | Que existe y se superó un plazo habitual. | SLA o histórico de tiempos de depósito. | No. | **NO RESPALDADA** |
| Corrida 1, `corridas/corrida_1.md`, ticket 101 | “ya lo estamos consultando con logística” | Que se inició una consulta real a logística. | Conector, registro o acción verificable de derivación a logística. | No. | **NO RESPALDADA** |
| Corrida 1, `corridas/corrida_1.md`, ticket 101 | “te confirmamos el estado en las próximas 24hs” | Compromiso de respuesta dentro de 24 horas. | SLA, política o workflow verificable que garantice ese plazo. | No; además el prompt prohíbe prometer plazos no incluidos en la política disponible. | **NO RESPALDADA** |
| Corrida 1, `corridas/corrida_1.md`, ticket 102 | “está sin stock en este momento en la web” | Confirmación del stock actual. | Inventario o consulta verificable al sitio/catálogo. | Parcial: la entrada informa que al cliente le figura sin stock, pero pide confirmación; no hay acceso independiente a inventario. | **AMBIGUA** |
| Corrida 1, `corridas/corrida_1.md`, ticket 102 | “Te avisamos apenas se repone” | Compromiso de notificación futura. | Suscripción, CRM, correo o workflow verificable de alertas. | No. | **NO RESPALDADA** |
| Corrida 1, `corridas/corrida_1.md`, ticket 103 | “lo estamos derivando al equipo correspondiente” | Que se ejecutó una derivación real. | Conector o registro de asignación al equipo. | No; `revision_humana: sí` expresa una decisión en la tabla, no demuestra una derivación ejecutada. | **NO RESPALDADA** |
| Corrida 2, `corridas/corrida_2.md`, ticket 104 | “Estamos derivando tu pedido de cambio de talle a un asesor” | Que se ejecutó una asignación a un asesor. | Conector o registro de enrutamiento. | No. | **NO RESPALDADA** |
| Corrida 2, `corridas/corrida_2.md`, ticket 105 | “la campera de jean no viene forrada; la de gamulán sí tiene forro interior” | Características concretas de dos productos. | Catálogo, ficha de producto o herramienta de consulta. | No. | **NO RESPALDADA** |
| Corrida 2, `corridas/corrida_2.md`, ticket 106 | “Derivamos tu pedido de reembolso al equipo correspondiente” | Que la derivación ya ocurrió. | Conector o registro de asignación. | No. | **NO RESPALDADA** |
| Corrida 3, `corridas/corrida_3.md`, ticket 107 | “Iniciamos tu cambio de talle de la campera” | Que se abrió o ejecutó un trámite de cambio. | Herramienta de pedidos/cambios o registro de la operación. | No. | **NO RESPALDADA** |
| Corrida 3, `corridas/corrida_3.md`, ticket 107 | “te contactamos para coordinar el envío del cambio” | Compromiso de contacto y coordinación logística futura. | Acceso a un canal de contacto y workflow de logística. | No. | **NO RESPALDADA** |
| Corrida 3, `corridas/corrida_3.md`, ticket 108 | “los envíos a Bariloche demoran entre 5 y 8 días hábiles” | Plazo habitual específico de envío. | SLA, tabla de zonas o herramienta de cotización logística. | No. | **NO RESPALDADA** |
| Corrida 3, `corridas/corrida_3.md`, ticket 109 | “Derivamos el caso” | Que se ejecutó una derivación real. | Conector o registro de asignación. | No. | **NO RESPALDADA** |
| Corrida 3, `corridas/corrida_3.md`, ticket 109 | “para reponerte el par de medias faltante” | Que la resolución será reponer el producto. | Política de faltantes, inventario y autorización/operación de reposición. | No. | **NO RESPALDADA** |

## Síntesis

La auditoría encontró 13 afirmaciones **NO RESPALDADAS** y 2 **AMBIGUAS**. Los cuatro ejemplos indicados para A3 no resultaron respaldados de forma verificable: el plazo de 24 horas, las características de las camperas y el plazo a Bariloche son no respaldados; la frase de stock es ambigua porque repite lo que ve el cliente, pero se presenta como confirmación sin herramienta de inventario.
