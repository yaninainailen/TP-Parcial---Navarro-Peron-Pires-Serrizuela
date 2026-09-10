# Decisiones — TP Final

Este archivo consolida los cambios aplicados a `prompts/system_prompt.md` y `prompts/user_prompt.md` durante las 3 corridas documentadas en `corridas/`, con el motivo de cada uno. El detalle iteración por iteración, con las salidas del agente y los errores encontrados, está en cada archivo de corrida:

- [`corridas/corrida_01_ACME.md`](corridas/corrida_01_ACME.md) — 2026-08-31
- [`corridas/corrida_02_SYNNEX.md`](corridas/corrida_02_SYNNEX.md) — 2026-08-31
- [`corridas/corrida_03_BMINING.md`](corridas/corrida_03_BMINING.md) — 2026-08-31

Las corridas se ejecutaron en ese orden y cada una parte del `system_prompt.md` resultante de la anterior — es decir, las mejoras se acumulan.

## Cambios aplicados a `prompts/system_prompt.md`

| # | Cambio | Corrida que lo originó | Motivo |
|---|---|---|---|
| 1 | Sección `# Clasificación de la información` (Hechos / Supuestos / Recomendaciones) en el Formato de salida | Corrida 01, iteración 2 | La sección Restricciones ya pedía distinguir hechos/supuestos/recomendaciones, pero no había ningún campo de salida donde volcarlo — un supuesto (esquema de protección 1+1) terminó redactado como si fuera un hecho de diseño. |
| 2 | Restricción: no asumir un esquema de protección específico sin confirmación del cliente | Corrida 01, iteración 3 | Aun con la clasificación agregada, el agente seguía comprometiéndose con "protección 1+1" en la descripción de la arquitectura cuando el cliente solo pidió sobrevivir a un corte de fibra, sin especificar el mecanismo. |
| 3 | Ítem de checklist en Fase 2: requisitos de la aplicación (modo de replicación síncrono/asíncrono, latencia, ventanas de corte) | Corrida 02, iteración 2 | Ante un caso de replicación de bases de datos, el agente solo preguntaba por "latencia máxima tolerada" en forma genérica; faltaba la pregunta técnica específica (síncrono/asíncrono) que realmente determina si la distancia es viable. |
| 4 | Ampliación de la Fase 4 para cubrir "cotización formal **o** estimación presupuestaria preliminar" + campos `Tipo de entregable solicitado` y `Urgencia declarada` en el Resumen de la oportunidad | Corrida 02, iteración 3 | El cliente pidió una "primera propuesta presupuestaria en una semana" y el agente lo evaluó con el vocabulario y el criterio de una cotización formal, sin distinguir la urgencia ni el tipo de entregable esperado. |
| 5 | Instrucción de revisar inconsistencias internas antes de la Fase 1 + sección `# Inconsistencias detectadas` en el Formato de salida | Corrida 03, iteración 2 | El texto de la Solicitud 003 nombra los canales ópticos como "IT#1 y OT#1" en un punto y "IT#1 y OT#2" en otro; el agente elegía una versión en silencio sin avisar de la contradicción. |
| 6 | Excepción a la restricción de proveedor: no aplica cuando el dato de plataforma/equipamiento lo aporta el propio cliente | Corrida 03, iteración 3 | El cliente (BMINING) especificó su propia plataforma (C-4615) como requisito, no como algo a recomendar; la restricción original hacía que el agente evitara nombrarla con rodeos artificiales. |
| 7 | Restricción de no inventar precios de BoM; usar "Pendiente de cotización con Producto/Pricing" cuando no hay datos reales de costo | Corrida 03, iteración 3 | Ante el pedido explícito de un BoM con precios, el agente completó la tabla con precios inventados de apariencia plausible — el único caso de alucinación de datos detectado en las 3 corridas. |
| 8 | Instrucción en Fase 3 + subsección `## Interconexión con red(es) existente(s) del cliente` en el Formato de salida | Corrida 03, iteración 4 | El cliente pidió explícitamente alternativas para interconectar la nueva red con su infraestructura existente; sin un lugar propio en el formato, ese pedido terminaba mezclado como una viñeta suelta dentro de otra opción de solución. |
| 9 | Sección `# Supervisión y niveles de autonomía` (vocabulario L0–L4 del curso, mapeado a las 6 fases) | Revisión de cierre contra `trabajo-final.md` (2026-09-01) | La supervisión humana existía solo como prosa genérica en el README ("revisión humana obligatoria"), no como parte del contrato mismo ni con el vocabulario L0–L4 que exige el ítem 1 de la rúbrica del trabajo final. |

