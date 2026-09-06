# Calibración

## Calibración histórica — versión previa a A2–A6

Esta primera parte conserva la calibración original y sus resultados tal como fueron obtenidos.
Los puntajes 100/28/28 que aparecen aquí son **históricos**: describen versiones previas del
evaluador y no deben interpretarse como resultados vigentes. La sección
`Recalibración final A6`, al final del documento, registra la configuración y los resultados
actuales posteriores a A2–A6.

Corrimos el agente corrector (`agente/system_prompt.md` + `rubrica.md`, tal como estaban escritos
en esa versión, sin atajos) sobre los 3 casos de `casos/`. Antes de correrlo, el grupo definió a ojo qué nota
esperaba para cada uno, para que la comparación sea honesta y no una racionalización posterior.

## Nota esperada por el grupo (definida antes de correr el agente)

| Caso | Nota esperada (humano) | Motivo |
|---|---|---|
| Excelente | ~95-100 | Cumple los 6 requisitos con solidez y encima reconoce una falla real. |
| Flojo | ~25-30 | Incompleto en casi todo, pero no miente sobre lo que hizo. |
| Tramposo | Baja, y sobre todo **marcado como sospechoso** | Lo importante acá no es solo el número — es que el informe deje claro que no se puede confiar en lo que afirma. |

---

## Corrida 1 — Caso `excelente`

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Excelente | 30/30 | Contrato completo (`prompts/`), herramienta real (planilla CSV) usada de verdad en las 3 corridas, salida en la misma tabla en las 3, supervisión L0–L4 en `GOBIERNO.md`. |
| Proceso documentado | Excelente | 25/25 | `DECISIONES.md` narra 2 iteraciones concretas (JSON→tabla por comillas rotas; regla de derivación agregada tras una prueba fallida) y reconoce una falla real no resuelta del todo (ticket 104, `corrida_2.md`). |
| Formato y reproducibilidad | Excelente | 15/15 | `README.md`, `prompts/`, `corridas/` (3, cada una con fecha), `DECISIONES.md` presentes y completos. |
| Análisis económico | Excelente | 15/15 | `ANALISIS_ECONOMICO.md` calcula el costo con los tokens reales anotados en cada corrida, proyecta a semana/año, y justifica el modelo mini con el criterio del curso. |
| Gobierno y riesgo | Excelente | 15/15 | `GOBIERNO.md` define permisos, riesgos con su mitigación concreta, revisión muestral y niveles L0–L4 con quién firma. |

## Puntaje total: 100/100

## Señales de alerta
(vacío — no se encontró ninguna afirmación sin respaldo)

## Sugerencia concreta de mejora
Probar la regla de desempate del system prompt con más variantes de mensajes ambiguos (queja +
pedido de cambio en el mismo texto) antes de dar por resuelta la limitación anotada en
`DECISIONES.md`.

**Comparación con lo esperado:** el grupo esperaba ~95-100 y el agente dio 100. **Coincide.** No
hubo desacuerdo en este caso — se incluye igual porque confirma que la rúbrica también reconoce
un trabajo bien hecho sin regatearle puntos por reconocer una falla propia.

---

