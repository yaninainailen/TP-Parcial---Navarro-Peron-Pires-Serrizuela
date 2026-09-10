<!--
Caso: solicitud_001_acme.md
Modelo: claude-sonnet-5
Fecha: 2026-09-05T21:57:18
system_prompt.md (sha256, 12 chars): ad45bc6c055a
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 19714
Tokens de salida: 4650
Costo estimado de esta corrida (USD): 0.1289
-->

# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: No especificado (ACME pide "una propuesta" en 3 semanas; no aclara si espera una cotización formal o una estimación presupuestaria preliminar)
Urgencia declarada por el cliente: "una propuesta en 3 semanas"

ACME Corp evalúa interconectar dos centros de datos propios separados por unos 120 km, con una capacidad inicial de 400G y una ampliación prevista a 800G en un horizonte de 2 años, junto con un requisito declarado de sobrevivencia ante el corte de una sola fibra. Es una oportunidad de transporte óptico punto a punto sobre infraestructura terrestre, dentro del alcance de la plataforma estándar, con brechas de información que deben cerrarse antes de comprometer un diseño definitivo.

# Requisitos del cliente
- Interconexión de 2 centros de datos propios, distancia aproximada 120 km.
- Capacidad inicial 400G, con ampliación prevista a 800G en un horizonte de 2 años.
- Sobrevivencia del servicio ante el corte de una sola fibra; el cliente no especificó el mecanismo ni el SLA.
- Routers Cisco existentes en ambos extremos (dato del cliente); interfaces ópticas en uso no identificadas.
- Plazo de respuesta esperado: 3 semanas.

# Clasificación de la información
Hechos: distancia aproximada de 120 km entre ambos centros de datos; capacidad inicial 400G; ampliación prevista a 800G en 2 años; existen routers Cisco en ambos extremos; plazo de propuesta de 3 semanas.
Supuestos: se asume que ambos centros de datos son instalaciones propias del cliente y que hay espacio y energía disponibles en ambos (no confirmado); se asume que existe al menos una ruta de fibra utilizable entre ambos sitios (no confirmado); se asume que "corte en una sola fibra" se refiere a un evento de corte físico en un tramo, y no a una degradación de canal (no confirmado).
Recomendaciones: confirmar cuántas rutas físicas independientes de fibra existen antes de comprometer cualquier arquitectura de sobrevivencia; confirmar el tipo de interfaz óptica de los routers antes de dimensionar transponders; tratar el requisito de 800G a 2 años como criterio de diseño desde el inicio, no como una ampliación a resolver después.

# Información faltante
- Mecanismo de sobrevivencia esperado y SLA de disponibilidad asociado: el cliente pidió resistir el corte de una sola fibra, pero no indicó si espera protección óptica dedicada (por ejemplo, dos fibras en rutas separadas), restauración automática, o resiliencia resuelta en la capa de routing.
- Cantidad de rutas físicas de fibra independientes disponibles entre ambos centros de datos, y si comparten algún tramo, ducto o cámara.
- Propiedad y tipo de la fibra: propia, arrendada, o servicio gestionado por un tercero.
- Tipo y velocidad de las interfaces ópticas de los routers Cisco en ambos extremos.
- Restricciones de espacio, energía y refrigeración en ambas salas.
- Requisitos de latencia y de las aplicaciones que van a usar el enlace.
- Ventanas de mantenimiento aceptables para tareas programadas.
- Perfil de crecimiento entre los 400G iniciales y los 800G a 2 años: si es un salto único o incremental, y qué determina el momento del upgrade.

# Preguntas de aclaración
- Cuando dicen que el servicio "debe sobrevivir a un corte en una sola fibra", ¿se refieren a continuidad sin interrupción perceptible, o a un restablecimiento dentro de un tiempo acotado? ¿Existe un SLA de disponibilidad comprometido con el negocio?
- ¿Cuántas rutas físicas de fibra independientes existen hoy entre ambos centros de datos? Si solo hay una, el requisito de sobrevivir al corte de "una sola fibra" podría satisfacerse con un par de fibras en la misma traza, o podría requerir una segunda ruta física completa: la diferencia cambia el alcance del proyecto.
- ¿La fibra es propia, arrendada a un tercero, o piensan contratar un servicio gestionado?
- ¿Qué modelo de router Cisco y qué interfaces ópticas tienen instaladas hoy en cada extremo?
- ¿Qué aplicaciones van a transportar sobre el enlace y qué latencia máxima toleran?
- ¿El crecimiento de 400G a 800G en 2 años es un salto planificado en una fecha determinada, o depende de la evolución del tráfico?
- ¿Hay restricciones de espacio en rack o de energía disponible en alguna de las dos salas?
- El plazo de 3 semanas, ¿es para una cotización formal con precios, o para una estimación presupuestaria preliminar?

# Alternativas de solución

## Opción 1 — Transporte DWDM punto a punto con sobrevivencia en capa óptica
Descripción: Sistema DWDM punto a punto entre ambos centros de datos, dimensionado a 400G iniciales con capacidad de escalar a 800G mediante el agregado de transponders o el uso de mayor ancho de banda por canal sobre la misma infraestructura. El mecanismo concreto de sobrevivencia ante el corte de una fibra queda por definir según la cantidad real de rutas físicas disponibles y el SLA que confirme el cliente.
Ventajas: cubre el crecimiento a 800G sin obra adicional si se dimensiona con margen desde el inicio; la sobrevivencia se resuelve en la capa de transporte, sin consumir recursos del router; 120 km es una distancia habitual para la plataforma estándar, sujeta a validación de presupuesto óptico.
Limitaciones: la sobrevivencia efectiva depende de que exista una segunda ruta física o un par de fibras independiente, algo que el cliente todavía no confirmó; requiere espacio y energía en ambas salas.
Riesgos: técnico — si solo existe una traza física compartida para ambas fibras, ningún mecanismo en capa óptica evita la caída ante un corte que afecte a la traza completa. Operativo — el cliente debe asumir la operación del equipo de transporte adicional, o contratar su gestión.

