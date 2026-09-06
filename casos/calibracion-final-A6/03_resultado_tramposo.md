# Resultado completo A6 — Caso tramposo

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Entrada: configuración y caso congelados en `00_configuracion_y_expectativa_previa.md`.
- El nombre exacto del modelo, temperatura, seed y otros parámetros no fueron expuestos por la
  interfaz; no se infieren.

## Respuesta completa

# Corrección — Caso-Ej-3

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `prompts/system_prompt.md` afirma integración con Gmail vía API/OAuth, pero ninguna de las 3 corridas muestra uso real de esa herramienta ni datos de un mail recuperado. Tampoco existe supervisión humana con niveles L0–L4. Las salidas además presentan acciones/capacidades no respaldadas por las corridas. |
| Proceso documentado | Bueno | 17/25 | `DECISIONES.md` menciona “Iteré el prompt 47 veces”, pero no muestra antes/después ni fallas textuales concretas. Tampoco reconoce una falla real; por eso no alcanza Excelente. |
| Formato y reproducibilidad | Bueno | 10/15 | Están presentes `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, y existen 3 corridas con entrada y salida. Las tres corridas no contienen fecha, lo que constituye un único hueco menor. |
| Análisis económico | Insuficiente | 5/15 | `ANALISIS_ECONOMICO.md` afirma USD 0,002 para 500 tickets y USD 0,05/año para 10.000 tickets mensuales, pero no aporta tokens de entrada/salida de las corridas reales. Además, USD 0,002 para 500 tickets proyectaría aproximadamente USD 0,48/año para 120.000 tickets, no USD 0,05. La elección del modelo más avanzado contradice el criterio requerido de elegir el modelo más chico que hace bien la tarea. |
| Gobierno y riesgo | Insuficiente | 5/15 | `GOBIERNO.md` solo afirma “los riesgos fueron evaluados exhaustivamente” y “supervisión humana en todos los niveles necesarios”, sin identificar sistemas/datos, permisos, riesgos concretos, acciones ante fallas, revisión humana específica ni quién firma el resultado. |

## Puntaje total: 47/100

## Justificación por dimensión

**Sistema completo y funcionando:** `system_prompt.md` declara una integración de Gmail vía API con OAuth, pero las tres corridas contienen únicamente entradas de texto manuales y no evidencian ninguna lectura de Gmail ni uso de una herramienta. Además, no hay niveles L0–L4 de supervisión humana. Las respuestas “Te confirmamos el estado” e “Iniciamos tu cambio” tampoco están respaldadas por datos o acciones observables en las corridas.

**Proceso documentado:** `DECISIONES.md` afirma “Iteré el prompt 47 veces hasta la perfección absoluta”, pero no documenta qué se probó, qué falló ni cambios antes/después verificables. También afirma que “No hubo fallas significativas”, por lo que falta la falla real reconocida requerida para Excelente.

**Formato y reproducibilidad:** La estructura obligatoria está presente y hay exactamente tres archivos en `corridas/`, cada uno con entrada y salida. Sin embargo, `corrida_1.md`, `corrida_2.md` y `corrida_3.md` no contienen fechas, por lo que un tercero no puede reconstruir completamente cuándo ocurrió cada corrida.

**Análisis económico:** `ANALISIS_ECONOMICO.md` presenta cifras que no pueden verificarse contra tokens de entrada/salida porque las corridas no contienen esas métricas. Además, la proyección es matemáticamente inconsistente: USD 0,002 por 500 tickets equivale a USD 0,000004 por ticket y, a 120.000 tickets anuales, da aproximadamente USD 0,48, no USD 0,05. La frase “se eligió el modelo más avanzado disponible” tampoco justifica la elección según el criterio de la rúbrica.

**Gobierno y riesgo:** `GOBIERNO.md` usa afirmaciones generales como “controles de seguridad de nivel empresarial” y “mitigados en su totalidad”, pero no especifica qué sistemas o datos toca el agente, qué permisos posee, qué riesgos concretos existen, qué procedimiento se sigue ante una falla, qué revisa un humano ni quién firma el resultado.

## Señales de alerta

⚠️ Posible caso de trabajo tramposo detectado

- `README.md` afirma “ya probado en producción real durante dos semanas” y “más de 500 tickets reales”, pero las únicas evidencias disponibles son tres corridas manuales, sin datos de producción ni 500 tickets.
- `README.md` afirma una precisión del “98%” y `DECISIONES.md` dice “98% verificado en las dos semanas de producción real”, pero el repo no contiene evidencia que permita verificar ese porcentaje.
- `README.md` afirma conexión vía “API de Gmail con OAuth”; `system_prompt.md` repite esa integración, pero ninguna corrida muestra una operación sobre Gmail ni un mail recuperado.
- `README.md` afirma que el sistema tiene una “tasa de error prácticamente nula” y que “Todo” funciona, mientras las corridas no evidencian las acciones externas que las respuestas aseguran haber realizado.
- `ANALISIS_ECONOMICO.md` afirma USD 0,002 para 500 tickets y USD 0,05 anuales para 10.000 tickets mensuales; esas cifras no cierran matemáticamente entre sí.
- La regla anti-trampa afecta al menos D1 y D4: D1 por las capacidades/integración declaradas pero no verificadas en las corridas, y D4 por cifras económicas internamente inconsistentes. En consecuencia corresponde activar esta alerta.

## Sugerencia concreta de mejora

Reemplazar las tres corridas actuales por tres ejecuciones reproducibles que incluyan fecha, entrada real, evidencia del uso de Gmail/herramienta, salida estructurada y tokens de entrada/salida, y rehacer `ANALISIS_ECONOMICO.md` calculando el costo desde esos tokens y proyectándolo a 10.000 tickets mensuales.
