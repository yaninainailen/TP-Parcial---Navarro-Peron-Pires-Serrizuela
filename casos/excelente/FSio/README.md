# TP Final — Agente Asistente para Ingenieros de Ventas en Redes Ópticas

Trabajo final de **Programación de y con Agentes de IA** · MBA UCEMA · 2026 2T · Prof. Alfredo B. Roisenzvit

**Autor:** Federico Serrizuela

## Dónde está cada requisito

Esta tabla existe porque la corrección la hace un agente. Cada fila apunta al artefacto que cubre esa dimensión de la rúbrica.

| Dimensión de la rúbrica | Peso | Dónde |
| --- | --- | --- |
| Sistema completo y funcionando | 30 | Contrato: [`prompts/system_prompt.md`](prompts/system_prompt.md) (las 6 piezas, ver mapa abajo) + [`prompts/user_prompt.md`](prompts/user_prompt.md) (contrato de entrada). Herramienta real: [`scripts/run_agent.py`](scripts/run_agent.py). Output estructurado: sección "Formato" del system prompt. Supervisión L0–L4: sección "Supervisión y niveles de autonomía" del system prompt, detallada en [`GOBIERNO.md`](GOBIERNO.md) |
| Proceso documentado | 25 | [`DECISIONES.md`](DECISIONES.md) (consolidado) + [`corridas/`](corridas/) (iteración por iteración, con los errores textuales) |
| Formato y reproducibilidad | 15 | Estructura obligatoria en la raíz + [`corridas/README.md`](corridas/README.md) (cómo reconstruir cada corrida) + [`corridas/raw/`](corridas/raw/) (6 salidas completas sin editar) |
| Análisis económico | 15 | [`ANALISIS_ECONOMICO.md`](ANALISIS_ECONOMICO.md) — costo real por corrida, proyección semanal/anual, elección de modelo justificada con datos |
| Gobierno y riesgo | 15 | [`GOBIERNO.md`](GOBIERNO.md) — sistemas y permisos, modos de falla reales, checklist del SE, quién firma, niveles por fase |

### Las seis piezas del system prompt

Cada pieza es una sección de primer nivel en [`prompts/system_prompt.md`](prompts/system_prompt.md), con el mismo nombre:

| Pieza | Sección | Qué contiene |
| --- | --- | --- |
| 1 · Rol | `# Rol` | Asistente Senior de Optical Network SE, +15 años en redes terrestres de transporte óptico; especialidad en DWDM, packet-optical y ROADM |
| 2 · Contexto | `# Contexto` | **La empresa** (fabricante de equipamiento óptico; Producto/Pricing fija precios, Ingeniería valida diseño), **el público** (el SE es el usuario; el cliente final nunca habla con el agente) y **los datos** que puede recibir |
| 3 · Tarea | `# Tareas` | Frase inequívoca del entregable, seguida del flujo de 6 fases y del chequeo de inconsistencias previo |
| 4 · Restricciones | `# Restricciones` | Veracidad (no asumir, distinguir hechos/supuestos, no inventar precios), producto y proveedores, y forma (tono + techos de extensión por sección). Se complementa con `# Criterios de escalación` |
| 5 · Formato | `# Formato` | Plantilla fija de 11 secciones, con las reglas de omisión |
| 6 · Ejemplos | `# Ejemplos` | 3 pares entrada→salida completos (AURELIA, NORDEX, QUILPO), cada uno enseñando una decisión distinta que suele salir mal |

La sección `# Supervisión y niveles de autonomía` no corresponde a ninguna de las seis piezas: es un agregado propio de este trabajo para cumplir el requisito de supervisión L0–L4 del ítem 1 de la consigna.

## Qué construí

Un agente asistente para un Optical Network Sales Engineer (SE) de soluciones de transporte de datos por fibra óptica (DWDM, packet-optical, ROADM). El agente toma información cruda de un cliente (notas de reunión, correos, requisitos técnicos) y la lleva a través de un flujo de 6 fases: desde el análisis de requisitos hasta la preparación de la reunión de seguimiento con el cliente, pasando por el análisis de brechas, la evaluación de alternativas de solución, la evaluación de si hay información suficiente para cotizar, y el borrador de una nota de ingeniería.