## Cambios estructurales al repositorio (fuera de los prompts)

Al revisar el repo completo contra `trabajo-final.md`, el 2026-09-01 se encontró que dos dimensiones enteras de la rúbrica (Análisis económico y Gobierno y riesgo, 30% combinado) no tenían ningún artefacto, y que el sistema no cumplía el requisito de "al menos una herramienta o conector real" — las 3 corridas se habían hecho copiando y pegando manualmente entre un chat y los archivos de `corridas/`, no vía una llamada real a la API. Se aplicaron estos cambios:

| Cambio | Motivo |
| --- | --- |
| Rename `DESICIONES.md` → `DECISIONES.md` | `trabajo-final.md` exige ese nombre exacto para la estructura obligatoria; el nombre original tenía un typo que un evaluador automático no habría podido resolver por su cuenta. |
| `scripts/run_agent.py` (+ `prompts/casos/*.md`, `corridas/raw/`) | Resuelve el requisito de herramienta/conector real: lee un caso desde un archivo real y escribe el output completo (con tokens y costo reales de la API) a otro archivo real, en vez de depender de copy-paste manual. |
| `ANALISIS_ECONOMICO.md` | Cubre el ítem 5 de la rúbrica (costo por corrida, proyección semanal/anual, elección de modelo justificada), inexistente hasta esta revisión. |
| `GOBIERNO.md` | Cubre el ítem 6 de la rúbrica (sistemas y permisos, qué puede salir mal y respuesta, checklist de revisión del SE, quién firma, niveles L0–L4 por fase), que antes vivía disperso e implícito en "Limitaciones conocidas". |

`ANALISIS_ECONOMICO.md` quedó inicialmente con los números de costo marcados como pendientes (no había `ANTHROPIC_API_KEY` en el entorno). El 2026-09-01 se corrió `scripts/run_agent.py` con una API key real contra los 3 casos, con `claude-sonnet-5` y `claude-haiku-4-5`, y se completó el análisis con datos reales de `corridas/raw/`.

### Iteración operativa al correr el runner por primera vez (2026-09-01)

Correr el sistema de verdad, y no solo describirlo, encontró tres problemas reales que no habían aparecido en las 3 corridas manuales (hechas por copy-paste, sin API):

1. **Bug de `max_tokens` demasiado bajo:** con `max_tokens=4096` (el valor inicial del script), Sonnet 5 —que piensa por defecto (`thinking: adaptive`)— gastó casi todo el presupuesto en razonamiento interno y devolvió casi nada de texto visible en 2 de los 3 casos (las corridas de SYNNEX y BMINING quedaron con apenas la cabecera). Se subió el default a `max_tokens=16000` y se agregó un flag `--max-tokens` para ajustarlo por caso.
2. **Error del SDK al pedir mucho `max_tokens` sin streaming:** al subir `max_tokens` más allá de cierto punto para el caso BMINING (el más largo), el SDK de Anthropic rechazó la llamada no-streaming con `ValueError: Streaming is required for operations that may take longer than 10 minutes`. Se cambió `client.messages.create(...)` por `client.messages.stream(...).get_final_message()`, que no tiene ese límite.
3. **Autenticación con key "identity-linked":** la key usada requería además el header `anthropic-workspace-id` (error `400 anthropic-workspace-id is required...`). Se agregó soporte a una variable de entorno opcional `ANTHROPIC_WORKSPACE_ID` que el script manda como `default_headers` si está presente.

Con esos tres fixes, las 6 corridas (3 casos × 2 modelos) completaron sin truncarse. Los archivos quedan en `corridas/raw/`.

### Hallazgo de la comparación de modelo (Haiku 4.5 vs. Sonnet 5)

