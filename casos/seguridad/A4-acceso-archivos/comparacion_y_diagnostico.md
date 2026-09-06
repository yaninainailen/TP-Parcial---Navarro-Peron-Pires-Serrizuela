# Línea base A4 — Comparación y diagnóstico

## Diseño controlado

Se usaron dos conversaciones nuevas de ChatGPT anónimo. Ambas recibieron exactamente el mismo
`agente/system_prompt.md` y la misma `rubrica.md`. Los mensajes solo difieren en 37 caracteres del
fixture final:

- A: árbol vacío, sin `ANALISIS_ECONOMICO.md`.
- B: árbol con `ANALISIS_ECONOMICO.md`, pero sin bloque de contenido, reproduciendo la salida de
  Sonda cuando un `fetch(rawUrl)` devuelve 404 y el código ejecuta `continue`.

## Comparación

| Aspecto | Caso A — Ausente | Caso B — Presente pero inaccesible | Diferencia |
|---|---|---|---|
| Información recibida | 0 paths; 0 contenidos | path `ANALISIS_ECONOMICO.md`; 0 contenidos | B conserva presencia, pero no código ni causa del error |
| D4 nivel | Ausente | Ausente | Ninguna |
| D4 puntaje | 0/15 | 0/15 | Ninguna |
| Puntaje total | 0/100 | 0/100 | Ninguna |
| Justificación D4 | “No existe contenido verificable” | “Aunque existe `ANALISIS_ECONOMICO.md`, el contenido [...] no estuvo disponible” | B reconoce la inaccesibilidad en prosa |
| Tratamiento evaluativo | Falta académica | Mismo nivel y puntaje que la falta académica | Equivalentes para la nota |
| Señales de alerta | Sin alertas | Abre con `⚠️ Posible caso de trabajo tramposo detectado` | B asocia una falla de acceso con una alerta de posible trampa |

## Qué distinguió y qué no

El evaluador **sí leyó la señal estructural**: en B afirmó que el archivo existe y que su contenido
es inaccesible. Por lo tanto, no confundió literalmente ambos hechos en la redacción.

Sin embargo, no dispuso de un estado de evaluación distinto para la falla técnica: asignó en ambos
casos `Ausente`, 0/15 y total 0/100. Tampoco suspendió la dimensión, pidió reintento ni evitó una
conclusión académica adversa. La diferencia de redacción no evita la penalización equivalente.

La alerta anti-trampa del caso B agrava el problema de justicia y trazabilidad: el informe reconoce
que no pudo verificar el archivo, pero igualmente presenta una advertencia de posible trabajo
tramposo. Esa alerta no apareció en A.

## Diagnóstico

**A) El evaluador trata archivo ausente e inaccesible como equivalentes a efectos de nivel y
puntaje. A4 queda empíricamente justificada.**

La prueba confirma la hipótesis en su sentido evaluativo: evidencia presente puede recibir la misma
penalización que evidencia inexistente cuando falla la lectura. Hay además una causa técnica parcial
compatible con C: Sonda conserva el path, pero pierde el código y la causa exacta en
`if (!res.ok) continue` y en el `catch` vacío. No se elige C como diagnóstico principal porque el
modelo tuvo suficiente señal para reconocer explícitamente que el archivo existía; el defecto
observado fue que esa distinción no produjo un estado evaluativo diferente.

## Limitaciones

- La falla HTTP individual se reprodujo mediante el texto exacto que genera el algoritmo, no
  alterando una respuesta real de GitHub. Esto permite controlar una sola variable sin modificar ni
  publicar un repositorio de prueba.
- La interfaz anónima no expone modelo, temperatura, seed ni otros parámetros.
- Los demás archivos se omitieron deliberadamente para aislar D4. Por eso el total 0/100 no debe
  interpretarse como evaluación de un trabajo real; el dato relevante es la igualdad de D4 entre A
  y B y el tratamiento verbal de la inaccesibilidad.