El repositorio incluye:
- [`prompts/system_prompt.md`](prompts/system_prompt.md) — el agente en sí (rol, contexto, las 6 fases, restricciones, criterios de escalación, niveles de autonomía y formato de salida fijo).
- [`prompts/user_prompt.md`](prompts/user_prompt.md) — el contrato de entrada: qué le entrega el SE al agente en cada corrida y en qué forma, más las 3 solicitudes (ACME, SYNNEX, BMINING) usadas como casos de prueba, con la explicación de por qué son casos construidos sobre patrones reales de pedido y no solicitudes reales copiadas.
- [`prompts/casos/`](prompts/casos/) — las mismas 3 solicitudes, una por archivo, más una cuarta (ANDINA LITIO) agregada después para ejercitar la escalación — ver [`DECISIONES.md`](DECISIONES.md).
- [`corridas/`](corridas/) — 3 corridas de desarrollo documentadas, cada una contra uno de los 3 casos exigidos por la consigna, iterando al menos 3 veces sobre errores reales y aplicando la mejora correspondiente al `system_prompt.md`. Empezar por [`corridas/README.md`](corridas/README.md), que explica la diferencia entre el registro de desarrollo y la evidencia de ejecución.
- [`corridas/raw/`](corridas/raw/) — 20 salidas completas sin editar, generadas vía API en tres generaciones del contrato (antes y después de cada cambio), con tokens y costo real de cada una.
- [`DECISIONES.md`](DECISIONES.md) — consolidado de todos los cambios aplicados a los archivos de prompt, con el motivo de cada uno, los cambios de alcance, lo que se descartó y las limitaciones del agente.
- [`scripts/run_agent.py`](scripts/run_agent.py) — el conector/herramienta real: corre el agente vía la API de Anthropic contra un caso de `prompts/casos/` y guarda el output completo en `corridas/raw/`, con los tokens y el costo reales de esa corrida.
- [`ANALISIS_ECONOMICO.md`](ANALISIS_ECONOMICO.md) — costo por corrida, proyección semanal/anual y justificación de la elección de modelo.
- [`GOBIERNO.md`](GOBIERNO.md) — sistemas que toca el agente y con qué permisos, qué puede salir mal y qué pasa cuando sale mal, qué revisa el SE antes de confiar en la salida, quién firma, y los niveles de autonomía (L0–L4) por fase.

## Cómo se lo pedí

Se partió del `system_prompt.md` original (un asistente de SE con 6 fases fijas y un formato de salida estructurado) y de 3 solicitudes de cliente con distinto nivel de completitud y distintos tipos de ambigüedad a propósito. Se corrió el agente contra cada solicitud, se revisó la salida buscando errores reales (no solo estéticos: supuestos disfrazados de hechos, preguntas de descubrimiento genéricas que deberían ser específicas, restricciones mal aplicadas, datos inventados, pedidos del cliente sin lugar en el formato de salida), y por cada error encontrado se aplicó un cambio concreto al `system_prompt.md`, se volvió a correr, y se verificó si el error se resolvía. El detalle iteración por iteración está en `corridas/`; el resumen de qué cambió y por qué está en `DECISIONES.md`.

## Qué hace el agente (resumen del flujo)

1. **Chequeo de inconsistencias** (antes de la Fase 1): revisa la información del cliente en busca de contradicciones internas y las declara, sin resolverlas por su cuenta.
2. **Fase 1 — Análisis de requisitos:** resume objetivos, capacidad, geografía, servicio y plazos.
3. **Fase 2 — Análisis de brechas:** identifica qué información falta (fibra, protección, latencia, requisitos de la aplicación, etc.) y arma preguntas de aclaración.
4. **Fase 3 — Evaluación de la solución:** describe alternativas de arquitectura (y, si el cliente lo pide, alternativas de interconexión con su red existente), con ventajas, limitaciones y riesgos de cada una.
5. **Fase 4 — Preparación para la cotización o estimación:** clasifica la oportunidad como lista / parcialmente lista / no lista, distinguiendo si lo pedido es una cotización formal o una estimación preliminar.
6. **Fase 5 — Nota de ingeniería:** arma un borrador con supuestos, soluciones propuestas, riesgos y próximos pasos.
7. **Fase 6 — Reunión de seguimiento:** prepara objetivo, agenda, mensajes clave y decisiones pendientes del cliente.

El agente ejecuta solo las fases que la información disponible permite, e indica explícitamente dónde se detuvo y por qué — nunca completa una sección con contenido de relleno. Si la oportunidad cae en un criterio de escalación (fibra submarina, rutas transfronterizas, ingeniería fotónica a medida, etc.), lo marca explícitamente en lugar de avanzar con las Fases 3 a 5.

## Cómo correrlo

**Opción real (recomendada) — vía el runner:**