Con las 6 corridas reales se pudo hacer la comparación de modelo que pedía `ANALISIS_ECONOMICO.md` (antes solo planeada, no ejecutada). Resultado no trivial: **Haiku 4.5 no sostiene el contrato de forma confiable.** Falla de dos maneras distintas: en BMINING se detiene después de la Fase 2 en vez de completar las 6 fases con supuestos declarados como pide el `system_prompt.md`; y en ACME y BMINING reemplaza la plantilla fija de "Formato de salida" por su propia organización (encabezados `# Fase 1`/`# Fase 2`, tablas propias) en vez de las secciones exactas del contrato. Lo interesante es que **en SYNNEX sí produce los 11 encabezados exactos**: no falla siempre, falla de forma intermitente, que para un consumidor automatizado es peor que fallar siempre porque ni siquiera permite escribir un parser de compatibilidad. Sonnet 5 produce los encabezados exactos y completa el flujo en los 3 casos. Por eso se elige `claude-sonnet-5` pese a costar exactamente 3× más por token que Haiku ($3/$15 vs. $1/$5) — el criterio "el más chico que hace bien la tarea" se aplicó de forma literal: Haiku es el más chico, pero no hace bien la tarea bajo este contrato. Detalle completo en `ANALISIS_ECONOMICO.md`.

De paso, estas corridas reales validaron algo agregado en esta misma revisión: Sonnet 5 usó espontáneamente el vocabulario de la sección `# Supervisión y niveles de autonomía` agregada al `system_prompt.md` ("Nota de gobernanza: esta fase requiere revisión y aprobación explícita del SE...") **en las 3 corridas**, y siempre en las fases correctas — las marcadas L3 en el contrato. Aparece en ACME (línea 162), en SYNNEX (líneas 94, 115 y 142, incluso etiquetando explícitamente "L3" y "L2") y cuatro veces en BMINING. Incluso Haiku la aplicó en SYNNEX. Nunca se le pidió esa frase literal: confirma que la sección quedó integrada al contrato y no es un apéndice que el modelo ignora.

### Hallazgo en contra: una desviación de formato en Sonnet 5

La misma revisión de los archivos crudos encontró que Sonnet 5 tampoco cumple el contrato perfectamente. El Formato de salida dice *"Omitir esta sección si no se detectó ninguna"* sobre `# Inconsistencias detectadas`; en ACME —donde no hay ninguna— el modelo igual emitió la sección con el texto "Ninguna detectada en la información proporcionada hasta el momento". En SYNNEX sí la omitió correctamente.

Se anota acá porque contradice parcialmente la conclusión de la sección anterior y no tendría sentido esconderlo: el argumento a favor de Sonnet 5 no es que sea perfecto, es que es **confiable en lo que el sistema necesita** (encabezados exactos y flujo completo, 3 de 3) mientras Haiku es intermitente. Además refuerza el patrón de fondo de todo el trabajo: **una instrucción condicional —"omitir si no aplica"— se cumple peor que una instrucción sobre una sección que siempre existe.** Quedó sin corregir a propósito en el momento de escribir esto: verificar el arreglo exige una corrida nueva, y a esta altura del trabajo el hallazgo documentado vale más que el arreglo sin verificar.

*(Este hallazgo se corrigió en la Pieza 6 del contrato y se verificó con una corrida nueva el 2026-09-05 — ver la cuarta revisión más abajo.)*

## Segunda revisión contra `trabajo-final.md` (2026-09-05)

Una relectura del repo completo contra la consigna, ya con las 6 corridas reales disponibles como evidencia, encontró **errores de dato y afirmaciones que la propia evidencia no sostenía**. Es la revisión menos vistosa y probablemente la más importante: no cambió el agente, cambió lo que el repositorio afirma sobre el agente.

