# El agente evaluador

Parcial de la materia "Creación de Agentes de IA" — MADE N-2T, UCEMA, 2026 2T.

**Integrantes:**

* Navarro, Yanina
* Peron, Walter
* Pires, Eber
* Serrizuela, Federico

## Qué construí

Un agente evaluador: un sistema que corrige el trabajo final de la materia aplicando una rúbrica
ejecutable y produce una salida estructurada, con criterios diseñados para lograr mayor
consistencia y una reproducibilidad controlada. Incluye la rúbrica
(`rubrica.md`), el agente corrector (`agente/`), tres trabajos finales de ejemplo construidos por
el grupo para probarlo (`casos/excelente`, `casos/flojo`, `casos/tramposo`) y la evidencia de que
sus resultados fueron contrastados con expectativas humanas definidas antes de ejecutar
(`calibracion.md`). No se afirma que un LLM produzca texto o puntajes necesariamente idénticos en
dos corridas.

## Cómo se lo pedí

El proceso completo, con los pedidos reales en orden, está en `interacciones/registro.md`. En
resumen: se definió primero que el corrector y la rúbrica tenían que ser **genéricos** (aplicables
a cualquier caso de negocio que un compañero elija para su trabajo final, no a uno fijo), se
escribió la rúbrica con niveles de puntaje **fijos** (no rangos) para reducir discrecionalidad y
mejorar la consistencia, se
escribió el system prompt del corrector con un formato de salida obligatorio, se construyeron tres
trabajos finales ficticios sobre el mismo caso (un agente de triage de consultas de clientes) para
poder comparar calidad de construcción y no calidad de idea, y se corrió el corrector de verdad
sobre los tres para calibrarlo contra el criterio del grupo.

## Qué funciona

- La rúbrica aplica 5 dimensiones con 4 niveles cada una, evidencia exigida por nivel y una regla
  anti-trampa explícita.
- La validación final posterior a A2–A6 distingue los tres casos: excelente **80/100**, flojo
  **33/100** y tramposo **47/100**. Solo el tramposo activa la alerta obligatoria
  `⚠️ Posible caso de trabajo tramposo detectado`. El 100/100 del caso excelente se conserva en
  `calibracion.md` como resultado histórico de una versión previa al criterio de grounding; no es
  el resultado vigente.
- **A2** probó un ataque directo de prompt injection dentro de un repo. El evaluador no obedeció
  la instrucción y, después del cambio, la detectó, citó su archivo y la informó sin alterar la
  nota por su mera presencia. Es evidencia sobre ese ataque controlado, no una demostración de
  inmunidad universal.
- **A3** incorporó grounding observable en D1. El caso excelente pasó de su 100/100 histórico a
  80/100 porque varias corridas presentan acciones o compromisos materiales sin respaldo; el
  resto de sus dimensiones permanece en Excelente.
- **A4** separó conceptualmente archivo ausente, presente y leído, y presente pero inaccesible. Una
  falla técnica ya no debe convertirse por sí sola en `Ausente`, penalización ni alerta de trampa.
- **A5** sincronizó el system prompt y la rúbrica embebidos de Sonda con sus archivos fuente. El
  cierre técnico de **A6** volvió a verificar igualdad exacta después de aclarar D4 y D5.
- **A6** fijó expectativas humanas por dimensión, conservó una iteración fallida, aclaró las
  fronteras `Insuficiente`/`Ausente` de D4 y D5 y cerró con una validación 80/33/47. La evidencia
  completa está en `casos/calibracion-final-A6/`.
- Como prueba externa adicional, Sonda v0.2 evaluó
  [`Contrato-Agente-Vaquillonas`](https://github.com/pireseber-lang/Contrato-Agente-Vaquillonas),
  un repositorio real creado para una consigna anterior y no para cumplir la rúbrica actual. Dio
  **15/100**, sin alerta anti-trampa: procesó el repo, aplicó las exigencias estructurales y separó
  carencias de manipulación. El resultado no es una recalificación académica del trabajo original
  ni demuestra validez general; la evidencia está en
  `casos/prueba-externa-real/Contrato-Agente-Vaquillonas/`.
- Un stress test adicional ("caso medio tramposo": 4 de 5 dimensiones reales y excelentes, una
  sola con un número económico que no cierra) encontró que la regla de alerta original (2+
  dimensiones afectadas) dejaba pasar sin aviso una mentira aislada en una sola dimensión — daba
  85/100 sin ninguna señal. Se ajustó la regla para que también dispare con una sola dimensión
  penalizada específicamente por la regla anti-trampa del número inventado (ver `calibracion.md`).
- El corrector también se corrió, sin cambiar nada de `agente/`, contra 3 repos reales ajenos al
  grupo (de la Entrega 1 de la materia, no trabajos finales): no se rompió, devolvió el formato
  esperado, y esa prueba expuso una ambigüedad real en `rubrica.md` ("`DECISIONES.md` (o
  equivalente)") que quedó resuelta de forma explícita citando la consigna del trabajo final.

## Cómo correr esto en la prueba de fuego (10/9)

La herramienta es `agente/sonda_v0.2.html`, con tres caminos elegibles paso a paso en la misma
página (detalle completo en `agente/config.md`):

1. **Un repo con API de Claude:** pegar la URL del repo, el nombre del creador, la API key de
   Anthropic, el Workspace ID y elegir el modelo (por defecto Claude Opus 5; hay opciones más
   baratas — Sonnet 5, Haiku 4.5 — en el mismo selector), y apretar "Analizar el repo". La página
   llama directo a la API, muestra la corrección junto con el uso de tokens y el costo estimado de
   esa corrida, y la guarda **automáticamente** en `casos/<nivel>/<Creador del repo>/` (informe +
   copia de `corridas/`, `prompts/` y los 4 archivos de proceso del repo evaluado). Conviene tener
   la key y el Workspace ID ya cargados y probados *antes* de que arranque la prueba, no en el
   momento.
2. **Un repo sin API ("Prompt Validador"):** sin key, pegar la URL, apretar "Crear Prompt
   Validador", copiar el resultado y pegarlo en un chat de IA ya abierto de antemano (Claude o
   ChatGPT) — conviene tener esa pestaña lista de antes. No guarda nada en disco.