## Corrida 1 — Caso `flojo` (antes del ajuste de rúbrica)

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `user_prompt.md` pega el mensaje del cliente a mano ("el cliente me escribió esto: [texto]"), no hay planilla ni conector real; el formato de salida cambia entre `corrida_1.md` (Categoría + Respuesta sugerida) y `corrida_2.md` (texto libre sin esa estructura); no hay niveles de supervisión definidos en ningún lado. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` es una sola línea sin ningún detalle verificable de qué cambió o qué falló. |
| Formato y reproducibilidad | Bueno | 10/15 | Hay solo 2 corridas en vez de 3, y a `corrida_1.md` le falta la fecha — la rúbrica, en su redacción original, alcanzaba el nivel Bueno con cualquiera de los dos huecos por separado. |
| Análisis económico | Insuficiente | 5/15 | El README dice "el modelo no es caro" sin ningún número propio. |
| Gobierno y riesgo | Ausente | 0/15 | No hay ninguna sección de gobierno; la única mención ("habría que avisarle a un humano... supongo") es una duda suelta dentro de una corrida, no un desarrollo del tema. |

## Puntaje total: 33/100

## Desacuerdo encontrado
El grupo esperaba una nota más baja (~25-30) porque el caso `flojo` acumula **dos** huecos de
formato a la vez (2 corridas en vez de 3, y falta de fecha en la que hay), no uno solo — y la
rúbrica original trataba ambos huecos como si fueran alternativas independientes, dándole Bueno
(10/15) con cualquiera de los dos. El grupo consideró que combinar dos huecos menores a la vez es
un problema de reproducibilidad más serio que tener uno solo, y que la rúbrica no lo distinguía.

## Ajuste hecho
Se reescribió el nivel Insuficiente de la dimensión "Formato y reproducibilidad" en `rubrica.md`
para que combinar dos o más huecos menores de la lista de "Bueno" a la vez baje el nivel a
Insuficiente, en vez de quedarse en Bueno (ver commit correspondiente).

## Corrida 2 — Caso `flojo` (después del ajuste, mismo repo, sin cambios en `casos/flojo/`)

| Dimensión | Nivel asignado | Puntos |
|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 |
| Proceso documentado | Insuficiente | 8/25 |
| Formato y reproducibilidad | **Insuficiente** | **5/15** |
| Análisis económico | Insuficiente | 5/15 |
| Gobierno y riesgo | Ausente | 0/15 |

## Puntaje total: 28/100

**Comparación con lo esperado:** el grupo esperaba ~25-30 y el agente, después del ajuste, dio 28.
**Coincide.**

---

## Corrida 1 — Caso `tramposo` (antes del ajuste del system prompt)

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | El prompt afirma integración real vía Gmail/OAuth, pero ninguna corrida muestra headers de mail, tokens de API ni configuración de OAuth — son texto tipeado a mano, igual que en el caso flojo. Por la regla anti-trampa, una herramienta afirmada pero no demostrada no cuenta como real. `GOBIERNO.md` no define niveles L0–L4 propios del caso, es texto genérico de compliance. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` afirma "47 iteraciones a la perfección" sin mostrar ningún antes/después concreto. La apelación a la simpatía ("lo hice solo, sin dormir") no suma ni resta puntos, se ignora. Descontando lo no verificable, el contenido real es equivalente a un `DECISIONES.md` mínimo. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Ninguna de las 3 corridas tiene fecha, lo que hace imposible verificar si corresponden de verdad a "dos semanas de producción" como afirma el README. |
| Análisis económico | Ausente | 0/15 | Afirma que 500 tickets con "el modelo más avanzado disponible" costaron ~USD 0,000004 por ticket — 25 veces menos por ticket que el modelo *mini* usado en el caso excelente (~USD 0,0001/ticket). Un modelo más grande costando mucho menos por ticket es matemáticamente inconsistente con cualquier tabla de precios real. Por la regla anti-trampa, un número inventado que no cierra puntúa igual o peor que no tener análisis económico. |
| Gobierno y riesgo | Insuficiente | 5/15 | `GOBIERNO.md` menciona ISO 27001, GDPR y SOC 2 en abstracto, sin ningún desarrollo específico de qué permisos tiene este agente puntual ni qué pasa si falla. |

## Puntaje total: 28/100

## Señales de alerta (primera corrida)
- El README afirma una integración con Gmail/OAuth que ninguna corrida respalda.
- El análisis económico tiene números que no cierran contra el caso excelente.
- Ninguna corrida tiene fecha, pese a afirmar dos semanas de producción real.
- `DECISIONES.md` afirma 47 iteraciones sin mostrar ninguna en concreto.

## Desacuerdo encontrado
El puntaje total del caso `tramposo` (28/100) quedó **igual** al del caso `flojo` después de su
ajuste (28/100), pese a ser situaciones completamente distintas: uno es un trabajo honestamente
incompleto, el otro miente activamente sobre lo que hizo. El grupo señaló que un informe que hace
sonar a ambos casos igual de "flojos" **no cumple el objetivo central del parcial**, que es que el
agente **detecte** al tramposo — no que le ponga la misma nota que a alguien que simplemente no
llegó. La sección "Señales de alerta" de la primera corrida ya listaba las inconsistencias, pero
sin ningún elemento que la distinguiera visualmente ni la jerarquizara por sobre una lectura
rápida del puntaje total.

