# Resultado completo A6 — Caso excelente

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Entrada: configuración y caso congelados en `00_configuracion_y_expectativa_previa.md`.
- El nombre exacto del modelo, temperatura, seed y otros parámetros no fueron expuestos por la
  interfaz; no se infieren.
- La línea “Procesó durante 11s” fue mostrada por la interfaz y no forma parte del informe del
  evaluador.

## Respuesta completa

# Corrección — Caso-Ej-1

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `prompts/system_prompt.md` y `user_prompt.md` están presentes y las 3 corridas muestran el mismo formato. Sin embargo, no hay una herramienta o conector real verificable: solo se proveen archivos CSV. Además, el grounding falla repetidamente: `corrida_1.md` afirma “ya lo estamos consultando con logística”, `corrida_3.md` “Iniciamos tu cambio de talle” y `corrida_3.md` da “5 y 8 días hábiles” para Bariloche sin evidencia de esas capacidades o datos en la entrada. |
| Proceso documentado | Excelente | 25/25 | `DECISIONES.md` documenta dos iteraciones concretas: el cambio de JSON a tabla markdown y la incorporación de la regla de derivación. También reconoce la falla real del ticket 104 de `corrida_2.md` y compara su mitigación con `corrida_3.md`. |
| Formato y reproducibilidad | Excelente | 15/15 | Están presentes `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`. Hay exactamente 3 corridas, fechadas el 2026-08-25, 2026-08-27 y 2026-08-29, con entradas CSV y salidas reconstruibles. |
| Análisis económico | Excelente | 15/15 | `ANALISIS_ECONOMICO.md` presenta tokens de entrada/salida para las tres corridas, calcula un costo promedio de “USD 0,00036”, proyecta costo diario/semanal/anual y justifica usar un modelo mini mediante el criterio “el modelo más chico que hace bien la tarea”. |
| Gobierno y riesgo | Excelente | 15/15 | `GOBIERNO.md` especifica acceso de “lectura únicamente”, riesgos y mitigaciones, revisión humana de tickets sensibles y muestreo 1/5. Además define explícitamente L0–L4 y quién firma en L4. |

## Puntaje total: 80/100

## Justificación por dimensión

**Sistema completo y funcionando:** Los prompts definen objetivo, categorías, reglas y formato, y las tres corridas respetan la tabla. Sin embargo, el repositorio no demuestra una herramienta o conector real usado en ejecución. Además, varias salidas presentan capacidades o datos no respaldados por la entrada, por ejemplo “ya lo estamos consultando con logística” (`corrida_1.md`) e “Iniciamos tu cambio” (`corrida_3.md`), por lo que el grounding limita esta dimensión a Insuficiente.

**Proceso documentado:** `DECISIONES.md` contiene dos iteraciones concretas con cambios específicos y una falla real reconocida. La sección “Falla real encontrada y no resuelta del todo (corrida 2)” identifica el ticket 104, explica el error y documenta la mitigación posterior en `corrida_3.md`.

**Formato y reproducibilidad:** La estructura obligatoria está completa y hay tres corridas con fecha, archivo de entrada y salida. Los CSV correspondientes están efectivamente presentes en `corridas/`, permitiendo reconstruir las entradas de cada ejecución sin adivinar su contenido.

**Análisis económico:** `ANALISIS_ECONOMICO.md` aporta cifras por corrida, promedia los tokens, calcula el costo por corrida y por ticket, y proyecta el costo a 50 consultas diarias, con una proyección anual de USD 1,83. También explicita la elección del modelo con el criterio requerido.

**Gobierno y riesgo:** `GOBIERNO.md` identifica qué datos puede leer el agente y qué sistemas no puede tocar, describe riesgos concretos y sus mitigaciones, establece revisión obligatoria y muestral, y define los niveles L0–L4, incluyendo al responsable que firma en L4.

## Señales de alerta

La salida de `corrida_1.md` afirma que “ya lo estamos consultando con logística”, aunque el propio `GOBIERNO.md` dice que el agente solo tiene acceso de lectura a los CSV y “no tiene acceso a la casilla de mail real, ni a los sistemas de pago, ni a la base de clientes”. En `corrida_3.md`, “Iniciamos tu cambio de talle” también presenta una acción ejecutada que no está respaldada por ninguna herramienta o conector. Asimismo, la respuesta sobre Bariloche (“5 y 8 días hábiles”) no surge de los datos de entrada ni de otra evidencia del repositorio.

## Sugerencia concreta de mejora

Incorporar un conector o herramienta verificable para las acciones y datos que hoy se presentan como reales —por ejemplo, consulta de logística y tiempos de envío— y modificar el prompt para que, cuando esa evidencia no esté disponible, el agente responda explícitamente que no puede verificarla en lugar de afirmar que realizó una acción o conoce un dato externo.
