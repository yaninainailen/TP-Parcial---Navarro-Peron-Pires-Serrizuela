# A6 — Resultado final — Caso excelente

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Mensaje congelado: SHA-256 `a3068e37867aabf8d0d6da3e0b79b95d1e1a084b9e7845d415be4ebf4dcdcdfd`.
- Ejecución única; la respuesta se conserva completa y sin correcciones manuales.
- La interfaz no expuso modelo, temperatura, seed ni otros parámetros; no se infieren.

## Respuesta completa

```text
Corrección — Caso-Ej-1
Puntaje por dimensión
Dimensión	Nivel asignado	Puntos	Evidencia citada
Sistema completo y funcionando	Insuficiente	10/30	system_prompt.md define el contrato y GOBIERNO.md define L0–L4, pero no hay herramienta/conector real usado en las corridas. Además, corrida_1.md afirma "ya lo estamos consultando con logística" y "te confirmamos el estado en las próximas 24hs", acciones/plazos que no están respaldados por ninguna herramienta o evidencia del repo; en corrida_3.md vuelve a afirmar "Iniciamos tu cambio de talle". La afirmación positiva material se repite en más de una salida.
Proceso documentado	Excelente	25/25	DECISIONES.md documenta al menos dos iteraciones concretas: el cambio de JSON a tabla markdown y la incorporación de la regla de derivación. También reconoce la falla del ticket 104 de corrida_2.md y explica la mitigación aplicada.
Formato y reproducibilidad	Excelente	15/15	Están presentes README.md, prompts/, corridas/ y DECISIONES.md. corrida_1.md, corrida_2.md y corrida_3.md tienen fecha, entrada completa y salida estructurada, con los CSV correspondientes presentes.
Análisis económico	Excelente	15/15	ANALISIS_ECONOMICO.md calcula el costo usando los tokens reales de las tres corridas (620/210, 540/160 y 510/150), proyecta el costo a 50 tickets diarios y 365 días, y justifica usar el modelo mini con el criterio "el modelo más chico que hace bien la tarea".
Gobierno y riesgo	Excelente	15/15	GOBIERNO.md especifica acceso de lectura a las planillas y ausencia de acceso a mail, pagos y base de clientes; detalla riesgos y mitigaciones, revisión obligatoria y muestral, y niveles L0–L4. En L4 identifica al responsable de atención al cliente como quien firma la tanda.
Puntaje total: 80/100
Justificación por dimensión

Sistema completo y funcionando: El contrato está bien definido en system_prompt.md, y GOBIERNO.md establece supervisión L0–L4. Sin embargo, no aparece ninguna herramienta o conector real utilizado en las tres corridas: solo se muestran CSV y respuestas. Además, las respuestas de corrida_1.md y corrida_3.md presentan acciones ejecutadas o compromisos no respaldados ("ya lo estamos consultando con logística", "Iniciamos tu cambio de talle"). Al repetirse, la regla de grounding limita la dimensión a Insuficiente.

Proceso documentado: DECISIONES.md contiene dos iteraciones verificables con cambios concretos: JSON → tabla markdown y una regla explícita de derivación. También reconoce la falla del ticket 104 en corrida_2.md y documenta que la solución quedó como mitigación parcial.

Formato y reproducibilidad: La estructura requerida está completa y las tres corridas son reconstruibles: cada corrida_N.md incluye fecha, entrada y salida, y cada CSV de entrada existe por separado. Las tres salidas mantienen la misma tabla estructurada.

Análisis económico: ANALISIS_ECONOMICO.md utiliza los tokens registrados en las tres corridas y calcula aproximadamente USD 0,00036 por corrida y USD 0,0001 por ticket. También incluye proyección diaria/semanal/anual y justifica la elección del modelo bajo el criterio de usar el modelo más chico que resuelve correctamente la tarea.

Gobierno y riesgo: GOBIERNO.md identifica permisos y sistemas involucrados, riesgos concretos, mitigaciones, revisión humana obligatoria y muestral, y la firma responsable en L4. La supervisión está suficientemente operacionalizada para cumplir el nivel Excelente.

Señales de alerta

No se cumplen las condiciones obligatorias para activar la alerta de posible trabajo tramposo: la regla anti-trampa afecta el nivel de una sola dimensión y no hay una dimensión puntuada como Insuficiente o Ausente por la regla Nº4 sobre números económicamente contradictorios. Sí queda registrada, como inconsistencia de grounding que afecta D1, la presentación de acciones/plazos no respaldados en corrida_1.md y corrida_3.md.

Sugerencia concreta de mejora

Eliminar del contrato y de las respuestas cualquier afirmación de acciones ejecutadas o plazos que el agente no pueda verificar, o incorporar una herramienta real de logística/cambios y mostrar su uso en las corridas; por ejemplo, reemplazar "ya lo estamos consultando con logística" por un borrador que indique que el caso será revisado por una persona.
```
