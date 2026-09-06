# A4 — Resultado completo DESPUÉS: caso A, archivo ausente

## Condiciones de ejecución

- Fecha: 2026-09-06.
- Único cambio respecto de la línea base: regla de estados de acceso en
  `agente/system_prompt.md`.
- `rubrica.md`, `agente/sonda_v0.2.html` y fixture A: sin cambios.
- Evaluador: ChatGPT anónimo en `chatgpt.com`.
- Conversación: <https://chatgpt.com/uc/6a9d84ea-3194-83ea-a412-856c9eb6def9>.
- Longitud del mensaje: 17.078 caracteres.
- SHA-256 del mensaje: `ec102176dccd9a2c81970de4bad8b48c48dcf30610ac13ec97d8b2a546022f06`.
- Modelo, temperatura, seed y demás parámetros: no expuestos por la interfaz anónima; no se
  infirieron.
- SHA-256 de la transcripción: `d95140fdd2a9ffaf055d155582266a658907cd3043471e60b44727f08bd56f9a`.

## Respuesta completa

```text
Corrección — A4-caso-A-ausente
Puntaje por dimensión
Dimensión	Nivel asignado	Puntos	Evidencia citada
Sistema completo y funcionando	Ausente	0/30	El repo tiene 0 archivos detectados; no existen system_prompt.md, user_prompt.md ni corridas verificables.
Proceso documentado	Ausente	0/25	No existe DECISIONES.md: la estructura reporta 0 archivos.
Formato y reproducibilidad	Ausente	0/15	No existen README.md, prompts/, corridas/ ni DECISIONES.md; el repo tiene 0 archivos detectados.
Análisis económico	Ausente	0/15	No hay archivos ni evidencia de análisis económico.
Gobierno y riesgo	Ausente	0/15	No hay archivos ni evidencia de gobierno, riesgos, permisos, revisión humana o firma.
Puntaje total: 0/100
Justificación por dimensión

Sistema completo y funcionando: No hay archivos en el repositorio (0 archivos detectados). Por lo tanto, no puede verificarse ningún agente, prompts, herramienta, corridas ni supervisión humana.

Proceso documentado: DECISIONES.md está ausente porque la estructura contiene 0 archivos. No existe evidencia verificable de iteraciones ni de fallas reconocidas.

Formato y reproducibilidad: La estructura obligatoria no está presente: no hay README.md, prompts/, corridas/ ni DECISIONES.md. Tampoco existen las 3 corridas requeridas.

Análisis económico: No hay ningún archivo o dato verificable que permita evaluar costos por corrida, proyección a escala o elección de modelo.

Gobierno y riesgo: No hay evidencia de sistemas o datos tocados, permisos, manejo de errores, revisión humana ni responsable que firme el resultado.

Señales de alerta

No corresponde disparar la alerta de posible trabajo tramposo: no hay discrepancias internas ni números inventados verificables; simplemente no se detectaron archivos.

Sugerencia concreta de mejora

Subir al repositorio la estructura mínima obligatoria (README.md, prompts/system_prompt.md, prompts/user_prompt.md, corridas/ con 3 corridas completas y DECISIONES.md) antes de volver a solicitar la evaluación.
```
