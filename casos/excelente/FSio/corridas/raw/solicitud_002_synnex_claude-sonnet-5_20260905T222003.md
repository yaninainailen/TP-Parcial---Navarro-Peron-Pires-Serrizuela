<!--
Caso: solicitud_002_synnex.md
Modelo: claude-sonnet-5
Fecha: 2026-09-05T22:20:03
system_prompt.md (sha256, 12 chars): 76d91e443f52
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 20037
Tokens de salida: 6197
Costo estimado de esta corrida (USD): 0.1531
-->

# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: Estimación presupuestaria preliminar (SYNNEX pide "una primera propuesta presupuestaria"; no solicita una cotización formal cerrada)
Urgencia declarada por el cliente: "en una semana"

SYNNEX Corp busca interconectar sus 2 oficinas principales, separadas por unos 58 km, sobre fibra oscura, con el objetivo específico de replicar sus bases de datos. Pide explícitamente que la solución se construya sobre 2 rutas de fibra independientes y que siga funcionando con una sola ruta ante un corte. El entregable esperado es un número preliminar en un plazo corto, no un proceso completo de cotización.

# Requisitos del cliente
- Interconexión de 2 oficinas principales, distancia aproximada 58 km, sobre fibra oscura.
- Propósito: replicación de bases de datos entre ambas oficinas.
- La solución debe construirse sobre 2 rutas de fibra ópticas independientes.
- Continuidad de servicio con una sola ruta operativa ante el corte de la otra; el cliente no especificó el mecanismo ni el SLA.
- Routers Cisco existentes en ambos extremos.
- Velocidad de las interfaces ópticas de los routers sin definir (1G, 10G o 100G).
- Primera propuesta presupuestaria requerida en una semana.

# Clasificación de la información
Hechos: distancia aproximada de 58 km; fibra oscura como medio; requisito explícito de 2 rutas independientes; propósito de replicación de bases de datos; routers Cisco existentes en ambos extremos; plazo de una semana para la primera propuesta presupuestaria.
Supuestos: se asume que las 2 rutas independientes existen o son contratables, ya que el cliente las plantea como requisito de construcción y no como pregunta; se asume que ambas oficinas tienen espacio y energía disponibles (no confirmado); se asume que la replicación de bases de datos es el único servicio previsto sobre el enlace (no confirmado).
Recomendaciones: definir el modo de replicación antes de dimensionar, porque determina la viabilidad de la arquitectura sobre 58 km; confirmar la velocidad de las interfaces de los routers antes de fijar cantidades de módulos tributarios; entregar la estimación como rango y con los supuestos declarados, dado el plazo de una semana.

# Información faltante
- Modo de replicación de la base de datos: síncrona o asíncrona, y RPO/RTO objetivo del negocio.
- Volumen de datos a replicar y caudal pico esperado, para dimensionar la capacidad.
- Velocidad y tipo de las interfaces ópticas de los routers Cisco en ambas oficinas.
- Confirmación de que las 2 rutas de fibra son físicamente independientes en todo su recorrido, sin tramos, ductos o cámaras compartidas.
- Mecanismo de continuidad esperado ante el corte de una ruta (resuelto en capa óptica, en capa de routing, u otro) y SLA asociado.
- Restricciones de espacio, energía y refrigeración en ambas oficinas.
- Ventanas de mantenimiento permitidas para tareas programadas.
- Latencia máxima tolerada por la aplicación que consume la replicación.

# Preguntas de aclaración
- ¿La replicación de la base de datos es síncrona o asíncrona? Si es síncrona, ¿cuál es el RPO/RTO objetivo? Los 58 km agregan del orden de 0,58 ms de latencia de ida y vuelta solo por propagación, y en replicación síncrona ese retardo impacta directamente en el tiempo de confirmación de cada transacción.
- ¿Qué volumen de datos se replica y cuál es el caudal pico esperado? Determina si la capacidad requerida está más cerca de 10G o de 100G.
- ¿Las 2 rutas de fibra son independientes en todo su recorrido, o comparten algún tramo, ducto o cámara? Un tramo compartido es un punto único de falla que anula el objetivo de continuidad.
- ¿Qué interfaces ópticas tienen hoy los routers Cisco en cada oficina?
- Cuando dicen que "debe seguir funcionando con una sola ruta" ante un corte, ¿el servicio debe continuar sin interrupción perceptible para la aplicación de replicación, o se acepta un restablecimiento en segundos o minutos?
- ¿Existen ventanas de mantenimiento en las que una interrupción programada sea tolerable?
- ¿Hay restricciones de espacio o energía en los racks de alguna de las dos oficinas?
- El plazo de una semana, ¿es para recibir el número preliminar, o esperan además una definición completa de la solución en ese plazo?

# Alternativas de solución

## Opción 1 — Transporte DWDM sobre las dos rutas, con continuidad en capa óptica
Descripción: Sistema DWDM entre ambas oficinas utilizando las dos rutas de fibra independientes que pide el cliente, con la continuidad resuelta en la capa óptica. El mecanismo concreto y el SLA quedan por confirmar con el cliente.
Ventajas: aprovecha directamente el requisito de dos rutas ya definido por SYNNEX; la conmutación en capa óptica es transparente para la aplicación de replicación; deja margen de crecimiento sobre la misma infraestructura.
Limitaciones: requiere equipamiento en ambos extremos de las dos rutas; la ventaja real depende de que la independencia física de las rutas sea completa a lo largo de todo el recorrido.
Riesgos: técnico — si la replicación resulta ser síncrona y el volumen es alto, la latencia agregada por los 58 km puede requerir revisar la arquitectura de la aplicación, no solo la del transporte. Operativo — sin confirmación de independencia física de las rutas, el objetivo de continuidad puede no cumplirse en la práctica.

