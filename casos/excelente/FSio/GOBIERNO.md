# Gobierno y riesgo — TP Final

Este archivo responde, para el agente asistente de SE descripto en [`prompts/system_prompt.md`](prompts/system_prompt.md), qué sistemas toca, qué puede salir mal, qué revisa un humano antes de confiar en la salida, y quién firma el resultado.

## Sistemas que toca y permisos

El agente **no tiene acceso a ningún sistema externo**. Es de solo lectura sobre lo que el Sales Engineer (SE) le entrega explícitamente en cada corrida:

| Sistema | Acceso del agente |
| --- | --- |
| Texto/archivo de la solicitud del cliente (notas de reunión, correos, requisitos) | Lectura, vía `scripts/run_agent.py` o pegado manual |
| `prompts/system_prompt.md` (su propio contrato) | Lectura |
| Sistema de archivos local (`corridas/raw/`) | Escritura únicamente del output que genera, vía `scripts/run_agent.py` |
| CRM / historial real del cliente | Sin acceso |
| Catálogo de producto / lista de precios vigente | Sin acceso — por eso todo BoM se marca "Pendiente de cotización con Producto/Pricing" (ver Restricciones en `system_prompt.md`) |
| Inventario de red / disponibilidad de fibra del cliente | Sin acceso |
| Email o cualquier canal de envío al cliente | Sin acceso — el agente nunca envía nada, solo redacta un borrador local |
| API de Anthropic (modelo de lenguaje) | **Salida de datos** — ver la sección siguiente |

El único conector real del sistema es `scripts/run_agent.py`: lee un archivo de solicitud de `prompts/casos/` y escribe el output completo en `corridas/raw/`. No hay ninguna vía por la que el agente escriba, modifique o envíe algo fuera de esa carpeta.

## Confidencialidad: qué datos salen de la empresa

Esta es la exposición real del sistema y no se resuelve con permisos de archivo. El agente **no corre localmente**: cada corrida envía la información del cliente —notas de reunión, requisitos técnicos, plazos, plataformas, nombre del cliente— a la API de Anthropic, un tercero fuera del perímetro de la empresa. Aunque el agente sea de solo lectura sobre lo que el SE le entrega, el acto de entregárselo ya es una transferencia de datos.

| Regla | Detalle |
| --- | --- |
| Qué se puede enviar | Requisitos técnicos, capacidades, distancias, topologías, plazos. Es el insumo mínimo para que el agente sirva |
| Qué **no** se envía sin autorización previa | Nombre real del cliente, contactos, precios negociados, condiciones comerciales, cualquier dato bajo NDA |
| Anonimización | Si el caso está bajo NDA, reemplazar el nombre del cliente por un alias antes de correr; el análisis técnico no pierde nada. Los 3 casos de este repositorio aplican esta regla en su forma más conservadora: en vez de anonimizar solicitudes reales, se construyeron casos nuevos sobre patrones reales de pedido, de modo que **ninguna información de cliente salió de la empresa para hacer este trabajo práctico** (ver `prompts/user_prompt.md`) |
| Antes de usarlo con un cliente real | Verificar los términos de uso de datos de la cuenta de API y la política de retención de la empresa. **Esa validación no está hecha en este trabajo** — es un requisito de despliegue, no de prototipo |
| Persistencia local | Los archivos de `corridas/raw/` quedan en disco con la información del cliente en texto plano. Si el caso es sensible, esa carpeta hereda la clasificación del caso |

**Credenciales:** `run_agent.py` lee `ANTHROPIC_API_KEY` (y opcionalmente `ANTHROPIC_WORKSPACE_ID`) de variables de entorno, nunca de un archivo del repositorio, y el `.gitignore` no necesita excluir secretos porque no hay ninguno versionado. La key es de la organización, no del agente: el agente no puede rotarla, leerla ni usarla para nada que no sea la llamada que el script construye. Rotar ante cualquier sospecha de exposición.

## Qué puede salir mal → qué pasa cuando sale mal