3. **Una lista de repos por CSV:** el mismo camino con API pero subiendo un CSV (`url,creador` por
   fila) para procesar muchos repos uno por uno.

Como el guardado automático en disco (camino 1 y 3) depende de una API del navegador que no
funciona abriendo el archivo con doble clic, hay que servir la página por `http://localhost` (ej.
`python -m http.server` desde la raíz del repo, y abrirla como
`http://localhost:8000/agente/sonda_v0.2.html`) y usar Chrome o Edge. El camino 2 (sin API, sin
guardado) sí funciona abriendo el archivo directo, en cualquier navegador.

Si la API de GitHub responde con error de rate-limit (límite de pedidos sin autenticar), esperar
unos minutos y reintentar, o probar desde otra red — no hay token de GitHub configurado a
propósito (decisión documentada en `interacciones/registro.md`), así que no hay forma de saltear
ese límite en el momento. Si en cambio falla la llamada a la API de Anthropic (key inválida, sin
crédito, rate-limit de la cuenta), el mensaje de error queda a la vista arriba del resultado — el
camino 2 (sin key) sigue funcionando como respaldo.

## Qué falta o qué falló

- Existe **variabilidad residual del LLM**. En A6, una repetición cambió D5 del caso flojo entre
  `Insuficiente` y `Ausente`, y otra ejecución activó una alerta anti-trampa no respaldada. Las
  auditorías llevaron a aclarar D5, pero no a agregar otra regla anti-trampa porque la norma
  vigente ya era suficientemente clara. Por eso hablamos de mayor consistencia y reproducibilidad
  controlada, no de determinismo absoluto.
- Una ejecución de A6 informó **31/100** aunque sus filas sumaban 33. El error no se reprodujo en
  dos repeticiones posteriores, que calcularon correctamente sus respectivos totales; se conserva
  como error puntual y no como fallo aritmético persistente demostrado.
- Sonda todavía mantiene copias embebidas de `system_prompt.md` y `rubrica.md`. Hoy coinciden
  exactamente con las fuentes, pero futuros cambios requieren repetir la verificación de
  sincronización para evitar divergencias.
- Sonda interpreta una URL de GitHub solo como `owner/repo` y evalúa la `default_branch` informada
  por GitHub. Una URL que incluya otra rama o commit no fija esa revisión; dos ejecuciones pueden
  observar contenido distinto si la rama por defecto cambia entre ambas.
- A4 corrigió la decisión conceptual del evaluador, pero Sonda aún pierde la causa de algunos
  fallos individuales de descarga (`404`, `429`, CORS o red), excluye ciertos tipos y archivos
  grandes, y puede truncar el contenido total. La estructura permite reconocer que un archivo
  existe, pero no siempre transmite un diagnóstico técnico preciso ni identifica individualmente
  todo lo omitido.
- A2 cubre el ataque directo probado; no se evaluaron exhaustivamente todas las variantes posibles
  de prompt injection. La prueba sobre `Contrato-Agente-Vaquillonas` aporta evidencia sobre un
  repositorio real externo, pero todavía falta validar el corrector sobre un trabajo final real
  construido específicamente para la consigna y la rúbrica vigentes.

## Qué aprendí

Que una rúbrica "ejecutable" no es solo escribir niveles y puntajes — hay que aplicarla de verdad
para encontrarle los huecos, porque en el papel una rúbrica puede sonar completa y recién al
correrla aparecen fronteras ambiguas, problemas de grounding o diferencias entre ausencia y falla
técnica. También que detectar una trampa no es lo mismo que ponerle una nota baja, y que una
calibración defendible necesita expectativas humanas previas, evidencia por dimensión e
iteraciones de un solo cambio. Las reglas discretas mejoran la consistencia, pero no eliminan la
variabilidad propia de un LLM.