## Opción 2 — Transporte simple sobre una ruta, con la segunda como respaldo gestionado por el cliente
Descripción: Transporte óptico sobre la ruta principal, con la segunda ruta disponible y la conmutación ante falla gestionada por los routers Cisco existentes.
Ventajas: menor inversión inicial en equipamiento óptico; el cliente conserva el control de la política de conmutación; aprovecha routers ya instalados.
Limitaciones: los tiempos de recuperación suelen ser mayores que los de una protección en capa óptica y pueden ser visibles para la replicación; consume puertos e ingeniería de routing del lado del cliente.
Riesgos: técnico — en replicación síncrona, una conmutación lenta puede provocar la caída de la sesión de replicación. Operativo — la responsabilidad de la continuidad queda a cargo del equipo de redes de SYNNEX, y depende de que los routers tengan puertos y licencias disponibles.

# Evaluación de la preparación para la cotización o estimación
Estado: Parcialmente listo
Justificación: Alcanza para producir la estimación presupuestaria preliminar que SYNNEX pide dentro de la semana, siempre que se declare explícitamente el supuesto de capacidad adoptado y se entregue como rango. No alcanza para una cotización formal: sin el modo de replicación, el caudal de datos, la confirmación de independencia física de las rutas y las interfaces de los routers, la capacidad, la cantidad de transponders y el mecanismo de continuidad quedan indefinidos. La estimación debe presentarse con esa salvedad escrita, no como un precio cerrado.

# Borrador de la nota de ingeniería

Resumen de la oportunidad: interconexión de dos oficinas principales de SYNNEX Corp separadas por unos 58 km sobre fibra oscura, con dos rutas independientes, para replicación de bases de datos.

Requisitos del cliente: dos rutas de fibra independientes; continuidad de servicio con una sola ruta operativa ante corte; integración con routers Cisco existentes; primera propuesta presupuestaria en una semana.

Supuestos de diseño: se asume fibra monomodo estándar en ambas rutas; se asume que los 58 km se cubren sin amplificación intermedia, sujeto a validación del presupuesto de potencia óptica por Ingeniería; se asume una capacidad inicial de 10G para la estimación preliminar, a confirmar contra el caudal real; se asume independencia física completa de las dos rutas.

Solución(es) propuesta(s): DWDM sobre ambas rutas con continuidad en capa óptica (Opción 1) como alternativa preferente por transparencia ante la replicación; transporte simple con respaldo gestionado por el cliente (Opción 2) como alternativa de menor inversión inicial.

Cuestiones pendientes: modo de replicación y latencia tolerada; caudal real de datos; interfaces de los routers Cisco; independencia efectiva de las dos rutas; mecanismo de continuidad y SLA.

Riesgos: que la replicación sea síncrona con requisitos de latencia ajustados frente a la distancia de 58 km; que las dos rutas compartan algún tramo y el objetivo de continuidad no se cumpla; que la estimación preliminar entregada en una semana se interprete como precio cerrado.

Próximos pasos: enviar las preguntas de aclaración priorizando el modo de replicación y la independencia de las rutas; preparar la estimación como rango con supuestos declarados; validar con Ingeniería el presupuesto de potencia óptica de ambas rutas sobre 58 km.

(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE antes de circular — Fase 5, nivel L3.)

# Reunión de seguimiento con el cliente

Objetivo: confirmar el modo de replicación, el caudal de datos y la independencia real de las dos rutas, para poder convertir la estimación preliminar en una cotización formal.

Orden del día: modo de replicación y requisitos de la aplicación; volumen y caudal a replicar; verificación de independencia física de las dos rutas; interfaces de los routers Cisco; alcance y validez de la estimación preliminar entregada.

Temas clave de discusión: el impacto de los 58 km sobre una eventual replicación síncrona; por qué un tramo de ducto compartido anularía el objetivo de continuidad pedido; la diferencia entre el número preliminar entregado en una semana y una cotización formal posterior.

Decisiones requeridas del cliente: modo de replicación objetivo; capacidad a contratar; si la continuidad se resuelve en capa óptica o de routing.

Resultados esperados: datos suficientes para avanzar hacia una cotización formal y acuerdo sobre la fecha de esa cotización.

# Próximas acciones recomendadas
1. Enviar las preguntas de aclaración a SYNNEX, priorizando el modo de replicación y la independencia de las rutas (el SE las revisa antes de enviarlas — nivel L2).
2. Preparar la estimación presupuestaria preliminar como rango, con los supuestos declarados por escrito, dentro del plazo de una semana.
3. Solicitar a SYNNEX la documentación de recorrido de ambas rutas de fibra.
4. Coordinar el inventario de interfaces ópticas de los routers Cisco en ambas oficinas.
5. Validar con Ingeniería el presupuesto de potencia óptica para 58 km sobre las dos rutas.