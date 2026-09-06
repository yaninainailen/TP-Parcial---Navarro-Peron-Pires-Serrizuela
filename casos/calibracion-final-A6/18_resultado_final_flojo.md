# A6 — Resultado final — Caso flojo

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Mensaje congelado: SHA-256 `b7c4b0d981cc1c6b51e0ad0f3e3d8b2bdf6a0def78e8f491b6b5fdeecd14797b`.
- Ejecución única; la respuesta se conserva completa y sin correcciones manuales.
- La interfaz no expuso modelo, temperatura, seed ni otros parámetros; no se infieren.

## Respuesta completa

```text
Corrección — Caso-Ej-2
Puntaje por dimensión
Dimensión	Nivel asignado	Puntos	Evidencia citada
Sistema completo y funcionando	Insuficiente	10/30	system_prompt.md y user_prompt.md existen, pero no hay herramienta o conector real. Además, corrida_1.md devuelve “Categoría: Consulta de producto...” mientras corrida_2.md usa un formato narrativo distinto.
Proceso documentado	Insuficiente	8/25	DECISIONES.md solo dice: “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” No documenta iteraciones concretas ni una falla real con antes/después.
Formato y reproducibilidad	Insuficiente	5/15	Existen README.md, prompts/, corridas/ y DECISIONES.md, pero solo hay 2 corridas y corrida_1.md no tiene fecha. Se combinan dos huecos menores.
Análisis económico	Insuficiente	5/15	README.md afirma “El modelo no es caro así que se puede usar tranquilamente para esto”, pero no aporta costos, tokens, proyección ni justificación del modelo.
Gobierno y riesgo	Insuficiente	5/15	corrida_2.md menciona: “Habría que avisarle a un humano antes de mandar nada, supongo.” Es una consideración de supervisión humana, pero no define permisos, riesgos, manejo concreto de errores ni responsable de firma.
Puntaje total: 33/100
Justificación por dimensión

Sistema completo y funcionando: system_prompt.md define un agente para clasificar y responder consultas, y user_prompt.md establece la entrada. Sin embargo, no existe ninguna herramienta o conector verificable y las dos corridas no mantienen un formato de salida estructurado consistente: la primera usa “Categoría...” y la segunda una respuesta narrativa. Tampoco hay supervisión L0–L4.

Proceso documentado: DECISIONES.md contiene únicamente la afirmación “Probé el prompt varias veces hasta que anduvo más o menos bien”, sin describir qué se probó, qué falló ni qué se modificó. Tampoco reconoce una falla real; por lo tanto no alcanza los requisitos de Bueno o Excelente.

Formato y reproducibilidad: La estructura obligatoria básica está presente, pero corridas/ contiene solamente corrida_1.md y corrida_2.md, en lugar de 3 corridas. Además, corrida_1.md no tiene fecha, mientras corrida_2.md sí está fechada como 2026-08-28. Esto impide reconstruir el historial completo sin adivinar.

Análisis económico: README.md solo afirma que “El modelo no es caro”. No hay tokens de entrada/salida de corridas reales, costo por corrida, proyección a escala ni una elección explícita basada en “el modelo más chico que hace bien la tarea”.

Gobierno y riesgo: La única evidencia es la frase de corrida_2.md “Habría que avisarle a un humano antes de mandar nada, supongo.” Esto constituye una consideración genérica de supervisión, pero no especifica qué revisa el humano, qué sistemas o datos puede tocar el agente, qué permisos tiene, cómo se manejan los errores ni quién firma el resultado.

Señales de alerta
Sugerencia concreta de mejora

Agregar una tercera corrida fechada y hacer que las tres usen un único formato estructurado; luego documentar en DECISIONES.md al menos dos iteraciones con ejemplos concretos de antes/después, una falla real y su corrección, junto con una herramienta verificable y niveles L0–L4 de supervisión.
```
