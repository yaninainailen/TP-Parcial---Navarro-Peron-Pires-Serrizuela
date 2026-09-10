<!--
Caso: solicitud_002_synnex.md
Modelo: claude-sonnet-5
Fecha: 2026-09-05T21:58:17
system_prompt.md (sha256, 12 chars): ad45bc6c055a
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 19792
Tokens de salida: 6437
Costo estimado de esta corrida (USD): 0.1559
-->

# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: Estimación presupuestaria preliminar (SYNNEX pide "una primera propuesta presupuestaria"; no solicita una cotización formal)
Urgencia declarada por el cliente: "en una semana"

SYNNEX Corp busca interconectar sus 2 oficinas principales, separadas por unos 58 km, sobre fibra oscura, con el objetivo de replicar bases de datos entre ambos sitios. El cliente pide explícitamente que la solución se construya sobre 2 rutas de fibra independientes y que el servicio continúe operando con una sola ruta disponible ante el corte de la otra, sin especificar el SLA asociado. El entregable esperado es una estimación preliminar en un plazo corto, no un proceso completo de cotización.

# Requisitos del cliente
- Interconexión de 2 oficinas principales, distancia aproximada 58 km, sobre fibra oscura.
- Aplicación destino: replicación de bases de datos.
- Construcción sobre 2 rutas de fibra ópticas independientes.
- Continuidad de servicio con una sola ruta operativa ante el corte de la otra; sin SLA especificado por el cliente.
- Routers Cisco existentes en ambos extremos.
- Interfaces ópticas de los routers sin definir (1G, 10G o 100G).
- Propuesta presupuestaria preliminar requerida en 1 semana.

# Clasificación de la información
Hechos: distancia aproximada de 58 km entre ambas oficinas; fibra oscura como medio; requisito explícito de 2 rutas independientes; aplicación de replicación de bases de datos; routers Cisco existentes en ambos extremos; plazo de 1 semana para la propuesta.
Supuestos: se asume que las 2 rutas independientes existen o son contratables, dado que el cliente las plantea como requisito de construcción y no como pregunta; se asume que ambas oficinas cuentan con espacio y energía disponibles para alojar equipamiento (no confirmado); se asume que la replicación de bases de datos es el único servicio previsto sobre el enlace en esta primera etapa (no confirmado).
Recomendaciones: priorizar la confirmación del modo de replicación (síncrono/asíncrono) antes de avanzar en el dimensionamiento, ya que es el dato que determina si 58 km resultan viables para la aplicación tal como está planteada; entregar la estimación preliminar como rango, con los supuestos declarados por escrito, dado el plazo ajustado de 1 semana.

# Información faltante
- Modo de replicación de la base de datos: síncrona o asíncrona, y RPO/RTO objetivo del negocio. Es el dato que más condiciona la viabilidad del diseño a esta distancia.
- Volumen de datos a replicar y caudal pico esperado, para dimensionar la capacidad requerida.
- Confirmación de que las 2 rutas de fibra son físicamente independientes en todo su recorrido, sin tramos, ductos o cámaras compartidas.
- Velocidad y tipo de las interfaces ópticas de los routers Cisco en ambas oficinas.
- Mecanismo de continuidad esperado ante el corte de una ruta y SLA de disponibilidad asociado.
- Restricciones de espacio, energía y refrigeración en ambas oficinas.
- Ventanas de mantenimiento permitidas para tareas programadas sobre el enlace.
- Propiedad de la fibra oscura: si ya está contratada por SYNNEX o si forma parte del alcance del proyecto.

# Preguntas de aclaración
- ¿La replicación entre ambas oficinas es síncrona o asíncrona? Si es síncrona, ¿cuál es el RPO/RTO objetivo? Los 58 km agregan del orden de 0,58 ms de latencia de ida y vuelta solo por propagación, y ese retardo puede impactar directamente en el tiempo de confirmación de cada transacción si la replicación es síncrona.
- ¿Qué volumen de datos se replica y cuál es el caudal pico esperado? Determina si la capacidad requerida está más cerca de 10G o de 100G.
- ¿Las 2 rutas de fibra oscura son independientes en todo su recorrido, o comparten algún tramo, ducto o cámara? Un tramo compartido anularía el objetivo de continuidad planteado.
- ¿Qué interfaces ópticas tienen hoy instaladas los routers Cisco en cada oficina?
- Ante el corte de una ruta, ¿el servicio debe continuar sin interrupción perceptible para la aplicación, o se acepta un breve restablecimiento?
- ¿Existen ventanas de mantenimiento tolerables para la aplicación de replicación?
- ¿Hay restricciones de espacio, energía o refrigeración en los racks de alguna de las dos oficinas?
- ¿La fibra oscura entre ambas oficinas ya está contratada, o su obtención forma parte del alcance de este proyecto?

# Alternativas de solución

## Opción 1 — Transporte DWDM sobre las dos rutas, con continuidad en capa óptica
Descripción: Sistema DWDM entre ambas oficinas, desplegado sobre las 2 rutas de fibra independientes que pide el cliente, con la continuidad ante el corte de una ruta resuelta en la capa óptica. El mecanismo concreto (por ejemplo, conmutación de protección u otro esquema) queda por confirmar junto con el SLA que el cliente aún no especificó.
Ventajas: aprovecha directamente el requisito de dos rutas ya definido por el cliente; la conmutación en capa óptica es transparente para la aplicación de replicación; deja capacidad de crecimiento disponible sobre la misma infraestructura.
Limitaciones: requiere equipamiento óptico en ambos extremos de las dos rutas; el beneficio real depende de que la independencia física de las rutas sea completa y verificable.
Riesgos: técnico — si la replicación resulta ser síncrona con un caudal alto, la latencia agregada por los 58 km puede requerir revisar la arquitectura de la aplicación, no solo la del transporte. Operativo — sin confirmación del SLA esperado, existe riesgo de sobredimensionar o subdimensionar la solución respecto de lo que el negocio realmente necesita.