Estos son los modos de falla reales, encontrados en las 3 corridas documentadas en `corridas/` (no hipotéticos):

| Qué puede salir mal | Encontrado en | Qué pasa cuando sale mal |
| --- | --- | --- |
| Un supuesto se redacta como si fuera un hecho confirmado (ej. esquema de protección 1+1 no pedido por el cliente) | Corrida 01 | El output nunca es la nota final: pasa por la revisión del SE (ver checklist abajo) antes de convertirse en algo que se comparte. Si el SE no cruza "Clasificación de la información" contra el resto del texto, el supuesto puede colarse — por eso ese cruce está en el checklist, no es opcional |
| Pregunta de descubrimiento genérica en vez de la pregunta técnica específica que determina viabilidad | Corrida 02 | El SE completa la brecha en la siguiente interacción con el cliente; no bloquea el flujo pero sí puede demorar una cotización si no se detecta a tiempo |
| Inconsistencia interna del texto del cliente resuelta en silencio (el agente elige una versión sin avisar) | Corrida 03 | El agente ahora la declara explícitamente en "Inconsistencias detectadas" en lugar de resolverla; el SE la confirma con el cliente antes de avanzar |
| Precio de BoM inventado con apariencia plausible | Corrida 03 (único caso de alucinación de datos encontrado) | El agente ahora usa "Pendiente de cotización con Producto/Pricing" en vez de inventar cifras; cualquier precio real requiere pasar por Producto/Pricing, nunca sale del agente |
| Redacción no determinística entre corridas con el mismo input | Inherente a un LLM (ver `DECISIONES.md`) | No afecta el contenido sustantivo si el formato de salida se respeta; el SE revisa contenido, no compara redacción byte a byte |
| Desviación del formato fijo: emite una sección que el contrato manda omitir | Corrida real ACME con `claude-sonnet-5`, 2026-09-01 (`corridas/raw/`, sección "Inconsistencias detectadas" con "Ninguna detectada") | **Corregido y verificado con una corrida nueva el 2026-09-05:** se agregó un ejemplo al contrato que muestra la omisión en lugar de solo pedirla en prosa (Pieza 6, Ejemplo 1); la corrida nueva de ACME ya no emite la sección. Detalle en `DECISIONES.md` |
| El modelo agrega contenido fuera del Formato especificado, imitando el comentario que cierra los ejemplos del contrato, y en un caso menciona el nombre de un cliente ficticio de esos ejemplos | Corridas reales de `claude-haiku-4-5` en SYNNEX y BMINING, 2026-09-05 — no observado en `claude-sonnet-5`, el modelo elegido | **Corregido y verificado con 8 corridas nuevas el mismo día:** se agregó una restricción explícita contra contenido fuera del Formato; 0 de 8 corridas repiten el patrón. Detalle en `DECISIONES.md` |
| El cumplimiento de formato de un modelo alternativo no generaliza a tipos de caso que ningún ejemplo del contrato cubre — inventa encabezados nuevos y repite el error de la sección condicional | Corrida real de `claude-haiku-4-5` en un caso de escalación agregado específicamente para probar esa rama del contrato, 2026-09-05 — `claude-sonnet-5` manejó el mismo caso sin ninguna desviación | No ocurrió con el modelo elegido. Es la razón, más precisa que las anteriores, por la que este trabajo no elige Haiku 4.5 pese a costar una fracción de Sonnet 5 — ver `ANALISIS_ECONOMICO.md` |
| Información confidencial del cliente enviada a la API de un tercero | Inherente al diseño (ver "Confidencialidad" arriba) | No es recuperable una vez enviada: el control es **preventivo**, en el paso 0 del checklist. Por eso los 4 casos de prueba se construyeron en lugar de tomarse de solicitudes reales |

## Qué revisa el SE antes de confiar en la salida

**Paso 0 — antes de correr el agente, no después:** confirmar que la información que se le va a entregar puede salir del perímetro de la empresa (ver "Confidencialidad" arriba). Es el único control de esta lista que no se puede aplicar retroactivamente.