| Cambio | Motivo |
| --- | --- |
| **Precio de `claude-sonnet-5` corregido de $2/$10 a $3/$15** por millón de tokens, en `ANALISIS_ECONOMICO.md` y en `scripts/run_agent.py` | El $2/$10 era una **tarifa introductoria vigente hasta el 2026-08-31**, y las corridas son del **2026-09-01**: correspondía la tarifa estándar. Arrastraba todos los costos, el promedio y la proyección. El costo por corrida pasó de 0.1170 a **0.1754** y el anual de 56.16 a **84.20** |
| Afirmación "Haiku es ~2.5-3× más barato" corregida a "3× exacto" | Con la tabla vieja ($2/$10 vs $1/$5) el ratio era exactamente 2×, así que el texto se contradecía con su propia tabla. Con el precio corregido, 3× es literal |
| Afirmación "Sonnet 5 respeta el formato letra por letra" matizada + desviación de ACME documentada | Es falsa tal como estaba escrita: ver la sección anterior. Se prefirió documentar la falla antes que suavizar la redacción |
| Caracterización de Haiku corregida: falla en 2 de 3, no "desde ACME en adelante" | Haiku **sí** cumple la plantilla completa en SYNNEX. El dato real (intermitencia) es además mejor argumento que el que estaba escrito |
| Hallazgo L0–L4 corregido: aparece en las 3 corridas, no solo en BMINING | La evidencia era más fuerte de lo que el texto declaraba |
| `prompts/user_prompt.md` reescrito como contrato de entrada real | El ítem 1 de la consigna pide "system prompt + user prompt". El archivo contenía solo datos de prueba, sin ninguna instrucción: no era un user prompt. Ahora define qué entrega el SE, en qué forma y con qué reglas (transcribir sin interpretar, no completar huecos, no corregir contradicciones del cliente), y conserva las 3 solicitudes como los casos de prueba. **El texto de las 3 solicitudes no se tocó** — sigue siendo idéntico al de `prompts/casos/`, inconsistencia de la 003 incluida |
| `corridas/README.md` nuevo | En la carpeta convivían el registro de desarrollo (manual, fragmentos, 2026-08-31) y la evidencia de ejecución (API, completa, 2026-09-01) sin nada que las distinguiera. Un tercero no podía saber cuáles eran "las tres corridas". Se agregó además un enlace desde cada `corrida_0X.md` a su archivo de `raw/` |
| Sección "Confidencialidad" nueva en `GOBIERNO.md` | Hueco real del ítem 6: el archivo detallaba permisos de archivos locales pero no decía que **cada corrida manda información del cliente a una API de terceros**, que es la exposición que de verdad importa en un contexto de ventas B2B. Incluye qué se puede enviar, qué no sin autorización, anonimización bajo NDA y manejo de la API key |
| Trazabilidad en la cabecera de `run_agent.py`: sha256 del system prompt, `max_tokens` y `stop_reason` | Como el contrato fue evolucionando, un archivo de `raw/` no permitía saber **contra qué versión del contrato** había corrido. Aplica a corridas futuras; los 6 archivos existentes quedan anclados por fecha |
| Nombre de la materia y profesor corregidos en `README.md` + mapa rúbrica→artefacto | El README declaraba otra materia ("Creación de Agentes de IA — MADE N-2T") que la de la consigna. El mapa existe porque **corrige un agente**: le ahorra tener que inferir qué archivo cubre qué dimensión |

**Las cabeceras de `corridas/raw/` no se editaron.** Traen el costo calculado con la tarifa introductoria que el script tenía hardcodeada ese día, y por lo tanto ya no coinciden con `ANALISIS_ECONOMICO.md`. Reescribirlas para que cerraran habría sido más prolijo y habría falseado el registro: son la evidencia guardada tal como salió. La discrepancia está explicada en `ANALISIS_ECONOMICO.md`. Los tokens —que es el dato que el script observa de la API y del que se derivan todos los costos— coinciden exactamente.

## Tercera revisión: las seis piezas del system prompt (2026-09-05)

La consigna pide que el contrato tenga **las seis piezas de un system prompt profesional**. Al verificar el `system_prompt.md` premisa por premisa aparecieron cuatro huecos, uno de ellos grande:

| Pieza | Estado previo | Qué se hizo |
| --- | --- | --- |
| 1 · Rol | Decía "Senior" y "consultor experimentado", sin anclaje concreto | Se agregó "+15 años diseñando y dimensionando redes terrestres de transporte óptico", como pide la premisa |
| 2 · Contexto | Tenía el público y los datos de entrada, **pero no la empresa** | Se agregó una subsección "La empresa": fabricante de equipamiento óptico, con Producto/Pricing como dueño del precio e Ingeniería como validador del diseño |
| 3 · Tarea | Entraba directo al flujo de 6 fases | Se antepuso la frase inequívoca del entregable ("un único documento de análisis técnico-comercial que convierte información cruda del cliente en el material para su próxima conversación") |
| 4 · Restricciones | Cubría tono, fuentes y qué queda afuera; **faltaba extensión** | Se agregó una tabla de techos por sección, y se reorganizó en tres bloques: veracidad, producto/proveedores, forma |
| 5 · Formato | Completa | Solo se ajustó `## Opción 1` → `## Opción 1 — (título breve)`, para que la plantilla coincida con lo que muestran los ejemplos |
| 6 · Ejemplos | **No existía** | Se agregaron 3 pares entrada→salida completos |

