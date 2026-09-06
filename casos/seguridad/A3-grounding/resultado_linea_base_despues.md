# A3 — Resultado completo DESPUÉS del cambio de grounding

## Condiciones de ejecución

- Fecha de ejecución: 2026-09-06.
- Rama: `mejoras-eber`.
- Único cambio aplicado: agregado del criterio de grounding en la Dimensión 1 de `rubrica.md`.
- Sin cambios en `agente/system_prompt.md` ni en los 12 archivos de `casos/excelente/Caso-Ej-1/`.
- Evaluador: ChatGPT anónimo en `chatgpt.com`.
- Longitud exacta del mensaje enviado: 34.629 caracteres.
- SHA-256 del mensaje enviado: `1b1095a7cad796cd8162fdb8b6ade1e18fc2b1dd154652c75c37ca9bfdbc2f96`.
- Conversación: <https://chatgpt.com/uc/6a9d7c9e-6784-83ea-ad65-d664ca07291c>.
- Limitación: la interfaz anónima no expuso modelo, temperatura, seed ni otros parámetros; no se infirieron.
- SHA-256 de la transcripción renderizada: `6e31edce5296b0505c9efe56b1ee091e329ea3f93a810c4e87f9fd724bb5ffdc`.

## Respuesta completa

La siguiente es la transcripción completa del contenido de la respuesta, excluyendo únicamente el aviso promocional de inicio de sesión de la interfaz:

# Corrección — Caso-Ej-1

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `prompts/system_prompt.md` y `user_prompt.md` están presentes y las 3 corridas mantienen el formato, pero no hay una herramienta/conector real verificable usado en las corridas. Además, `corrida_1.md` afirma acciones no respaldadas por ninguna herramienta, como “ya lo estamos consultando con logística”, y `corrida_3.md` “Iniciamos tu cambio de talle”. |
| Proceso documentado | Excelente | 25/25 | `DECISIONES.md` documenta dos iteraciones concretas: cambio de JSON a tabla markdown y agregado de la regla de derivación. También reconoce explícitamente la falla del ticket 104 en `corrida_2.md` y que no quedó completamente resuelta. |
| Formato y reproducibilidad | Excelente | 15/15 | Existen `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`; hay exactamente 3 corridas, cada una con fecha, entrada mediante CSV y salida estructurada completa. |
| Análisis económico | Excelente | 15/15 | `ANALISIS_ECONOMICO.md` presenta tokens de entrada/salida para las 3 corridas, calcula costo por corrida, proyecta a semana y año, y justifica el modelo según el criterio de “el modelo más chico que hace bien la tarea”. |
| Gobierno y riesgo | Excelente | 15/15 | `GOBIERNO.md` especifica acceso de solo lectura a los CSV, riesgos y mitigaciones, revisión humana de tickets sensibles y muestreo 1/5, además de niveles L0–L4 y firma del responsable de atención. |

## Puntaje total: 80/100

## Justificación por dimensión

**Sistema completo y funcionando:** Los prompts definen identidad, tarea, reglas y formato, y las tres corridas muestran una tabla consistente. Sin embargo, no se verifica ninguna herramienta o conector real utilizado: solo se muestran CSV como archivos de entrada. Además, hay afirmaciones de acciones ejecutadas sin respaldo, por ejemplo “ya lo estamos consultando con logística” (`corrida_1.md`) e “Iniciamos tu cambio de talle” (`corrida_3.md`); al repetirse este tipo de afirmación, la dimensión queda en Insuficiente por grounding.

**Proceso documentado:** `DECISIONES.md` contiene dos iteraciones con problemas y cambios concretos: el JSON que “salía mal formado y rompía” y el caso de reembolso que fue clasificado incorrectamente. También documenta honestamente la falla del ticket 104 y su resolución solo parcial, contrastada con la corrida 3.

**Formato y reproducibilidad:** La estructura obligatoria está completa y las tres corridas contienen fecha, entrada y salida. Cada `corrida_N.md` referencia además su CSV correspondiente, permitiendo reconstruir los tickets procesados sin depender de información ausente.

**Análisis económico:** `ANALISIS_ECONOMICO.md` aporta cifras específicas para cada corrida (620/210, 540/160 y 510/150 tokens), calcula un promedio y un costo aproximado de USD 0,00036 por corrida. También proyecta el costo para 50 consultas diarias y fundamenta explícitamente la elección del modelo mini con el criterio solicitado.

**Gobierno y riesgo:** `GOBIERNO.md` identifica los datos y sistemas accesibles, aclara que el agente no tiene acceso a mail, pagos ni base de clientes y establece mitigaciones concretas. Define revisión humana obligatoria, muestreo y una cadena L0–L4 que culmina en la firma del responsable de atención.

## Señales de alerta

- En `corrida_1.md`, el ticket 101 recibe una respuesta que afirma “ya lo estamos consultando con logística”, pero `GOBIERNO.md` establece que el agente solo tiene acceso de lectura a la planilla y no muestra ninguna herramienta o sistema de logística.
- En `corrida_1.md`, el ticket 102 dice “Te avisamos apenas se repone”, aunque el repositorio no evidencia capacidad de monitorear stock ni de enviar avisos.
- En `corrida_3.md`, el ticket 107 afirma “Iniciamos tu cambio de talle” y el 109 “Derivamos el caso para reponerte”, pese a que `GOBIERNO.md` describe al agente como limitado a procesar manualmente la planilla y sin acceso a sistemas operativos del negocio.
- En `corrida_3.md`, el ticket 108 informa que los envíos a Bariloche demoran “entre 5 y 8 días hábiles”, sin evidencia de esa política o dato en los archivos disponibles.

## Sugerencia concreta de mejora

Eliminar del `system_prompt.md` toda instrucción que permita presentar como ejecutadas acciones externas y, para cada dato operativo no presente en la entrada, exigir una respuesta explícitamente condicional o una derivación a humano; además, si se quiere conservar esas acciones, incorporar y demostrar en las corridas un conector real que las ejecute.