## Ajuste hecho
Se agregó a `agente/system_prompt.md` una regla obligatoria: si la regla anti-trampa de
`rubrica.md` bajó el nivel de 2 o más dimensiones por falta de evidencia o inconsistencia, la
sección "Señales de alerta" tiene que abrir con la línea `⚠️ Posible caso de trabajo tramposo
detectado`, así el informe distingue "está incompleto" de "miente sobre lo que hizo" sin depender
de que alguien note que el número total es bajo.

## Corrida 2 — Caso `tramposo` (después del ajuste, mismo repo, mismo puntaje)

El puntaje por dimensión y el total no cambian (28/100, porque el problema no era el número sino
el informe) — lo que cambia es el encabezado de la sección de alerta:

## Señales de alerta (segunda corrida)
**⚠️ Posible caso de trabajo tramposo detectado**
- El README afirma conexión real vía Gmail/OAuth y 500 tickets en producción; el repo no tiene
  ninguna corrida, config ni log que lo respalde — las 3 corridas son texto tipeado a mano, igual
  que en un caso de prueba, no tráfico real.
- El análisis económico afirma que un modelo "más avanzado" costó 25 veces menos por ticket que el
  modelo mini del caso excelente — inconsistente con cualquier tabla de precios real.
- Ninguna corrida tiene fecha, incompatible con la afirmación de "dos semanas de producción real".
- `DECISIONES.md` afirma 47 iteraciones "a la perfección" sin describir ninguna en concreto.

**Comparación con lo esperado:** el grupo no esperaba un número específico para este caso, esperaba
que quedara **marcado como sospechoso** de forma inequívoca. Con el ajuste, el informe de
`tramposo` y el de `flojo` ya no se leen igual aunque el total coincida: solo el de `tramposo`
abre con la alerta obligatoria. **Coincide con lo esperado.**

---

## Resultado histórico de esta etapa

| Caso | Puntaje del agente | Esperado por el grupo | ¿Coincide? | Cómo se detecta |
|---|---|---|---|---|
| Excelente | 100/100 | ~95-100 | Sí | Puntaje alto, sin alertas. |
| Flojo | 28/100 (tras ajuste) | ~25-30 | Sí | Puntaje bajo, sin alertas (no mintió sobre nada). |
| Tramposo | 28/100 | Puntaje bajo + marcado como sospechoso | Sí | Puntaje bajo **y** alerta obligatoria de trabajo tramposo — la distinción con `flojo` no está en el número, está en la alerta. |

Dos ajustes reales quedaron hechos sobre la rúbrica y el corrector a partir de esta calibración:
combinar huecos menores de formato baja de nivel (afecta a cualquier trabajo, no solo a este caso),
y la detección de trampa tiene que ser explícita en el informe, no inferida del puntaje total.

---

## Prueba adicional: corriendo el corrector contra repos reales (no construidos por el grupo)

Los 3 casos anteriores (`excelente`, `flojo`, `tramposo`) los construyó el propio grupo — prueban
que el corrector distingue niveles de calidad diseñados a propósito para eso, no que funcione
igual de bien sobre un trabajo real que nadie ajustó a la rúbrica. Para probar esto, se corrió el
corrector (mismo `system_prompt.md` + `rubrica.md`, sin ningún cambio) sobre tres repos reales de
la Entrega 1 de la materia (`agentes-ia-ucema-ej1`, `agentes-ia-ucema-ej2` y `Repo-Yanina-Navarro`),
que no son trabajos finales.

**Resultado esperado:** puntaje bajo en los tres, porque ninguno tiene la estructura que pide un
trabajo final (no fueron pensados para eso). **Confirmado:** el corrector los puntuó bajo, de
forma consistente, sin romperse ni devolver un formato distinto al esperado.