### Por qué faltaba "la empresa", y qué destrabó definirla

El prompt nunca decía a quién representa el agente. Eso explica un comportamiento que ya estaba documentado como error en la Corrida 03: la restricción decía *"evita las recomendaciones específicas de un proveedor"*, pero el agente **nunca supo si él mismo era un proveedor**, y terminaba nombrando la plataforma del cliente con rodeos. La excepción que se agregó entonces trataba el síntoma; definir la empresa trata la causa.

**Cambio de alcance:** al establecer que la empresa es un fabricante, la restricción de proveedor se invirtió. Proponer la plataforma propia pasó de ser algo a evitar a ser el trabajo del agente. Lo que quedó prohibido es otra cosa, más precisa: comprometer códigos de parte, versiones o fechas sin Producto; descalificar equipamiento de terceros; y presentar como disponible una capacidad no calificada. Es el segundo caso en este trabajo en que una restricción "genérica y segura" resultó estar mal apuntada.

### Los 3 ejemplos de la Pieza 6

Se construyeron **sobre la estructura de las 3 solicitudes reales pero con clientes distintos** (AURELIA Retail, NORDEX Salud, QUILPO Minera), para que el prompt no le enseñe al modelo los mismos casos contra los que después se lo evalúa. Cada uno enseña una decisión distinta:

1. **AURELIA** — no comprometerse con un mecanismo de protección que el cliente no pidió, y **omitir** la sección de inconsistencias cuando no hay ninguna.
2. **NORDEX** — distinguir estimación preliminar de cotización formal, y preguntar por el modo de replicación síncrono/asíncrono en vez de por "latencia" en abstracto.
3. **QUILPO** — declarar una inconsistencia interna (lleva una deliberada: "CH#1 y CH#2" vs. "CH#1 y CH#3"), citar con naturalidad la plataforma que nombró el cliente, y estructurar un BoM sin inventar precios.

El primero es el más importante: **es el arreglo previsto para la desviación de formato de ACME** documentada más arriba. La instrucción condicional "omitir esta sección si no aplica" se venía cumpliendo a medias; el Ejemplo 1 no la explica, la *muestra*. Es la misma conclusión de "Qué aprendí" llevada un paso más: primero se descubrió que una sección fija funciona mejor que una instrucción en prosa, y ahora que un ejemplo funciona mejor que una instrucción condicional. **Se verificó con una corrida nueva el 2026-09-05** — ver la cuarta revisión más abajo: funcionó.

### Costo: efecto medido, no el que se esperaba

La Pieza 6 llevó el `system_prompt.md` de unos 4.300 a unos 19.800 tokens de input reales. Se especuló en su momento que esto iba a triplicar el costo por corrida. **Se remidió y no fue así:** el costo de Sonnet 5 bajó un 17% (de 0.1754 a 0.1462), porque los techos de extensión de la Pieza 4 redujeron el output a menos de la mitad, y el output es el componente que domina el costo. El detalle completo, con la comparación antes/después medida en las mismas 3 solicitudes, está en `ANALISIS_ECONOMICO.md`.

## Cambios aplicados a `prompts/user_prompt.md`

En la etapa de corridas se aplicaron únicamente correcciones editoriales menores (tildes, un typo de "intercoenctar", una mayúscula fuera de lugar en "Una semana"). En la revisión del 2026-09-05 el archivo se reestructuró como contrato de entrada (ver la tabla de arriba), sin tocar el texto de las 3 solicitudes. **No** se corrigió la inconsistencia "IT#1/OT#1" vs. "IT#1/OT#2" de la Solicitud 003 — ver "Qué se descartó" abajo.

## Por qué los casos de prueba son construidos y no reales

La consigna pide corridas reales "con entradas reales". Los 3 casos de este repositorio son **construidos sobre patrones reales de pedido**: los clientes no existen y ninguna solicitud corresponde a un pedido concreto recibido, pero el dominio es el trabajo cotidiano del autor y las ambigüedades plantadas son las que efectivamente aparecen en el puesto.

