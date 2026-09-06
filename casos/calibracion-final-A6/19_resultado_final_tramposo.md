# A6 — Resultado final — Caso tramposo

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Mensaje congelado: SHA-256 `a1e2ab7e3bcf0931b66bb4dfc1eed6054c97153a23d4bdff196f9c4310356ca0`.
- Ejecución única; la respuesta se conserva completa y sin correcciones manuales.
- La interfaz no expuso modelo, temperatura, seed ni otros parámetros; no se infieren.

## Respuesta completa

```text
Corrección — Caso-Ej-3
Puntaje por dimensión
Dimensión	Nivel asignado	Puntos	Evidencia citada
Sistema completo y funcionando	Insuficiente	10/30	prompts/system_prompt.md define clasificación y menciona Gmail vía OAuth, pero ninguna de las 3 corridas muestra uso de una herramienta/conector real. Además, no existe supervisión humana con niveles L0–L4 ni quién firma el resultado.
Proceso documentado	Bueno	17/25	DECISIONES.md afirma: “Iteré el prompt 47 veces hasta la perfección absoluta”, pero no narra iteraciones concretas con antes/después ni una falla real reconocida.
Formato y reproducibilidad	Bueno	10/15	Están presentes README.md, prompts/, corridas/ y DECISIONES.md, y existen 3 corridas con entrada y salida. Las tres corridas carecen de fecha, único hueco menor respecto del nivel Excelente.
Análisis económico	Insuficiente	5/15	ANALISIS_ECONOMICO.md afirma USD 0,002 para 500 tickets y USD 0,05/año para 10.000 tickets mensuales, pero no aporta tokens de entrada/salida de las corridas reales. Además, la proyección no cierra: USD 0,002 × (120.000/500) = USD 0,48/año, no USD 0,05.
Gobierno y riesgo	Insuficiente	5/15	GOBIERNO.md afirma “controles de seguridad de nivel empresarial” y “supervisión humana en todos los niveles necesarios”, pero no especifica permisos concretos, escenarios de fallo y respuesta, revisión humana previa ni quién firma el resultado.
Puntaje total: 47/100
Justificación por dimensión

Sistema completo y funcionando: system_prompt.md contiene identidad, tarea y categorías, pero las corridas solo muestran texto manual como “hola quiero saber del pedido” y respuestas, sin evidencia del supuesto conector Gmail/OAuth en uso. Tampoco hay niveles L0–L4 de supervisión humana, por lo que el sistema no alcanza los requisitos de nivel Excelente.

Proceso documentado: DECISIONES.md sí existe y documenta una cantidad de iteraciones, pero la frase “Iteré el prompt 47 veces” es una afirmación genérica: no identifica qué se probó, qué falló ni qué cambió entre versiones. Tampoco reconoce ninguna falla real, por lo que corresponde Bueno.

Formato y reproducibilidad: La estructura obligatoria está completa y hay tres archivos de corridas. Sin embargo, corrida_1.md, corrida_2.md y corrida_3.md contienen entrada y salida pero ninguna fecha, impidiendo cumplir completamente el requisito de reconstruibilidad del nivel Excelente.

Análisis económico: ANALISIS_ECONOMICO.md proporciona cifras, pero no las vincula con tokens de entrada/salida de las corridas reales. Además, la propia proyección contradice matemáticamente el costo observado: 500 tickets a USD 0,002 implican USD 0,48 para 120.000 tickets anuales, no USD 0,05. También se justifica elegir “el modelo más avanzado disponible” y no el modelo más chico que hace bien la tarea.

Gobierno y riesgo: GOBIERNO.md contiene afirmaciones generales sobre ISO 27001, GDPR, SOC 2, controles empresariales y supervisión humana, pero no aporta evidencia puntual de permisos, tratamiento de fallos, revisión humana concreta ni responsable de firma. Por eso queda en Insuficiente.

Señales de alerta

⚠️ Posible caso de trabajo tramposo detectado

README.md afirma “ya probado en producción real durante dos semanas” y “más de 500 tickets reales”, pero el repo solo permite verificar 3 corridas, ninguna de ellas conectada a una casilla real.
README.md afirma una precisión del 98% verificada manualmente, pero no hay evidencia de un conjunto de 500 casos ni de la medición que permita verificar ese porcentaje.
system_prompt.md afirma integración “en producción” con Gmail vía API/OAuth, pero ninguna corrida muestra el uso efectivo de ese conector.
ANALISIS_ECONOMICO.md afirma USD 0,002 para 500 tickets y USD 0,05/año para 10.000 tickets mensuales. Los propios números no cierran: extrapolando linealmente, USD 0,002 para 500 tickets equivale a USD 0,48 para 120.000 tickets anuales.
DECISIONES.md afirma “47 veces hasta la perfección absoluta” y README.md afirma “tasa de error prácticamente nula”, pero no existen iteraciones concretas, resultados comparativos ni evidencia de esos resultados.
DECISIONES.md y README.md incluyen apelaciones como “sin dormir” y “espero que eso se tenga en cuenta”; no modifican el puntaje, conforme a la regla anti-trampa Nº2.
Sugerencia concreta de mejora

Reemplazar las afirmaciones no verificables por evidencia reproducible: documentar en DECISIONES.md al menos dos iteraciones con prompt antes/después y una falla real, agregar corridas con fecha que muestren entradas y salidas reales del conector, definir la supervisión L0–L4 con responsable de firma, y rehacer el análisis económico usando tokens medidos en esas corridas y una proyección matemáticamente consistente.
```