```bash
pip install -r scripts/requirements.txt
export ANTHROPIC_API_KEY=sk-ant-...
python scripts/run_agent.py prompts/casos/solicitud_001_acme.md
```

El output completo, con los tokens y el costo reales de esa corrida, queda guardado en `corridas/raw/`.

**Opción manual (para explorar rápido sin la API):**

1. Usar el contenido de `prompts/system_prompt.md` como system prompt de un modelo de lenguaje.
2. Pasarle como mensaje de usuario la información real de un cliente (se puede usar cualquiera de las 3 solicitudes de `prompts/user_prompt.md` o `prompts/casos/` como ejemplo).
3. Revisar la salida con el formato fijo que define el propio `system_prompt.md` (Resumen de la oportunidad, Inconsistencias detectadas, Requisitos del cliente, Clasificación de la información, Información faltante, Preguntas de aclaración, Alternativas de solución, etc.).

## Qué funciona

- Distingue con claridad hechos, supuestos y recomendaciones gracias a la sección dedicada agregada en la Corrida 01, en vez de mezclar todo en prosa libre.
- No se compromete con mecanismos de protección, esquemas técnicos o precios que el cliente no confirmó — los dos primeros casos de prueba (ACME y SYNNEX) muestran que, tras las mejoras, el agente deja esos puntos correctamente abiertos como preguntas o supuestos en lugar de inventarlos.
- Detecta y declara inconsistencias explícitas dentro del texto del cliente (caso BMINING, canales "IT#1/OT#1" vs. "IT#1/OT#2") en lugar de elegir una versión en silencio.
- Distingue una cotización formal de una estimación presupuestaria preliminar, y refleja la urgencia declarada por el cliente desde el resumen inicial.
- Da un tratamiento propio (arquitectura, ventajas, limitaciones, riesgos) a pedidos específicos del cliente, como alternativas de interconexión con su red existente, en vez de mezclarlos dentro de otra sección.

## Qué falta o qué falló — limitaciones del agente

- **No procesa diagramas ni archivos reales:** un diagrama tiene que transcribirse a texto para que el agente lo use.
- **No tiene acceso a datos reales** de fibra, producto, precios ni inventario de red del cliente — todo BoM se marca "Pendiente de cotización con Producto/Pricing" en vez de inventarse.
- **No hace cálculos de ingeniería reales** (potencia óptica, dispersión); los deja como supuestos de diseño a validar por ingeniería.
- **No es determinístico**, y no solo en la redacción: se midió una variación de costo del 38% entre dos corridas del mismo caso, mismo modelo, contrato casi idéntico (detalle en [`ANALISIS_ECONOMICO.md`](ANALISIS_ECONOMICO.md)).
- **Solo detecta inconsistencias explícitas** dentro del propio texto recibido, no contra un CRM externo.
- **Un modelo alternativo (Haiku 4.5) no generaliza el formato a casos que los ejemplos del contrato no cubren.** No es una limitación del modelo elegido: se probó con un cuarto caso diseñado para disparar la escalación (una rama del contrato que las 3 solicitudes originales no ejercitan), y mientras `claude-sonnet-5` —el modelo del sistema— lo manejó sin ninguna desviación de formato, Haiku 4.5 inventó dos encabezados que no existen en el contrato. Detalle en [`ANALISIS_ECONOMICO.md`](ANALISIS_ECONOMICO.md).
- **No reemplaza el criterio del Sales Engineer:** toda salida requiere revisión humana — ver los niveles de autonomía por fase en [`GOBIERNO.md`](GOBIERNO.md).

El detalle completo de estas limitaciones, junto con los cambios de alcance aplicados y lo que se descartó a propósito, está en [`DECISIONES.md`](DECISIONES.md).

## Qué aprendí

Que la mayoría de los errores reales de este agente no eran de redacción sino de **ausencia de estructura**: la sección "Restricciones" del prompt original ya pedía cosas razonables (distinguir hechos de supuestos, no asumir información faltante, evitar recomendaciones de proveedor) pero, al no tener un lugar fijo en el formato de salida donde aplicarse, el modelo las cumplía a medias o de forma inconsistente entre corridas. Agregar una sección o un campo específico en el Formato de salida resultó, corrida tras corrida, más efectivo que reforzar la misma instrucción en prosa. También aprendí que una restricción "genérica y segura" (como evitar recomendaciones de proveedor) puede volverse un problema cuando el caso de uso cambia — en la Corrida 03 esa misma restricción, pensada para no inventar recomendaciones, terminó haciendo que el agente evitara citar un dato que el propio cliente había dado; hizo falta una excepción explícita, no una regla más estricta.