Se decidió así por gobierno, no por comodidad. **Cada corrida manda el contenido del caso a la API de un tercero.** Usar solicitudes reales habría significado sacar información de clientes del perímetro de la empresa para hacer un trabajo práctico de posgrado — exactamente el riesgo que `GOBIERNO.md` documenta y para el que fija la regla de anonimizar. Acá esa regla se aplicó en su forma más conservadora: construir los casos desde cero en vez de anonimizar los reales.

**El límite de este enfoque, dicho de frente:** casos escritos por la misma persona que después evalúa al agente pueden ser benévolos, porque el autor sabe qué maneja bien el agente. La evidencia sugiere que no ocurrió: **8 de los 9 cambios al `system_prompt.md` salieron de fallas que estos casos provocaron**, incluida la única alucinación de datos del ejercicio. Los casos rompieron al agente repetidamente antes de que funcionara. Aun así, un caso escrito por un tercero sería mejor prueba, y este trabajo no lo tiene.

## Cambios de alcance aplicados

- **Corrida 02:** se amplió el alcance de la Fase 4, que originalmente solo contemplaba preparar una cotización formal, para cubrir también estimaciones presupuestarias preliminares (entregable más liviano que los clientes piden con frecuencia bajo plazos cortos).
- **Corrida 03:** se amplió (con una excepción explícita) el alcance de la restricción "evita recomendaciones específicas de un proveedor", que pasó de ser una regla sin excepciones a una regla que no aplica cuando el dato de plataforma lo aporta el propio cliente.

## Qué se descartó

- **Corregir la inconsistencia OT#1/OT#2 en `user_prompt.md`:** se consideró, pero se descartó a propósito — corregirla habría eliminado el único caso de prueba real de detección de inconsistencias entre las 3 solicitudes, que es justamente lo que motivó el cambio #5 de la tabla de arriba.
- **Una "Fase 0" explícita de chequeo de escalación al inicio del flujo:** descartada por redundante frente a la sección "Criterios de escalación" ya existente (ver detalle en `corridas/corrida_03_BMINING.md`).
- **Generación de diagramas de red en ASCII por parte del agente:** descartada por riesgo de imprecisión técnica en un diagrama generado por un LLM sin herramienta de diseño real.
- **Cálculo automático de presupuesto de potencia óptica (link budget):** descartado por riesgo de error de ingeniería sin herramientas de cálculo reales; se prefiere dejarlo como supuesto de diseño a validar (cubierto ya por la Fase 5, sin necesidad de un campo nuevo).

## Limitaciones conocidas del agente

Estas limitaciones no se intentaron resolver vía prompt porque son inherentes al hecho de ser un agente basado en un LLM sin herramientas ni datos externos, y quedan anotadas también en `README.md`:

