# Análisis económico — TP Final

## Cómo se mide

`scripts/run_agent.py` es la única fuente de estos números: llama a la API de Anthropic con `prompts/system_prompt.md` como system prompt y un caso de `prompts/casos/` como input, y guarda `usage.input_tokens` / `usage.output_tokens` reales de la respuesta en la cabecera de cada archivo de `corridas/raw/`. No se estiman tokens contando palabras — el propio agente tiene la restricción de no inventar cifras (BoM), y este análisis sigue el mismo criterio.

Hay **tres generaciones de evidencia** en `corridas/raw/` (20 archivos en total), porque el `system_prompt.md` cambió dos veces de forma sustancial:

| Generación | Fecha | Casos | Contrato | sha256 (12) | Estado |
| --- | --- | --- | --- | --- | --- |
| 1 | 2026-09-01 | 001–003 | Sin Ejemplos ni techos de extensión | *(campo no existía aún)* | Histórica |
| 2 | 2026-09-05 (temprano) | 001–003 | Con Ejemplos y techos de extensión | `ad45bc6c055a` | Histórica — superada por la generación 3 |
| 3 | 2026-09-05 (tarde) | 001–004 | + restricción anti-fuga de contenido + caso 004 (escalación) | `76d91e443f52` | **Vigente — es la que describe el sistema actual** |

Esta sección usa la generación 3 como fuente de verdad. Las generaciones 1 y 2 se conservan y se citan para mostrar, con datos reales, el efecto de cada cambio — nunca se estimó un efecto sin volver a medirlo.

Para reproducirlo:

```bash
pip install -r scripts/requirements.txt
export ANTHROPIC_API_KEY=sk-ant-...
python scripts/run_agent.py prompts/casos/solicitud_001_acme.md --model claude-sonnet-5 --max-tokens 28000
```

Nota operativa encontrada al correrlo por primera vez (generación 1, 2026-09-01): con `max_tokens` bajo (4096), Sonnet 5 —que piensa por defecto (`thinking: adaptive`)— gastaba casi todo el presupuesto en razonamiento y devolvía casi nada de texto visible en 2 de los 3 casos. Se subió `max_tokens` a 16000 por defecto (configurable con `--max-tokens`) y se pasó a `client.messages.stream(...)` para evitar el error del SDK de Anthropic sobre streaming obligatorio en respuestas largas. Detalle completo en `DECISIONES.md`.

## Precios usados (API de Anthropic, USD por millón de tokens)

| Modelo | Input | Output |
| --- | --- | --- |
| `claude-haiku-4-5` | $1.00 | $5.00 |
| `claude-sonnet-5` | $3.00 | $15.00 |

Estos son los precios **de lista** de ambos modelos. Sonnet 5 tuvo una tarifa introductoria de $2.00 / $10.00 vigente **hasta el 2026-08-31**; la generación 1 de evidencia (2026-09-01) es un día posterior, así que ya corresponde la tarifa estándar. Los costos de las generaciones 1 y 2 mostrados abajo están recalculados con esta tarifa; las cabeceras de los 6 archivos de la generación 1, sin editar, todavía traen el número calculado con la tarifa introductoria que el script tenía hardcodeada ese día — es una discrepancia deliberada, explicada en `DECISIONES.md`, y no se reescribieron esos archivos porque son evidencia guardada tal como salió.

## Qué cambió al agregar la Pieza 6 (Ejemplos) y los techos de extensión — medido, no estimado

La revisión que agregó la Pieza 6 dejaba una advertencia: agregar 3 ejemplos completos al `system_prompt.md` iba a triplicar el input, y los techos de extensión por sección iban a reducir el output, con un efecto neto "indeterminado hasta volver a medir". Se remidió con una corrida real de los 3 casos (generación 2, contrato idéntico en las 6 corridas).

| | Sonnet 5 — antes (gen. 1) | Sonnet 5 — después (gen. 2) | Haiku 4.5 — antes (gen. 1) | Haiku 4.5 — después (gen. 2) |
| --- | --- | --- | --- | --- |
| Input promedio | 4.637 tok | 19.807 tok (+327%) | 3.486 tok | 14.940 tok (+329%) |
| Output promedio | 10.768 tok | 5.784 tok (**-46%**) | 5.810 tok* | 4.550 tok |
| Costo promedio | USD 0,1754 | USD 0,1462 (-17%) | USD 0,0325 | USD 0,0377 (+16%) |

\* El promedio de output "antes" de Haiku está inflado por la comparación: en esa generación, Haiku abandonaba el flujo en BMINING después de la Fase 2, así que su output real por corrida completa era mayor que 5.810 cuando efectivamente completaba el trabajo.

**El resultado no fue el esperado, y fue una buena noticia:** el costo de Sonnet 5 bajó pese a que el contrato casi se cuadruplicó en tamaño, porque el output —que domina el costo a $15/M tok— se redujo a menos de la mitad por los techos de extensión. Ninguno de los dos escenarios especulados ("el costo se va a triplicar") se cumplió.

## Segundo cambio: restricción anti-fuga + caso de escalación — generación 3 (vigente)