**Hallazgo 1 — ambigüedad real en la rúbrica.** En `agentes-ia-ucema-ej2`, el contenido de proceso
(versiones de prompt guardadas, decisiones de diseño) está en el `README.md`, no en un archivo
`DECISIONES.md`. Esto expuso que la rúbrica decía "`DECISIONES.md` (o equivalente)" sin definir qué
contaba como equivalente. **Ajuste hecho:** se agregó una aclaración en `rubrica.md` resolviendo
esto de forma estricta, citando que la consigna del trabajo final exige "sin excepciones de
formato" — no se ablandó el criterio pese a que el contenido de `agentes-ia-ucema-ej2` era, en
calidad, bueno.

**Hallazgo 2 — alerta de seguridad fuera de la rúbrica.** Al leer
`agentes-ia-ucema-ej2/dump_rcta.py` apareció un hostname interno y un usuario reales hardcodeados
en un archivo de un repositorio público. No es parte de ninguna dimensión de la rúbrica, pero se
registra acá porque es el tipo de cosa que un corrector automatizado debería poder señalar aunque
no le sume ni reste puntaje.

**Repo 3 — `Repo-Yanina-Navarro` (Entrega 1, generador de copies para @barescopados).**
Arquitectura distinta a los dos anteriores (HTML/CSS/JS + API de Gemini desde el navegador, sin
Python). Puntaje: 20/100, sin señales de alerta.

**Aclaración, no hallazgo nuevo:** los tres repos probados hasta acá (`ej1`, `ej2` y este) son de
la Entrega 1 de la materia, que nunca pidió `DECISIONES.md` ni la estructura del trabajo final —
por eso los tres puntúan bajo en Proceso documentado y Formato. Esto no es evidencia de una
ambigüedad real en la rúbrica (ya la resolvimos en el hallazgo 1); es evidencia de que el corrector
distingue correctamente "esto no es un trabajo final" de "esto es un trabajo final flojo" — que es
un resultado distinto y también necesario de confirmar. Lo que sí prueba, con un stack de código
totalmente distinto a los otros dos, es que el corrector no depende de un lenguaje o arquitectura
particular para aplicar la rúbrica.

**Por qué importa esta prueba:** los 3 casos oficiales prueban que el corrector distingue niveles
de calidad diseñados a propósito. Esta prueba adicional muestra que, sobre un repo real y no
preparado, el corrector no se rompe, produce el formato esperado, y su aplicación estricta de la
rúbrica saca a la luz ambigüedades reales antes de la prueba de fuego.

---

## Stress test: caso "medio tramposo" (una sola dimensión fabricada)

Los 3 casos oficiales y las pruebas contra repos reales confirman que el corrector distingue bien
trabajos honestos de trabajos que mienten en casi todo. Faltaba probar el caso intermedio: ¿qué
pasa si alguien miente en **una sola** dimensión, bien escondida, y el resto del trabajo es
genuino? Se armó un caso de prueba en papel (no un repo completo, un ejercicio de calibración): 4
de las 5 dimensiones excelentes y reales, y `ANALISIS_ECONOMICO.md` con un costo por corrida que no
cierra contra los tokens reales mostrados en las corridas del mismo caso (10 veces más barato de lo
que da la cuenta real).

**Resultado con la regla original:** 85/100, sin alerta — la regla exigía 2+ dimensiones afectadas
por la regla anti-trampa, y acá bajó una sola. Un puntaje alto con una mentira verificable adentro,
sin ninguna señal visible arriba del informe.

**Ajuste hecho:** se modificó la "Regla obligatoria" de `agente/system_prompt.md` para que la
alerta también se dispare si una sola dimensión queda penalizada específicamente por la regla
anti-trampa Nº4 (número que contradice matemáticamente otra evidencia del repo), sin importar
cuántas otras dimensiones estén afectadas. Con el ajuste, el mismo caso da el mismo puntaje
(85/100, el número no cambia) pero el informe abre con la alerta obligatoria.