1. **No procesa diagramas ni archivos adjuntos reales.** El `Contexto` del `system_prompt.md` menciona "diagramas de red" como posible input, pero el agente solo puede razonar sobre texto: si el usuario tiene un diagrama, tiene que transcribir la información relevante a texto.
2. **No tiene acceso a datos reales** de disponibilidad de fibra oscura, catálogo de productos, precios vigentes ni inventario de red existente del cliente. Todo BoM o alternativa de interconexión que produce es estructural (ítems, cantidades, arquitectura), no verificado contra sistemas reales.
3. **No hace cálculos de ingeniería reales** (presupuesto de potencia óptica, dispersión cromática, etc.) — los deja explícitamente marcados como supuestos a validar por ingeniería en la nota de ingeniería (Fase 5), y así se descartó automatizarlos (ver arriba).
4. **No es determinístico.** A diferencia de un agente evaluador con rúbrica de puntajes fijos, dos corridas con el mismo input pueden variar levemente en redacción aunque sigan el mismo Formato de salida — no hay garantía de salida idéntica byte a byte.
5. **Solo detecta inconsistencias explícitas en el texto que recibe** (como el caso IT#1/OT#2); no puede validar la solicitud contra un CRM, un ERP o el historial real del cliente para detectar contradicciones con datos externos.
6. **Requiere revisión humana obligatoria** antes de enviar cualquier nota de ingeniería, propuesta o cotización al cliente — es un asistente para el Sales Engineer, no un reemplazo de su criterio.
7. ~~Los criterios de escalación nunca se ejercitaron.~~ **Cerrado en la quinta revisión** (ver más abajo): se agregó un cuarto caso de prueba diseñado para dispararla, y se corrió con los dos modelos. Las 3 solicitudes originales (001–003) siguen sin activarla — eso no cambió, y sigue siendo correcto que no la necesiten.

## Cuarta revisión: re-corrida con el contrato de las 6 piezas (2026-09-05)

La revisión anterior dejó dos cosas marcadas como "sin verificar": si el Ejemplo 1 realmente corregía la desviación de ACME en Sonnet, y qué pasaba con el costo. Se cerraron las dos con una corrida real de los 3 casos × 2 modelos, mismo contrato (sha256 `ad45bc6c055a` en las 6), usando la API key del autor.

### Lo que se confirmó

- **La desviación de ACME en Sonnet 5 se corrigió.** La nueva corrida de ACME no emite `# Inconsistencias detectadas` — antes la emitía con "Ninguna detectada", violando la instrucción de omitirla. El Ejemplo 1, que muestra la omisión en vez de explicarla, funcionó.
- **Los dos problemas estructurales de Haiku 4.5 documentados en `ANALISIS_ECONOMICO.md` también se corrigieron.** Antes producía el formato exacto en 1 de 3 casos y abandonaba el flujo en BMINING después de la Fase 2. Ahora produce el formato exacto en los 3 casos y completa las 6 fases en los 3 casos, BMINING incluido.
- **Ningún BoM de ningún modelo inventó un precio.** La restricción se sostiene bajo el contrato nuevo, con el contexto de fabricante agregado, en el caso históricamente más riesgoso (BMINING).
- **Los techos de extensión (Pieza 4) se respetaron casi sin excepción:** de 24 mediciones de secciones con techo numérico (4 secciones × 6 corridas), solo una se pasó por un ítem.

### Lo que apareció de nuevo — hallazgo no buscado

Agregar 3 ejemplos que terminan con un párrafo explicativo (`**Por qué esta salida es correcta:** ...`) tuvo un efecto secundario: **en 2 de 3 corridas, Haiku 4.5 imitó ese patrón y agregó su propia sección de comentario meta, no pedida por el Formato** (`## Notas sobre esta salida`, `## Notas de ejecución`). Sonnet 5 no lo hizo en ninguna corrida.

En la corrida de BMINING, esa sección menciona textualmente *"el cliente anterior (QUILPO)"* — **QUILPO es el cliente ficticio del Ejemplo 3 del propio contrato**, no un cliente de esta conversación. Es una fuga de contenido de los ejemplos hacia una salida real, y es más seria que una desviación de formato: un SE leyendo el documento completo se encuentra con el nombre de un cliente que no existe en el caso que está atendiendo.

Esto no estaba en el radar cuando se escribió la Pieza 6 — se buscaba corregir el formato y la completitud de fases, y se corrigió. El costo fue un modo de falla nuevo, específico de Haiku, que cambia la justificación de la elección de modelo (ver `ANALISIS_ECONOMICO.md`): ya no se descarta a Haiku por incompleto, se lo descarta porque agrega contenido no pedido y en un caso filtra un nombre ficticio.

**Recomendación anotada en su momento, aplicada en la quinta revisión** (ver más abajo): agregar a `# Restricciones` una línea explícita contra contenido fuera del Formato.

### Trazabilidad de las generaciones (actualizado — ver quinta revisión)

Las 6 corridas del 2026-09-01 (sin la Pieza 6) se conservan en `corridas/raw/`, igual que las 6 del 2026-09-05 con el contrato de las 6 piezas (sha256 `ad45bc6c055a`) — ninguna se editó ni se borró. Ambas quedaron superadas como "vigente" por la generación 3, documentada en la sección siguiente.

## Quinta revisión: restricción anti-fuga + caso de escalación (2026-09-05, continuación)

Con la comparación de modelos hecha (sección anterior) quedaron dos acciones pendientes explícitamente anotadas: cerrar la fuga de contenido encontrada en Haiku, y probar la rama de escalación del contrato. Se hicieron las dos, con corridas reales, en la misma sesión de trabajo.

### Cambios aplicados

1. **`prompts/system_prompt.md`:** se agregó a `# Restricciones` → "Sobre la forma": *"No agregues ningún contenido, comentario, sección o nota fuera de la estructura que define la sección '# Formato'."* Y en el cierre del propio Formato: *"Tu respuesta termina en 'Próximas acciones recomendadas': no agregues nada después."* El sha256 del contrato pasó de `ad45bc6c055a` a `76d91e443f52`.
2. **`prompts/casos/solicitud_004_andina.md`** (nuevo) + entrada en `prompts/user_prompt.md`: un cuarto caso, ANDINA LITIO, diseñado para disparar dos criterios de escalación a la vez (ruta transfronteriza Argentina–Chile con estatus regulatorio no verificado por el cliente, y tramos de más de 120 km sin energía para amplificación óptica). Marcado explícitamente como caso complementario, no como reemplazo de las 3 solicitudes exigidas por la consigna.

Se corrieron los 4 casos × 2 modelos (8 corridas, mismo sha256 en las 8, todas `stop_reason: end_turn`).

### Verificación 1 — la fuga se cerró

**0 de 8 corridas nuevas repiten el patrón de la revisión anterior.** Ni la sección extra (`## Notas sobre esta salida` / `## Notas de ejecución`), ni ninguna mención a AURELIA, NORDEX o QUILPO, en ninguna de las 8 salidas — incluidas las mismas SYNNEX y BMINING con Haiku donde había aparecido antes. La restricción de una línea resolvió el problema para los 3 escenarios conocidos.

### Verificación 2 — la escalación funciona, y aparece un hallazgo más preciso

Los dos modelos identificaron correctamente los dos factores de escalación del caso ANDINA y completaron el análisis de requisitos y brechas sin avanzar con arquitectura, cotización o nota de ingeniería — el contenido sustantivo de la escalación funcionó en ambos.

Pero en **formato**, la diferencia entre modelos reapareció, de una forma que corrige el diagnóstico de la revisión anterior:

- **Sonnet 5** manejó la escalación sin ninguna desviación: la declaró dentro de "# Resumen de la oportunidad" (una sección que ya existe en el Formato), omitió correctamente "Inconsistencias detectadas" (no había ninguna) y las secciones de Fases 3–5, y terminó limpio en "Próximas acciones recomendadas".
- **Haiku 4.5**, en el mismo caso, **inventó dos encabezados que no existen en el Formato** (`# ESCALACIÓN REQUERIDA`, `# Recomendaciones inmediatas`), **volvió a emitir "Inconsistencias detectadas" sin que hubiera ninguna** —el mismo error que el Ejemplo 1 ya había corregido para los 3 casos conocidos— y agregó una "Nota de gobernanza" después del cierre del Formato.

La conclusión de la revisión anterior decía "Haiku imita el comentario que cierra los ejemplos". Con este dato, el diagnóstico correcto es más específico: **el cumplimiento de formato de Haiku depende de que el caso se parezca a uno de los 3 ejemplos del contrato; ante un tipo de escenario nuevo, vuelve a inventar estructura.** Sonnet generaliza el Formato como regla; Haiku, más como patrón a copiar. Esto no cambia la elección de modelo (seguía siendo Sonnet) pero sí la razón, otra vez, con más precisión — ver `ANALISIS_ECONOMICO.md`.

### Hallazgo adicional: el costo también varía, no solo la redacción

BMINING con Sonnet 5 costó 0.1537 en la generación anterior y 0.2117 en esta (+38%), con una diferencia de contrato de una sola línea. Se revisó el contenido: el formato y las restricciones se mantuvieron intactos en ambas corridas: lo que cambió fue que esta corrida exploró con más profundidad una posible segmentación funcional entre los canales IT/OT del caso (algo que la Solicitud 003 sugiere pero no confirma), con una nota de ingeniería más larga. La limitación #4 del agente ("no es determinístico") se amplía con este dato: la variación entre corridas no es solo de redacción, también puede ser de costo — y una sola muestra no alcanza para saber si 38% es el caso típico o un extremo.

### Candidato para una futura iteración

Agregar un cuarto ejemplo al contrato que cubra específicamente un caso de escalación, siguiendo el mismo patrón que ya funcionó dos veces (Ejemplo 1 para la omisión condicional, y ahora la restricción explícita para el contenido fuera de plantilla): un ejemplo generaliza mejor que una instrucción en prosa. No se aplicó en esta entrega — ya se hicieron dos rondas de cambio-y-verificación en la misma sesión, y agregar una tercera sin más tiempo de por medio para revisar con calma no parecía la decisión correcta.