Al comparar modelos con la generación 2 apareció un hallazgo no buscado (detallado más abajo, sección "Elección de modelo"): Haiku 4.5 imitaba el párrafo que cierra cada ejemplo del contrato y, en un caso, mencionó por nombre a un cliente ficticio de esos ejemplos dentro de una salida real. Se aplicaron dos cambios y se volvió a correr todo:

1. **Restricción explícita en `# Restricciones`:** *"No agregues ningún contenido, comentario, sección o nota fuera de la estructura que define la sección '# Formato'."* + un cierre explícito en el propio Formato: *"Tu respuesta termina en 'Próximas acciones recomendadas': no agregues nada después."*
2. **Caso 004 (ANDINA LITIO):** un caso nuevo, diseñado para disparar dos de los cinco criterios de escalación (ruta transfronteriza con estatus regulatorio incierto + ingeniería de línea fotónica a medida por tramos extremos sin energía), agregado porque ninguno de los 3 casos originales ejercitaba esa rama del contrato.

Se corrieron los 4 casos × 2 modelos (8 corridas, sha256 `76d91e443f52` en las 8, todas con `stop_reason: end_turn`).

## Costo por corrida — generación vigente (medido el 2026-09-05, contrato con anti-fuga + caso de escalación)

| Caso | Modelo | Tokens in | Tokens out | Costo (USD) |
| --- | --- | --- | --- | --- |
| Solicitud 001 (ACME) | `claude-sonnet-5` | 19959 | 4520 | 0.1277 |
| Solicitud 002 (SYNNEX) | `claude-sonnet-5` | 20037 | 6197 | 0.1531 |
| Solicitud 003 (BMINING) | `claude-sonnet-5` | 20161 | 10079 | 0.2117 |
| **Promedio (001–003)** | `claude-sonnet-5` | **20052** | **6932** | **0.1642** |
| Solicitud 004 (ANDINA, escalación) | `claude-sonnet-5` | 20037 | 8658 | 0.1900 |
| Solicitud 001 (ACME) | `claude-haiku-4-5` | 15050 | 3981 | 0.0350 |
| Solicitud 002 (SYNNEX) | `claude-haiku-4-5` | 15099 | 2760 | 0.0289 |
| Solicitud 003 (BMINING) | `claude-haiku-4-5` | 15191 | 4886 | 0.0396 |
| **Promedio (001–003)** | `claude-haiku-4-5` | **15113** | **3876** | **0.0345** |
| Solicitud 004 (ANDINA, escalación) | `claude-haiku-4-5` | 15098 | 3056 | 0.0304 |

El promedio de las 3 corridas exigidas por la consigna (001–003) es el que se usa en la proyección de costo. La Solicitud 004 se reporta aparte porque es un caso complementario agregado para probar la escalación, no parte del volumen regular estimado.

**Hallazgo adicional: el costo no es estable ni siquiera con el mismo caso, modelo y criterio de fondo.** BMINING con Sonnet 5 costó 0.1537 en la generación 2 y 0.2117 en la generación 3 —un salto del 38%— pese a que la única diferencia de contrato entre ambas corridas es una restricción de una línea. La causa no es el contrato: es que el modelo, en esa corrida puntual, exploró con más profundidad la posible segmentación funcional IT/OT de los canales (algo que la Solicitud 003 sugiere pero no confirma) y escribió una nota de ingeniería más larga. El formato y las restricciones de contenido se mantuvieron intactos en ambas corridas — lo que varió fue la extensión de las secciones en prosa, que los techos de la Pieza 4 no acotan tan estrictamente como a las secciones con viñetas. Esto amplía la limitación #4 del agente ("no es determinístico"): la variación no es solo de redacción, también puede ser de costo, y con esta única muestra no alcanza para saber si 38% es un caso extremo o algo frecuente.

## Elección de modelo — "el más chico que hace bien la tarea"

Haiku 4.5 es exactamente 3× más barato que Sonnet 5 por token, así que sigue siendo el candidato natural por costo. La comparación pasó por tres rondas de evidencia real, cada una encontrando algo que la anterior no había visto.

### Ronda 1 (generación 2): los problemas estructurales de Haiku se corrigieron

Antes de la Pieza 6, Haiku producía el formato exacto en 1 de 3 casos y abandonaba el flujo en BMINING después de la Fase 2. Con los 3 ejemplos agregados, Haiku produjo el formato exacto en los 3 casos y completó las 6 fases en los 3 casos. En ningún BoM de ningún modelo apareció un precio inventado, en ninguna de las dos generaciones.

### Ronda 2 (generación 2): apareció una fuga

El `system_prompt.md` cierra cada ejemplo de la Pieza 6 con un párrafo `**Por qué esta salida es correcta:** ...`. Haiku 4.5 imitó ese patrón: en 2 de 3 corridas agregó una sección propia no pedida por el Formato, y en BMINING esa sección mencionó por nombre a **QUILPO, el cliente ficticio del Ejemplo 3 del propio contrato**, como si fuera un cliente real de esa conversación. Sonnet 5 no lo hizo en ninguna corrida.

### Ronda 3 (generación 3): la fuga se corrigió, pero apareció algo más específico

