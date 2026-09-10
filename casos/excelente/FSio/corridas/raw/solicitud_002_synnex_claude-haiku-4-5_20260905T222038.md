<!--
Caso: solicitud_002_synnex.md
Modelo: claude-haiku-4-5
Fecha: 2026-09-05T22:20:38
system_prompt.md (sha256, 12 chars): 76d91e443f52
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 15099
Tokens de salida: 2760
Costo estimado de esta corrida (USD): 0.0289
-->

```
# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: Estimación presupuestaria preliminar (SYNNEX pide "una primera propuesta presupuestaria"; no solicita una cotización formal cerrada)
Urgencia declarada por el cliente: "en una semana"

SYNNEX Corp busca interconectar dos oficinas principales separadas por unos 58 km sobre fibra oscura, con el objetivo específico de replicar bases de datos. Pide explícitamente una construcción sobre dos rutas de fibra independientes y continuidad ante el corte de una de ellas. El entregable esperado es una estimación presupuestaria preliminar en un plazo acotado.

# Requisitos del cliente
- Interconexión de 2 oficinas principales, distancia aproximada 58 km, sobre fibra oscura.
- Aplicación destino: replicación de bases de datos.
- La interconexión debe construirse sobre 2 rutas de fibra ópticas independientes.
- Continuidad del servicio ante el corte de una de las rutas; el cliente no especificó el mecanismo ni el SLA.
- Routers Cisco existentes en ambos extremos; velocidad de las interfaces ópticas sin definir (1G, 10G o 100G).
- Estimación presupuestaria preliminar requerida en una semana.

# Clasificación de la información
Hechos: distancia aproximada de 58 km; fibra oscura como medio; requisito explícito de 2 rutas independientes; aplicación de replicación de base de datos; plazo de una semana para una propuesta presupuestaria preliminar.
Supuestos: se asume que las 2 rutas independientes existen o son contratables, ya que el cliente las plantea como requisito de construcción y no como pregunta; se asume que ambas oficinas tienen espacio y energía disponibles (no confirmado); se asume que la replicación es el único servicio previsto sobre el enlace (no confirmado).
Recomendaciones: definir el modo de replicación antes de dimensionar, porque determina la viabilidad de la arquitectura; entregar la estimación como rango y con los supuestos declarados, dado el plazo de una semana.

# Información faltante
- Modo de replicación de la base de datos: síncrona o asíncrona. Es el dato que más condiciona el diseño.
- Latencia máxima tolerada por la aplicación, y RPO/RTO objetivo del negocio.
- Velocidad y tipo de las interfaces ópticas de los routers en ambos extremos.
- Volumen de datos a replicar y caudal pico esperado, para dimensionar la capacidad.
- Confirmación de que las 2 rutas de fibra son físicamente independientes en todo su recorrido, sin tramos compartidos.
- Mecanismo de continuidad esperado ante el corte de una ruta y SLA asociado.
- Restricciones de espacio, energía y refrigeración en ambas oficinas.
- Ventanas de mantenimiento permitidas para la aplicación de replicación.

# Preguntas de aclaración
- ¿La replicación entre ambas oficinas es síncrona o asíncrona? Si es síncrona, ¿cuál es el RPO/RTO objetivo? Los 58 km agregan del orden de 0,58 ms de latencia de ida y vuelta solo por propagación, y en replicación síncrona ese retardo impacta directamente en el tiempo de confirmación de cada transacción.
- ¿Qué volumen de datos se replica y cuál es el caudal pico? Determina si la capacidad requerida está más cerca de 10G o de 100G.
- ¿Las 2 rutas de fibra son independientes en todo su recorrido, o comparten algún tramo, ducto o cámara? Un tramo compartido es un punto único de falla que anula el objetivo.
- ¿Qué interfaces ópticas tienen hoy los routers Cisco en cada oficina?
- Ante el corte de una ruta, ¿el servicio debe continuar sin interrupción perceptible para la aplicación, o se acepta un restablecimiento en segundos?
- ¿Existen ventanas de mantenimiento en las que una interrupción programada sea tolerable para la aplicación de replicación?
- ¿Hay restricciones de espacio en rack o de energía disponible en alguna de las dos oficinas?

# Alternativas de solución

## Opción 1 — Transporte DWDM sobre las dos rutas, con continuidad en capa óptica
Descripción: Sistema DWDM entre ambas oficinas utilizando las dos rutas de fibra independientes que pide el cliente, con la continuidad resuelta en la capa óptica. El mecanismo concreto queda por confirmar junto con el SLA esperado.
Ventajas: aprovecha directamente el requisito de dos rutas que el cliente ya definió; la conmutación en capa óptica es transparente para la aplicación de replicación; deja capacidad de crecimiento sobre la misma infraestructura.
Limitaciones: requiere equipamiento en ambos extremos de las dos rutas; la ventaja real depende de que la independencia física de las rutas sea completa.
Riesgos: técnico — si la replicación resulta ser síncrona y el volumen es alto, la latencia agregada puede requerir revisar la arquitectura de la aplicación, no solo la del transporte. Operativo — una aplicación de base de datos crítica en producción restringe fuertemente las ventanas de corte para la implementación.

## Opción 2 — Transporte simple sobre una ruta, con la segunda como respaldo gestionado por el cliente
Descripción: Transporte óptico sobre la ruta principal, con la segunda ruta disponible y la conmutación gestionada por los routers Cisco del cliente.
Ventajas: menor inversión inicial en equipamiento óptico; el cliente decide la política de conmutación; aprovecha inteligencia de routing que ya posee.
Limitaciones: los tiempos de recuperación son mayores y pueden ser visibles para la replicación; consume puertos e ingeniería de routing del lado del cliente.
Riesgos: técnico — en replicación síncrona, una conmutación lenta puede provocar la caída de la sesión de replicación. Operativo — la responsabilidad de la continuidad queda del lado del equipo de redes de SYNNEX.

# Evaluación de la preparación para la cotización o estimación
Estado: Parcialmente listo
Justificación: Alcanza para producir la estimación presupuestaria preliminar que el cliente pide dentro de una semana, siempre que se declare explícitamente el supuesto de capacidad adoptado y se entregue como rango. No alcanza para una cotización formal: sin el modo de replicación, el caudal y las interfaces de los routers, la capacidad y la cantidad de transponders quedan indefinidas. La estimación debe presentarse con esa salvedad escrita, no como un precio cerrado.

# Borrador de la nota de ingeniería

Resumen de la oportunidad: interconexión de dos oficinas principales de SYNNEX Corp separadas por unos 58 km sobre fibra oscura, con dos rutas independientes, para replicación de bases de datos.

Requisitos del cliente: dos rutas de fibra independientes; continuidad ante el corte de una ruta; soporte de la replicación de base de datos; estimación presupuestaria preliminar en una semana.

Supuestos de diseño: se asume fibra monomodo estándar en ambas rutas; se asume que 58 km se cubren sin amplificación intermedia, sujeto a validación por Ingeniería; se asume una capacidad inicial de 10G para la estimación preliminar, a confirmar contra el caudal real; se asume independencia física completa de las dos rutas.

Solución(es) propuesta(s): DWDM sobre ambas rutas con continuidad en capa óptica (Opción 1) como alternativa preferente para una aplicación de base de datos crítica; transporte simple con respaldo gestionado por el cliente (Opción 2) como alternativa de menor inversión.

Cuestiones pendientes: modo de replicación y latencia tolerada; caudal real; interfaces de los routers; independencia efectiva de las rutas.

Riesgos: que la replicación sea síncrona con requisitos de latencia incompatibles con la distancia; que las rutas compartan un tramo y el objetivo de continuidad no se cumpla; que la estimación preliminar se interprete como precio cerrado.

Próximos pasos: enviar las preguntas de aclaración priorizando el modo de replicación; preparar la estimación como rango con supuestos declarados; validar con Ingeniería el presupuesto de potencia óptica de ambas rutas.

(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE antes de circular — Fase 5, nivel L3.)

# Reunión de seguimiento con el cliente

Objetivo: confirmar el modo de replicación y el caudal para convertir la estimación preliminar en una cotización formal, y validar la independencia real de las dos rutas.

Orden del día: modo de replicación y requisitos de la aplicación; volumen y caudal a replicar; verificación de independencia física de las rutas; interfaces de los routers; alcance y validez de la estimación preliminar.

Temas clave de discusión: el impacto de los 58 km sobre la replicación síncrona; por qué un tramo de ducto compartido anula el objetivo de continuidad; la diferencia entre una estimación preliminar y una cotización formal.

Decisiones requeridas del cliente: modo de replicación objetivo; capacidad a contratar; si la continuidad se resuelve en capa óptica o de routing.

Resultados esperados: datos suficientes para cotizar formalmente y acuerdo sobre la fecha de esa cotización.

# Próximas acciones recomendadas
1. Enviar las preguntas de aclaración, priorizando el modo de replicación (el SE las revisa antes de enviarlas — nivel L2).
2. Preparar la estimación preliminar como rango, con los supuestos declarados por escrito, dentro de una semana.
3. Solicitar a SYNNEX la documentación de recorrido de ambas rutas de fibra.
4. Coordinar el inventario de interfaces ópticas de los routers Cisco.
5. Validar con Ingeniería el presupuesto de potencia óptica de las dos rutas.
6. Agendar la reunión de seguimiento para la semana siguiente, una vez entregada la estimación.
```