## Opción 2 — Transporte simple sobre una ruta, con la segunda como respaldo gestionado por el cliente
Descripción: Transporte óptico sobre la ruta principal, dejando la segunda ruta disponible como respaldo, con la conmutación entre rutas gestionada por los routers Cisco existentes del cliente.
Ventajas: menor inversión inicial en equipamiento óptico dedicado; el cliente conserva control directo sobre la política de conmutación; aprovecha routers que ya están instalados.
Limitaciones: los tiempos de recuperación ante un corte suelen ser mayores que los de una protección en capa óptica; consume puertos e ingeniería de routing del lado del cliente.
Riesgos: técnico — en un escenario de replicación síncrona, una conmutación lenta entre rutas puede provocar la caída de la sesión de replicación. Operativo — la responsabilidad de garantizar la continuidad queda del lado del equipo de redes de SYNNEX, no de la plataforma de transporte.

# Evaluación de la preparación para la cotización o estimación
Estado: Parcialmente listo
Justificación: La información disponible alcanza para producir la estimación presupuestaria preliminar que el cliente solicita dentro del plazo de 1 semana, siempre que se entregue como rango y con los supuestos de capacidad declarados explícitamente. No alcanza para una cotización formal: el modo de replicación, el caudal de datos, la independencia real de las dos rutas y las interfaces de los routers son datos que determinan directamente la capacidad, la cantidad de equipamiento y, por lo tanto, el precio final.

# Borrador de la nota de ingeniería

Resumen de la oportunidad: interconexión de dos oficinas principales de SYNNEX Corp separadas por unos 58 km, sobre fibra oscura, con dos rutas independientes, para replicación de bases de datos.

Requisitos del cliente: dos rutas de fibra independientes; continuidad de servicio ante el corte de una ruta; integración con routers Cisco existentes; propuesta presupuestaria preliminar en 1 semana.

Supuestos de diseño: se asume fibra monomodo estándar en ambas rutas; se asume que los 58 km se cubren sin amplificación intermedia, sujeto a validación del presupuesto de potencia óptica por Ingeniería; se asume una capacidad inicial de referencia de 10G para la estimación preliminar, a confirmar contra el caudal real de replicación; se asume independencia física completa entre las dos rutas.

Solución(es) propuesta(s): DWDM sobre ambas rutas con continuidad en capa óptica (Opción 1), como alternativa preferente dado el requisito explícito de dos rutas; transporte simple con respaldo gestionado por el cliente (Opción 2), como alternativa de menor inversión inicial.

Cuestiones pendientes: modo de replicación y latencia tolerada; caudal real de datos; interfaces ópticas de los routers; independencia física verificada de las dos rutas; SLA esperado ante falla.

Riesgos: que la replicación resulte ser síncrona con requisitos de latencia ajustados frente a los 58 km de distancia; que las dos rutas compartan algún tramo físico y el objetivo de continuidad no se cumpla realmente; que la estimación preliminar se interprete como un precio cerrado antes de confirmar los datos pendientes.

Próximos pasos: enviar las preguntas de aclaración priorizando el modo de replicación; preparar la estimación preliminar como rango con los supuestos declarados; validar con Ingeniería el presupuesto de potencia óptica de ambas rutas una vez confirmadas las distancias reales.

(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE antes de circular — Fase 5, nivel L3.)

# Reunión de seguimiento con el cliente

Objetivo: confirmar el modo de replicación y el caudal de datos para poder convertir la estimación preliminar en una cotización formal, y validar la independencia real de las dos rutas de fibra.

Orden del día: modo de replicación y requisitos de la aplicación; volumen y caudal de datos a replicar; verificación de independencia física de las dos rutas; interfaces ópticas de los routers Cisco; alcance y validez de la estimación preliminar a entregar.

Temas clave de discusión: el impacto de los 58 km sobre una eventual replicación síncrona; por qué un tramo de ducto compartido entre las dos rutas anularía el objetivo de continuidad; la diferencia entre el número preliminar entregado en esta etapa y una cotización formal posterior.

Decisiones requeridas del cliente: modo de replicación objetivo; capacidad a contratar; nivel de SLA esperado ante el corte de una ruta; si la fibra oscura ya está contratada o si su obtención forma parte del proyecto.

Resultados esperados: datos suficientes para avanzar de la estimación preliminar a una cotización formal, y acuerdo sobre la fecha de esa cotización.

# Próximas acciones recomendadas
1. Enviar las preguntas de aclaración a SYNNEX, priorizando el modo de replicación (el SE las revisa antes de enviarlas — nivel L2).
2. Preparar la estimación presupuestaria preliminar como rango, con los supuestos declarados por escrito, dentro del plazo de 1 semana.
3. Solicitar a SYNNEX la documentación de recorrido de las dos rutas de fibra oscura, para verificar independencia física.
4. Coordinar el relevamiento de interfaces ópticas de los routers Cisco en ambas oficinas.
5. Validar con Ingeniería el presupuesto de potencia óptica para 58 km una vez confirmadas las distancias reales de cada ruta.