## Opción 2 — Sobrevivencia resuelta en la capa de routing sobre transporte simple
Descripción: Transporte óptico sin protección dedicada en capa óptica, con la continuidad resuelta a nivel de los routers Cisco existentes mediante enlaces lógicos redundantes sobre dos caminos físicos.
Ventajas: menor cantidad de equipamiento óptico dedicado a protección; aprovecha routers que el cliente ya tiene instalados; el cliente conserva control directo del comportamiento ante falla.
Limitaciones: consume puertos e ingeniería de routing del lado del cliente; los tiempos de recuperación suelen ser mayores que los de una protección en capa óptica; el crecimiento a 800G puede requerir renovar interfaces de router, no solo el transporte.
Riesgos: técnico — depende de que los routers tengan puertos libres, licencias y capacidad para soportar 800G a futuro. Operativo — traslada la responsabilidad de la sobrevivencia al equipo de redes del cliente.

# Evaluación de la preparación para la cotización o estimación
Estado: Parcialmente listo
Justificación: Hay información suficiente para una estimación presupuestaria preliminar de alto nivel, porque la distancia, la capacidad inicial y el objetivo de crecimiento a 2 años están definidos. No la hay para una cotización formal: faltan el mecanismo de sobrevivencia y su SLA, la cantidad de rutas físicas de fibra disponibles y el tipo de interfaz de los routers. Esos tres datos determinan la topología, la cantidad de transponders y, por lo tanto, el precio.

# Borrador de la nota de ingeniería

Resumen de la oportunidad: interconexión de dos centros de datos de ACME Corp separados por unos 120 km, con 400G iniciales, ampliación prevista a 800G en 2 años y requisito de sobrevivencia ante el corte de una sola fibra.

Requisitos del cliente: capacidad 400G con evolución a 800G en 2 años; sobrevivencia ante corte de una sola fibra; integración con routers Cisco existentes; propuesta en 3 semanas.

Supuestos de diseño: se asume enlace terrestre sobre fibra monomodo estándar; se asume que los 120 km requieren validación de presupuesto de potencia óptica por Ingeniería, dado que exceden la distancia cómoda sin amplificación intermedia de otros casos similares; se asume disponibilidad de espacio y energía en ambas salas; se asume que el crecimiento a 800G se dimensiona desde el diseño inicial para evitar una segunda intervención en sitio.

Solución(es) propuesta(s): DWDM punto a punto con sobrevivencia en capa óptica (Opción 1) como alternativa preferente por escalabilidad y transparencia ante falla; sobrevivencia en capa de routing (Opción 2) como alternativa de menor inversión inicial en equipamiento óptico dedicado.

Cuestiones pendientes: mecanismo de sobrevivencia y SLA asociado; cantidad de rutas físicas y su independencia; interfaces de los routers; restricciones de sala; perfil temporal del crecimiento a 800G.

Riesgos: que exista una única traza física compartida para ambas fibras, lo que invalidaría el requisito de sobrevivencia tal como está planteado; interfaces de router incompatibles con el dimensionamiento a 800G; distancia de 120 km que requiera amplificación intermedia no contemplada en la estimación inicial; plazo de 3 semanas ajustado si hace falta relevamiento de sitio.

Próximos pasos: enviar las preguntas de aclaración; solicitar relevamiento de ambas salas y de la planta de fibra; validar el presupuesto de potencia óptica para 120 km con Ingeniería una vez confirmada la ruta.

(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE antes de circular — Fase 5, nivel L3.)

# Reunión de seguimiento con el cliente

Objetivo: cerrar las tres brechas que hoy impiden pasar de una estimación preliminar a una cotización formal: mecanismo de sobrevivencia, rutas físicas disponibles e interfaces de los routers.

Orden del día: confirmación del requisito de sobrevivencia y del SLA esperado; revisión de la planta de fibra disponible entre ambos sitios; inventario de interfaces en los routers Cisco; restricciones de sala; perfil temporal del crecimiento a 800G; próximos pasos y cronograma.

Temas clave de discusión: la diferencia práctica entre "sin interrupción" y "restablecimiento rápido", y su impacto en el costo; la dependencia entre sobrevivencia y cantidad/independencia de rutas físicas; el camino de crecimiento de 400G a 800G y si conviene dimensionar de entrada para evitar una segunda intervención.

Decisiones requeridas del cliente: nivel de disponibilidad objetivo; si la sobrevivencia se resuelve en capa óptica o en capa de routing; si la fibra será propia o contratada; momento previsto del upgrade a 800G.

Resultados esperados: brechas cerradas y acuerdo sobre el tipo de entregable esperado (estimación preliminar o cotización formal) y su fecha.

# Próximas acciones recomendadas
1. Enviar las preguntas de aclaración a ACME (el SE las revisa antes de enviarlas — nivel L2).
2. Solicitar el relevamiento de la planta de fibra entre ambos centros de datos, verificando independencia de trazas.
3. Coordinar el inventario de interfaces ópticas de los routers Cisco.
4. Preparar una estimación presupuestaria preliminar con los supuestos declarados como tales.
5. Validar con Ingeniería el presupuesto de potencia óptica para 120 km una vez confirmada la ruta.