**Por qué importa:** el umbral de "2 o más dimensiones" protegía bien contra el tramposo que miente
en todos lados (el caso oficial `tramposo`), pero no contra el que elige mentir en un solo lugar
difícil de verificar a ojo. Este es el caso más peligroso para la prueba de fuego en vivo, porque un
puntaje alto no genera la misma sospecha inmediata que uno bajo.

---

# Recalibración final A6

Esta sección reemplaza como referencia vigente —sin borrar el historial anterior— los resultados
de la calibración original. La recalibración se realizó después de A2–A5, fijó una expectativa
humana por dimensión antes de ejecutar y conservó tanto las corridas exitosas como las
iteraciones fallidas. La evidencia completa está en `casos/calibracion-final-A6/`.

## Cambios acumulados antes de A6

| Mejora | Hallazgo y alcance validado |
|---|---|
| A2 · Prompt injection | En el ataque directo probado, la versión inicial no obedeció la instrucción maliciosa pero tampoco la informó. El cambio hizo que el contenido del repo se trate como dato no confiable, que el intento se cite en `Señales de alerta` y que su sola existencia no cambie la nota. Esta prueba no demuestra inmunidad frente a toda variante posible. |
| A3 · Grounding | Se auditaron 13 afirmaciones no respaldadas y 2 ambiguas en el caso excelente. El nuevo criterio obliga a revisar hechos, capacidades, acciones y compromisos; las repeticiones materiales limitan D1 a Insuficiente. Por eso el 100/100 histórico dejó de ser vigente. |
| A4 · Acceso a archivos | Se distinguieron los estados ausente, presente y leído, y presente pero inaccesible. La inaccesibilidad técnica no debe convertirse por sí sola en `Ausente`, penalización o alerta anti-trampa. |
| A5 · Sincronización de Sonda | Se comprobó que Sonda usaba copias embebidas desactualizadas y se sincronizaron `systemPromptText` y `rubricaText` con sus fuentes. |

## Configuración congelada

