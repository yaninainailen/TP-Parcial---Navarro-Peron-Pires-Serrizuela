# Prueba externa real — Contrato-Agente-Vaquillonas

## Contexto y objetivo

Esta prueba documenta una ejecución del agente evaluador actual sobre un repositorio real externo
a los fixtures controlados `excelente`, `flojo` y `tramposo` usados durante la calibración.

El repositorio evaluado corresponde a un trabajo anterior de la materia “Creación de Agentes de
IA”, realizado antes del trabajo final y antes de que existiera la rúbrica actual. Por lo tanto,
el resultado **no es una recalificación académica** ni debe reinterpretarse como la nota original
de ese trabajo. El objetivo es observar si Sonda puede procesar un repositorio real no construido
para satisfacer la rúbrica vigente y producir una corrección trazable.

## Repositorio evaluado

- Repositorio: <https://github.com/pireseber-lang/Contrato-Agente-Vaquillonas>
- Condición: repositorio real externo al conjunto controlado de calibración.
- Consigna de origen: anterior al trabajo final y distinta de la que exige la rúbrica vigente.

## Modo de ejecución

- Sonda: v0.2.
- Camino: un solo repositorio de GitHub.
- Modalidad: `Sin API · Prompt Validador`.
- Procedimiento: Sonda generó el prompt validador y este se ejecutó en un LLM externo.
- Modelo, temperatura, seed y otros parámetros: no informados; no se infieren.

## Resultado resumido

| Dimensión | Nivel | Puntaje |
|---|---|---:|
| D1 · Sistema completo y funcionando | Insuficiente | 10/30 |
| D2 · Proceso documentado | Ausente | 0/25 |
| D3 · Formato y reproducibilidad | Insuficiente | 5/15 |
| D4 · Análisis económico | Ausente | 0/15 |
| D5 · Gobierno y riesgo | Ausente | 0/15 |
| **Total** |  | **15/100** |

- Alerta `⚠️ Posible caso de trabajo tramposo detectado`: **No**.
- Respuesta íntegra, sin correcciones manuales:
  `resultado_completo.md`.

## Observaciones sobre el comportamiento

1. **Sonda logró detectar y evaluar el repositorio.** El informe identificó nueve archivos,
   reconoció `system_prompt.md`, `user_prompt.md`, el README, los prompts y las salidas disponibles,
   y produjo las cinco dimensiones, total, señales y sugerencia exigidos.
2. **Aplicó las exigencias estructurales de la rúbrica.** D2 quedó en Ausente porque el proceso
   estaba en `README.md` y no en un `DECISIONES.md` dedicado. D3 señaló la ausencia de las carpetas
   obligatorias `prompts/` y `corridas/`, además de fechas de corrida.
3. **No confundió desajuste de consigna con manipulación.** Las carencias observadas son esperables
   porque el trabajo fue creado para una consigna anterior. El informe las trató como diferencias
   estructurales y de evidencia, no como prueba de trampa.
4. **No generó una alerta anti-trampa injustificada.** La sección de señales declaró que no había
   números deliberadamente inventados ni intento de manipular al evaluador.
5. **La sugerencia de mejora es coherente con el diagnóstico.** Propuso reorganizar prompts y
   corridas, agregar fechas y trasladar las iteraciones a `DECISIONES.md`, sin exigir rehacer el
   agente desde cero.
6. **La nota tiene alcance limitado.** El 15/100 expresa el ajuste de este repositorio a la rúbrica
   actual, no la calidad o calificación académica del trabajo bajo su consigna original.

## Diagnóstico

La ejecución aporta evidencia adicional de funcionamiento sobre un repositorio real externo al
conjunto controlado: Sonda procesó una estructura no preparada para la rúbrica, aplicó los
descriptores, mantuvo la salida esperada y separó incumplimientos estructurales de manipulación.

Una sola prueba no demuestra validez general, robustez total, repetibilidad estadística ni buen
desempeño sobre cualquier arquitectura o repositorio. Tampoco sustituye una validación sobre un
trabajo final real creado bajo la misma consigna que la rúbrica vigente.

## Recomendación documental posterior

Esta prueba permite actualizar parcialmente la limitación que decía que el corrector no había sido
probado contra un trabajo real externo a los fixtures. Ya existe una ejecución sobre un trabajo
real, pero de una consigna anterior; por eso no corresponde eliminar toda la limitación.

Texto recomendado para `README.md`:

> El evaluador fue ejecutado también sobre un repositorio real externo a los fixtures controlados:
> `Contrato-Agente-Vaquillonas`, creado para una consigna anterior. Sonda pudo procesarlo, aplicar
> estrictamente la estructura vigente y distinguir carencias de una alerta de trampa. Esta prueba
> aporta evidencia adicional de funcionamiento, pero no equivale a validar el evaluador sobre un
> trabajo final real producido específicamente para la rúbrica actual.

Texto recomendado para `calibracion.md`:

> Como prueba externa adicional, Sonda v0.2 se ejecutó en modo Prompt Validador sobre
> `Contrato-Agente-Vaquillonas`. El resultado fue 15/100, sin alerta anti-trampa. El puntaje no se
> interpreta como recalificación del trabajo original, porque el repositorio respondía a una
> consigna anterior; el hallazgo relevante es que el evaluador procesó un repo real externo,
> aplicó las exigencias estructurales y separó falta de adecuación de manipulación. Evidencia:
> `casos/prueba-externa-real/Contrato-Agente-Vaquillonas/`.