Se agregó la restricción anti-fuga y se volvió a correr todo. **La fuga original desapareció por completo: 0 de 8 corridas nuevas repiten el patrón de la Ronda 2** — ni la sección extra, ni la mención a ningún cliente de los ejemplos (AURELIA, NORDEX, QUILPO).

Pero el caso 004 (escalación) — un tipo de escenario que **ningún ejemplo del contrato cubre** — reveló algo más preciso que "Haiku imita a los ejemplos":

- **Sonnet 5 manejó la escalación sin ninguna desviación de formato.** Declaró la escalación explícitamente dentro de "# Resumen de la oportunidad", nombró los dos factores, completó Fases 1–2, omitió correctamente "# Inconsistencias detectadas" (no había ninguna) y las secciones de Fases 3–5, y terminó en "# Próximas acciones recomendadas" sin nada después.
- **Haiku 4.5, en el mismo caso, inventó dos encabezados que no existen en el Formato** (`# ESCALACIÓN REQUERIDA`, `# Recomendaciones inmediatas`), **volvió a emitir "# Inconsistencias detectadas" cuando no había ninguna** —el mismo error que el Ejemplo 1 corrige en los 3 casos conocidos— y agregó una "### Nota de gobernanza" después del cierre del bloque de formato.

El diagnóstico correcto no es "Haiku imita el comentario de los ejemplos": es que **el cumplimiento de formato de Haiku depende de que el escenario se parezca a alguno de los 3 ejemplos**. Ante un tipo de caso nuevo, vuelve a inventar estructura. Sonnet generaliza el Formato como una regla, no como un patrón a copiar; Haiku lo trata más como lo segundo.

- **Los techos de extensión (Pieza 4) se respetaron en 23 de 24 mediciones** de las 6 corridas de los casos 001–003 (4 secciones con techo numérico × 6 corridas): una sola se pasó por un ítem.
- **No se inventó ningún precio de BoM** en ninguna de las 8 corridas de esta generación.
- **La escalación, en cuanto a contenido (no de formato), se manejó correctamente por los dos modelos:** ambos identificaron los dos factores de escalación, ambos completaron el análisis de requisitos y brechas sin avanzar con arquitectura, cotización o nota de ingeniería, y ninguno inventó una viabilidad regulatoria o técnica que el caso no sostiene.

### Conclusión

Se mantiene `claude-sonnet-5` como modelo del sistema. La razón volvió a cambiar, y cada vez con más precisión: no es que Haiku no complete el flujo (se corrigió), ni solo que copie el comentario de los ejemplos (también se corrigió). Es que el cumplimiento de formato de Haiku no generaliza a escenarios que los ejemplos del contrato no cubren, mientras que el de Sonnet sí. Para un sistema cuya corrección la hace un agente evaluador automático y donde nuevos tipos de caso van a aparecer con el tiempo —no todos los futuros clientes van a parecerse a AURELIA, NORDEX o QUILPO—, esa diferencia importa más que el costo: Sonnet cuesta 0.1642 por corrida contra 0.0345 de Haiku, una diferencia de 13 centavos.

Un candidato para una futura iteración —no aplicado en esta entrega— es agregar un cuarto ejemplo al contrato que cubra específicamente un caso de escalación, siguiendo el mismo patrón que ya funcionó dos veces en este trabajo: un ejemplo generaliza mejor que una instrucción en prosa. Ver `DECISIONES.md`.

## Proyección de costo (semanal / anual)

**Supuesto de volumen** (a ajustar si no representa el caso real): un SE procesa aproximadamente **5 oportunidades nuevas por semana** que ameritan correr el flujo completo (Fases 1–6), más una cantidad similar de re-corridas por iteración dentro de la misma oportunidad (información que llega incompleta y se reprocesa) — estimar un **factor de 2 corridas por oportunidad**.

Con el costo promedio vigente de `claude-sonnet-5` (USD 0.1642 por corrida, promedio de las 3 corridas exigidas):

| Período | Corridas | Costo estimado (USD) |
| --- | --- | --- |
| Por semana | 5 oportunidades × 2 corridas = 10 | 10 × 0.1642 = **1.64** |
| Por año (48 semanas hábiles) | 480 | 480 × 0.1642 = **78.82** |

Como referencia, el mismo volumen con `claude-haiku-4-5` (USD 0.0345 promedio) costaría **16.56 al año** — unos 62 dólares menos. Dado el hallazgo de variabilidad de costo documentado arriba (±38% observado en una sola comparación), estos números son una proyección de orden de magnitud, no una cifra exacta — a esta escala, de todos modos, siguen siendo una fracción despreciable de una sola hora de trabajo del SE.

Esta proyección no incluye el costo de un eventual SE humano revisando cada salida (tiempo de revisión), que es el costo dominante del sistema en producción — el agente reduce el tiempo de primer borrador, no lo reemplaza (ver [`GOBIERNO.md`](GOBIERNO.md)). A ese volumen, el costo de API es marginal frente al costo del tiempo del SE: la justificación económica del sistema está en el tiempo de borrador ahorrado, no en el costo de tokens en sí.