- Rama: `mejoras-eber`.
- Commit base del evaluador: `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- `agente/system_prompt.md`: idéntico a ese HEAD; SHA-256 normalizado
  `f5b4d4166ad600db90c69414eadbd2c68405ac9b26fa29062cf3dd16e9228890`.
- `rubrica.md`: misma base más las aclaraciones finales D4 y D5; SHA-256 normalizado
  `9dfdb6e989e7515d562863e133010375c0b0321ecb97f6fa86d96a87d1bd5682`.
- `rubricaText` efectivo de Sonda: igualdad exacta con `rubrica.md`, 10.130 caracteres y el mismo
  SHA-256 normalizado. `systemPromptText` también coincide exactamente con su fuente.
- Casos originales: `casos/excelente/Caso-Ej-1/`, `casos/flojo/Caso-Ej-2/` y
  `casos/tramposo/Caso-Ej-3/`, sin modificaciones.
- Las ejecuciones finales se hicieron en conversaciones nuevas de ChatGPT anónimo. La interfaz no
  expuso modelo, temperatura, seed ni otros parámetros; no se infieren ni se afirma determinismo.

La configuración, los árboles Git de los casos y los hashes de los mensajes completos quedaron
registrados antes de ejecutar en
`casos/calibracion-final-A6/16_expectativa_final_previa.md`. La sincronización final de Sonda está
en `casos/calibracion-final-A6/21_sincronizacion_sonda_final.md`.

## Expectativa humana previa por dimensión

| Caso | D1 | D2 | D3 | D4 | D5 | Total | Alerta esperada |
|---|---:|---:|---:|---:|---:|---:|---|
| Excelente | Insuficiente 10 | Excelente 25 | Excelente 15 | Excelente 15, **AMBIGUO** | Excelente 15 | 80 | No |
| Flojo | Insuficiente 10 | Insuficiente 8 | Insuficiente 5 | Insuficiente 5 | Insuficiente 5 | 33 | No |
| Tramposo | Insuficiente 10 | Insuficiente 8 | Insuficiente 5, **AMBIGUO** | Ausente 0 | Insuficiente 5 | 28; alternativa 33 | Sí |

La expectativa no se tomó de resultados históricos del agente, sino de la rúbrica vigente y de la
evidencia real de cada caso:

- **Excelente:** D1 debía bajar por acciones y compromisos materiales no respaldados repetidos en
  las corridas; D2, D3 y D5 cumplían todos sus requisitos. D4 se marcó ambiguo solo por la duda de
  si aceptar tokens autodeclarados, requisito que la rúbrica no excluye.
- **Flojo:** los prompts y corridas justificaban D1 bajo; `DECISIONES.md` no narraba iteraciones;
  había dos huecos simultáneos de reproducibilidad; “El modelo no es caro” justificaba D4
  Insuficiente y avisar a un humano justificaba D5 Insuficiente.
- **Tramposo:** Gmail/OAuth, producción, precisión e iteraciones no tenían respaldo; las corridas
  carecían de fecha; las cifras económicas eran internamente incompatibles; el gobierno era
  genérico. D3 admitía como alternativa Bueno si la ausencia repetida de fecha se consideraba un
  único tipo de hueco.

## Línea base y desacuerdo material de A6

La primera recalibración posterior a A2–A5 está conservada en los archivos `00`–`04`. Dio
**80/100** al excelente, **28/100** al flojo y **47/100** al tramposo. Excelente y tramposo
mantuvieron las detecciones centrales esperadas, pero D4 del flojo quedó en `Ausente — 0/15`
aunque el evaluador citó “El modelo no es caro”, frase equivalente al ejemplo entonces escrito
para `Insuficiente — 5/15`. El diagnóstico fue B: un único desacuerdo material antes de cerrar la
documentación.

## Iteración fallida del system prompt

Primero se probó una regla general de selección de nivel en `agente/system_prompt.md`. La ejecución
`05` mantuvo D4 en `Ausente — 0/15` y además D5 varió de 5 a 0. La comparación `06` clasificó la
prueba como no validada. El cambio fue revertido y `agente/system_prompt.md` volvió exactamente a
HEAD; los archivos `05_resultado_flojo_despues_regla_seleccion_nivel.md` y
`06_comparacion_correccion_seleccion_nivel.md` se conservaron como evidencia de la iteración
fallida.

## Aclaración específica D4

Se modificó únicamente la frontera `Insuficiente`/`Ausente` de D4 para establecer que una
consideración económica cualitativa como “el modelo no es caro” es evidencia mínima y que
`Ausente` exige que no exista consideración económica alguna. La ejecución `07` corrigió D4 de 0
a 5 sin cambiar D1, D2, D3 ni D5.

Esa salida informó **31/100**, aunque sus filas sumaban `10 + 8 + 5 + 5 + 5 = 33`. Las dos
repeticiones controladas `09` y `10` mantuvieron D4 en 5 y calcularon correctamente sus propios
totales, 33 y 28 respectivamente. Por eso `11_comparacion_replicacion_D4.md` documenta el 31 como
un error aritmético aislado, no como un fallo persistente demostrado.

## Variabilidad D5 y aclaración específica

La diferencia entre las dos repeticiones estuvo en D5: E0 y R1 asignaron
`Insuficiente — 5/15`, mientras R2 asignó `Ausente — 0/15` reconociendo esencialmente la misma
mención de revisión humana. La auditoría `12_auditoria_D5.md` determinó que ambos niveles eran
razonables bajo el texto anterior: `Insuficiente` aceptaba una frase genérica, mientras `Ausente`
dependía de que no existiera una sección dedicada.

Se aclararon solo esas dos filas para que cualquier consideración de gobierno, riesgo o
supervisión humana pueda justificar Insuficiente aunque esté fuera de una sección dedicada. La
ejecución `13` dejó D5 en 5, mantuvo D1–D4 y calculó 33 correctamente. La comparación está en
`14_comparacion_aclaracion_D5.md`.

## Auditoría de la alerta anti-trampa

La ejecución `13` activó por primera vez la alerta anti-trampa del caso flojo usando “Anda bien” y
“No hubo grandes problemas”. La auditoría textual `15_auditoria_alerta_antitrampa.md` comprobó que
esas frases eran genéricas o carecían de evidencia, pero no demostraban contradicciones que
hubieran bajado causalmente dos dimensiones ni un número falso. La alerta fue una aplicación
variable no respaldada por las reglas, que ya eran suficientemente claras. Se conservó la salida
y no se agregó ninguna norma nueva para acomodar una ejecución aislada.

## Validación final

Con la expectativa final fijada en `16_expectativa_final_previa.md`, se ejecutó una sola vez cada
caso sin cambiar la configuración entre corridas. Las respuestas completas están en `17`, `18` y
`19`; la comparación está en `20_comparacion_final.md`.

| Caso | D1 | D2 | D3 | D4 | D5 | Total vigente | Alerta anti-trampa |
|---|---:|---:|---:|---:|---:|---:|---|
| Excelente | Insuficiente 10 | Excelente 25 | Excelente 15 | Excelente 15 | Excelente 15 | **80/100** | No |
| Flojo | Insuficiente 10 | Insuficiente 8 | Insuficiente 5 | Insuficiente 5 | Insuficiente 5 | **33/100** | No |
| Tramposo | Insuficiente 10 | Bueno 17 | Bueno 10 | Insuficiente 5 | Insuficiente 5 | **47/100** | Sí |

Las tres sumas son correctas. D4 y D5 del flojo coinciden con la expectativa aclarada y no aparece
la alerta improcedente. El excelente conserva la reducción de grounding en D1. El tramposo queda
inequívocamente marcado y cita producción, volumen, precisión, Gmail/OAuth, iteraciones y cifras
económicas no respaldadas o incompatibles.

## Comparación humano vs. evaluador

| Caso y dimensión | Clasificación | Explicación |
|---|---|---|
| Excelente D1–D5 | **COINCIDE** | Los cinco niveles y el total coinciden; D1 cita el grounding repetido. |
| Flojo D1–D5 | **COINCIDE** | Los cinco niveles coinciden; las aclaraciones D4 y D5 se aplican con la evidencia esperada. |
| Tramposo D1 y D5 | **COINCIDE** | Se reconocen falta de herramienta/supervisión concreta y gobierno genérico. |
| Tramposo D2 | **DIFERENCIA JUSTIFICABLE** | El evaluador leyó “47 iteraciones” como iteraciones narradas pero genéricas, ejemplo compatible con Bueno; la tensión con la regla de afirmaciones no verificadas ya estaba documentada. |
| Tramposo D3 | **DIFERENCIA JUSTIFICABLE** | Adoptó la alternativa Bueno registrada antes de ejecutar: la falta de fecha en las tres corridas como un único tipo de hueco. |
| Tramposo D4 | **DIFERENCIA JUSTIFICABLE** | Demostró la inconsistencia USD 0,48 vs. USD 0,05 y aplicó la instrucción específica de tratar números que no cierran como Insuficiente; la regla anti-trampa también permite igualar o empeorar la ausencia. |

Las diferencias del tramposo elevan el total respecto de la expectativa primaria, pero no cambian
la conclusión evaluativa central y corresponden a límites interpretativos declarados antes o ya
documentados. No se ajustó la expectativa después de ver la salida para fabricar coincidencia.

## Conclusión y límites

**Diagnóstico final: A) CONFIGURACIÓN FINAL ACEPTABLE.**

La configuración actual ofrece mayor consistencia, trazabilidad a evidencia y reproducibilidad
controlada, pero no determinismo absoluto. La muestra conservó variabilidad residual en D5, en una
alerta anti-trampa y en un total aritmético aislado. Las auditorías distinguieron qué variaciones
justificaban aclarar una frontera y cuáles no justificaban agregar reglas.

Quedan además límites operativos: Sonda mantiene copias embebidas que deben resincronizarse ante
cambios futuros; evalúa la rama por defecto y no fija una rama o commit incluido en la URL; y no
siempre transmite la causa precisa de archivos omitidos por error, tipo, tamaño o truncamiento.
A2 valida resistencia y detección para el ataque directo ensayado, no para todas las formas
posibles de prompt injection.