Checklist mínimo antes de que cualquier salida del agente se use en una conversación real con el cliente:

1. La sección "Clasificación de la información" es consistente con el resto del documento — ningún supuesto quedó redactado como hecho en otra sección.
2. Si el criterio de escalación aplica, está declarado explícitamente y las Fases 3–5 no avanzaron como ROM estándar (verificado con un caso real que dispara escalación — ver `DECISIONES.md`).
3. Ningún precio de BoM aparece inventado — todo lo que no viene de Producto/Pricing dice "Pendiente de cotización".
4. Las inconsistencias detectadas en el texto del cliente están listadas, no resueltas en silencio.
5. Las preguntas de aclaración son específicas al caso (no genéricas) y cubren lo que realmente falta para diseñar.
6. Los supuestos de diseño de la nota de ingeniería (Fase 5) son técnicamente razonables — el agente no hace cálculos de ingeniería reales (potencia óptica, dispersión), así que esto lo valida ingeniería, no el agente.

## Quién firma

El SE es el responsable final de cualquier nota de ingeniería, propuesta o cotización que llegue al cliente. El agente redacta un borrador; nunca firma, nunca envía, nunca se comunica directamente con el cliente. Cuando el entregable involucra precios reales de BoM, el paso adicional de Producto/Pricing es obligatorio antes de que el SE pueda firmar una cotización formal.

## Niveles de autonomía por fase (L0–L4)

| Nivel | Significado |
| --- | --- |
| L0 | El SE hace la tarea manualmente; el agente no participa |
| L1 | El agente redacta una propuesta; el SE la reescribe o la usa como punto de partida, sin obligación de revisarla campo por campo |
| L2 | El agente ejecuta la fase solo; el SE la revisa después de generada, antes de la siguiente acción externa |
| L3 | El agente ejecuta la fase solo; el SE debe revisarla y aprobarla explícitamente antes de que el resultado se use en cualquier paso siguiente |
| L4 | El agente ejecuta y actúa de forma autónoma, sin revisión humana previa |

Este sistema **nunca opera en L4** para ninguna fase — no tiene permisos para enviar nada al cliente por su cuenta. La asignación por fase:

| Fase | Nivel | Motivo |
| --- | --- | --- |
| Chequeo de inconsistencias | L2 | Bajo riesgo: solo declara, no resuelve — el SE la lee al pasar |
| Fase 1 — Análisis de requisitos | L2 | Resume información ya provista por el cliente; error de resumen es fácil de detectar al leer |
| Fase 2 — Análisis de brechas | L2 | Genera preguntas, no compromisos; el SE las cura antes de mandarlas al cliente |
| Fase 3 — Evaluación de la solución | L3 | Compromete arquitectura y riesgos técnicos — requiere aprobación explícita del SE antes de que la propuesta avance |
| Fase 4 — Preparación de cotización/estimación | L3 | Clasifica la oportunidad como lista/parcialmente lista/no lista — decisión comercial que el SE debe validar antes de comunicarla |
| Fase 5 — Nota de ingeniería | L3 | Incluye supuestos de diseño no verificados por herramientas reales; requiere aprobación de ingeniería/SE antes de circular |
| Fase 6 — Reunión de seguimiento | L2 | Es una preparación interna del SE, no algo que llega al cliente directamente |

*Nota de alcance: la escala L0–L4 de arriba está definida explícitamente en este archivo en lugar de asumirse, para que la asignación por fase sea evaluable con el criterio que se declara acá y no dependa de una interpretación implícita. El eje que la ordena es **cuánta autoridad tiene el agente sobre lo que sale hacia el cliente**, que es el riesgo que importa en este caso de uso. Que el sistema nunca opere en L4 no es una limitación técnica sino una decisión de diseño: el agente no tiene ningún canal de salida al cliente, así que la barrera es estructural y no depende de que el modelo se comporte bien.*
