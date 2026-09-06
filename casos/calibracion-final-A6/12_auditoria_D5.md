# A6 — Auditoría específica D5

## Descriptor vigente

Texto literal de `rubrica.md`:

| Nivel | Puntos | Evidencia que lo exige |
|---|---|---|
| Excelente | **15** | Dice qué sistemas/datos toca el agente y con qué permisos · qué puede salir mal **y** qué pasa concretamente cuando sale mal · qué revisa un humano antes de confiar en la salida · quién firma el resultado. |
| Bueno | **10** | Cubre la mayoría de los puntos pero de forma superficial (ej. dice qué puede salir mal pero no qué se hace al respecto). |
| Insuficiente | **5** | Hay una mención genérica de una frase sin desarrollo ("hay que tener cuidado con los datos"). |
| Ausente | **0** | No hay ninguna sección de gobierno o riesgo. |

## Evidencia del caso flojo

| Archivo | Fragmento textual exacto | Elemento de D5 que podría satisfacer | Nivel que podría justificar |
|---|---|---|---|
| `prompts/system_prompt.md` | “Si es un reclamo, avisame.” | Escalamiento o aviso a una persona ante un tipo de caso. No define quién recibe el aviso, qué revisa, qué decide ni qué ocurre después. | `Insuficiente — 5/15`: es una mención genérica de una frase sin desarrollo. |
| `corridas/corrida_2.md` | “Habría que avisarle a un humano antes de mandar nada, supongo.” | Revisión o intervención humana antes de enviar una salida, uno de los elementos nombrados en Excelente. Está expresado de forma condicional y sin procedimiento, permisos ni responsable. | `Insuficiente — 5/15`: es una mención genérica de una frase sin desarrollo. |
| `README.md` | “Por ahí a veces contesta distinto de otras veces pero en general funciona.” | Reconocimiento genérico de algo que puede salir mal: variación de respuestas. No explica impacto, mitigación ni responsable. | A lo sumo `Insuficiente — 5/15`; es evidencia secundaria y ambigua de riesgo. |
| `corridas/corrida_2.md` | “Le respondería algo como pidiendo disculpas y que se va a resolver el reembolso.” | Muestra una salida potencialmente riesgosa o comprometida, pero no documenta por sí misma gobierno, controles ni responsabilidad. | No justifica un nivel de D5 por sí sola. |
| `DECISIONES.md` | “No hubo grandes problemas.” | No identifica un riesgo ni una medida de gobierno concreta. | No justifica un nivel de D5. |

La evidencia más directa es “Habría que avisarle a un humano antes de mandar nada, supongo”. Esa
frase coincide con el tipo de evidencia mínima descripta por `Insuficiente`: una mención genérica,
de una frase y sin desarrollo. No alcanza `Bueno` porque no cubre la mayoría de los elementos de
D5 ni siquiera superficialmente.

Al mismo tiempo, el caso no contiene una sección dedicada titulada o estructurada como gobierno o
riesgo. Por lo tanto, también cumple literalmente la condición escrita para `Ausente`: “No hay
ninguna sección de gobierno o riesgo”. La rúbrica no establece si una mención genérica ubicada en
un prompt o una corrida prevalece sobre la inexistencia de una sección dedicada.

## D5 en E0

Nivel y puntaje: `Insuficiente — 5/15`.

Evidencia citada en la tabla:

> `system_prompt.md` solo indica “Si es un reclamo, avisame.” `corrida_2.md` agrega “Habría que
> avisarle a un humano antes de mandar nada, supongo”, pero no define permisos, tratamiento de
> fallas, revisión formal ni quién firma.

Justificación completa:

> La única regla explícita es “Si es un reclamo, avisame” en `system_prompt.md`. En
> `corrida_2.md` aparece la idea condicional de avisar a un humano, pero no se establecen niveles
> L0–L4, permisos, sistemas/datos involucrados, procedimiento ante errores o responsable que
> firme el resultado.

E0 interpretó las dos frases como evidencia genérica de escalamiento y supervisión humana. Al no
estar desarrolladas, aplicó `Insuficiente`.

## D5 en R1

Nivel y puntaje: `Insuficiente — 5/15`.

Evidencia citada en la tabla:

> En `corridas/corrida_2.md` se menciona “Habría que avisarle a un humano antes de mandar nada”,
> pero no se detallan sistemas, permisos, manejo de fallas, revisión formal ni quién firma.

Justificación completa:

> El caso de reclamo de `corrida_2.md` reconoce que “Habría que avisarle a un humano antes de
> mandar nada”, pero no existe una política concreta de permisos, tratamiento de errores, revisión
> humana o firma del resultado.

R1 aplicó la misma lectura sustantiva que E0: existe una mención humana relevante, pero es una
frase aislada y no una política desarrollada.

## D5 en R2

Nivel y puntaje: `Ausente — 0/15`.

Evidencia citada en la tabla:

> No existe una sección o evidencia que indique sistemas/datos y permisos, riesgos, tratamiento
> de errores, revisión humana o quién firma el resultado.

Justificación completa:

> No hay evidencia de permisos o sistemas/datos involucrados, manejo concreto de errores,
> revisión humana previa ni responsable que firme el resultado. La mención de avisar ante un
> reclamo en `system_prompt.md` no desarrolla estos requisitos.

R2 reconoció la mención de aviso, pero exigió que desarrollara los requisitos superiores para
contarla. También puso el foco en que no existe una sección o política de gobierno. Esa lectura
lleva al descriptor literal de `Ausente`, aunque deja sin efecto el descriptor de `Insuficiente`
para una frase genérica sin desarrollo.

## Comparación

| Aspecto | E0 | R1 | R2 |
|---|---|---|---|
| Reconoce aviso o intervención humana | Sí | Sí | Sí, en la justificación |
| Considera la frase evidencia mínima de D5 | Sí | Sí | No |
| Exige sección o política desarrollada para evitar Ausente | No | No | Sí |
| Nivel | Insuficiente | Insuficiente | Ausente |
| Puntos | 5/15 | 5/15 | 0/15 |

E0 y R1 aplicaron literalmente el descriptor de `Insuficiente`: existe una mención genérica de
una frase sin desarrollo. R2 aplicó literalmente el descriptor de `Ausente`: no existe una
sección de gobierno o riesgo. La diferencia no surge de evidencia distinta, sino de que ambos
descriptores pueden activarse simultáneamente y la rúbrica no define cuál prevalece.

## Diagnóstico A/B/C/D

**C) Los descriptores actuales permiten razonablemente ambas interpretaciones.**

La evidencia encaja de forma directa en `Insuficiente`, especialmente la frase “Habría que
avisarle a un humano antes de mandar nada, supongo”. Sin embargo, el caso también carece de una
sección dedicada de gobierno o riesgo, condición literal de `Ausente`. Esta superposición explica
la diferencia entre E0/R1 y R2 y constituye una ambigüedad real de frontera en la rúbrica.

No se propone todavía una solución textual y no se realizó ninguna nueva ejecución.
