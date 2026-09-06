# A3 — Resultado completo de la línea base (ANTES)

## Condiciones de ejecución

- Fecha de ejecución: 2026-09-06.
- Rama y revisión evaluadas: `mejoras-eber`, `aac6c0b65d0e2fe9aeb0529650510e2fb038104d`.
- Evaluador: ChatGPT anónimo en `chatgpt.com`.
- Entrada: `agente/system_prompt.md` vigente (incluida A2), `rubrica.md` vigente y los 12 archivos completos de `casos/excelente/Caso-Ej-1/`.
- Longitud exacta del mensaje enviado: 33.957 caracteres.
- SHA-256 del mensaje enviado: `96e1623e594c275a029a4643787a094c926b21a9a20987afa4a8dbecb47d2306`.
- Conversación: <https://chatgpt.com/uc/6a9d7afb-070c-83ea-b532-34f4193dcfac>.
- Limitación: la interfaz anónima no expuso el nombre exacto del modelo, temperatura, seed ni otros parámetros. No se infirieron ni inventaron.

## Respuesta completa

La siguiente es la transcripción completa del contenido renderizado de la respuesta:

# Corrección — Caso-Ej-1

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Bueno | 21/30 | `prompts/system_prompt.md` y `prompts/user_prompt.md` están presentes y completos; las 3 corridas muestran entradas CSV y salidas en tabla. La supervisión L0–L4 está definida en `GOBIERNO.md`. Falta evidencia verificable de una herramienta o conector real usado en las corridas. |
| Proceso documentado | Excelente | 25/25 | `DECISIONES.md` documenta dos iteraciones concretas: cambio de JSON a tabla markdown y agregado de la regla de derivación. Además reconoce la falla real del ticket 104 en la corrida 2 y muestra la mitigación posterior. |
| Formato y reproducibilidad | Excelente | 15/15 | Existen `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`. `corrida_1.md`, `corrida_2.md` y `corrida_3.md` incluyen fecha, entrada completa y salida completa, con sus respectivos CSV. |
| Análisis económico | Excelente | 15/15 | `ANALISIS_ECONOMICO.md` calcula tokens de entrada/salida por las tres corridas, costo promedio por corrida, costo por ticket y proyección diaria/semanal/anual. También justifica usar un modelo mini bajo el criterio de "el modelo más chico que hace bien la tarea". |
| Gobierno y riesgo | Excelente | 15/15 | `GOBIERNO.md` especifica acceso de solo lectura, riesgos y mitigaciones, revisión humana obligatoria y muestral, y niveles L0–L4 con responsable de firma. |

## Puntaje total: 91/100

## Justificación por dimensión

**Sistema completo y funcionando:** Los prompts definen identidad, categorías, reglas, herramienta de entrada y formato de salida, y las tres corridas muestran tablas con el mismo formato. `GOBIERNO.md` define supervisión L0–L4. Sin embargo, el requisito de herramienta/conector real usado en las corridas no queda verificablemente demostrado: el repo muestra archivos CSV y texto incorporado, pero no evidencia un conector o herramienta ejecutada.

**Proceso documentado:** `DECISIONES.md` contiene dos iteraciones concretas con problemas y cambios específicos: el JSON mal formado y la falla de derivación de un reembolso. También reconoce explícitamente la falla del ticket 104 de `corrida_2.md` y documenta que la mitigación no se considera definitivamente resuelta.

**Formato y reproducibilidad:** La estructura obligatoria está completa y hay exactamente tres corridas. Cada `corrida_N.md` tiene fecha, referencia al CSV de entrada, contenido de entrada y tabla de salida; por ejemplo, `corrida_3.md` está fechada 2026-08-29 y contiene tanto la entrada como la salida.

**Análisis económico:** `ANALISIS_ECONOMICO.md` presenta 620/210, 540/160 y 510/150 tokens para las tres corridas y calcula un promedio de ~557 tokens de entrada y ~173 de salida. La aritmética del costo promedio y de la proyección a 50 consultas diarias es consistente con las cifras declaradas, y la elección del modelo está vinculada explícitamente al criterio de usar el modelo más chico que hace bien la tarea.

**Gobierno y riesgo:** `GOBIERNO.md` identifica los sistemas tocados y los permisos, describe tres riesgos concretos y sus mitigaciones, establece revisión humana para `revision_humana: sí` y una muestra de 1 cada 5 para los demás, y define L0–L4 incluyendo que el responsable de atención al cliente firma la tanda antes de su salida por el canal real.

## Señales de alerta

No se cumple ninguna de las dos condiciones que obligan a abrir con ⚠️ Posible caso de trabajo tramposo detectado. La única reducción por evidencia insuficiente corresponde a la dimensión 1 y no surge de una contradicción matemática ni de una inconsistencia interna entre afirmaciones y corridas. Los números económicos declarados son aritméticamente consistentes con los propios datos presentados.

## Sugerencia concreta de mejora

Agregar al repo evidencia verificable del uso de una herramienta o conector real —por ejemplo, un registro de una ejecución que muestre explícitamente la lectura de uno de los CSV mediante esa herramienta— para que la dimensión Sistema completo y funcionando pueda alcanzar Excelente.
