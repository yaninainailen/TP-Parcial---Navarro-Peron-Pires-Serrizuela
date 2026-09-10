<!--
Caso: solicitud_001_acme.md
Modelo: claude-sonnet-5
Fecha: 2026-09-05T22:18:23
system_prompt.md (sha256, 12 chars): 76d91e443f52
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 19959
Tokens de salida: 4520
Costo estimado de esta corrida (USD): 0.1277
-->

# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: No especificado (ACME pide "una propuesta" en 3 semanas; no aclara si espera una cotización formal o una estimación presupuestaria preliminar)
Urgencia declarada por el cliente: "una propuesta en 3 semanas"

ACME Corp evalúa interconectar dos centros de datos propios separados por unos 120 km, con 400G iniciales y crecimiento previsto a 800G en dos años, y con un requisito declarado de sobrevivencia ante el corte de una sola fibra. Es una oportunidad de transporte óptico punto a punto sobre infraestructura terrestre, dentro del alcance de la plataforma estándar, aunque la distancia amerita validar el presupuesto de potencia óptica con Ingeniería.

# Requisitos del cliente
- Interconexión de 2 centros de datos propios, distancia aproximada 120 km.
- Capacidad total inicial de 400G, con crecimiento previsto a 800G en un horizonte de 2 años.
- Sobrevivencia ante el corte de una sola fibra; el cliente no especificó el mecanismo ni el SLA.
- Routers Cisco existentes en ambos extremos (dato del cliente); interfaces ópticas en uso no identificadas.
- Plazo de respuesta esperado: 3 semanas.

# Clasificación de la información
Hechos: distancia aproximada de 120 km entre ambos centros de datos; capacidad inicial 400G; crecimiento previsto a 800G en 2 años; existen routers Cisco en ambos extremos; plazo de propuesta de 3 semanas.
Supuestos: se asume que ambos centros de datos son instalaciones propias del cliente y que hay espacio y energía disponibles en los dos (no confirmado); se asume que existe al menos una ruta de fibra utilizable entre ambos sitios y que "una sola fibra" se refiere a un par de fibras dentro de esa ruta (no confirmado); a 120 km, se asume que puede requerirse amplificación intermedia o compensación de la línea óptica, sujeto a validación del presupuesto de potencia por Ingeniería.
Recomendaciones: confirmar cuántas rutas físicas independientes de fibra existen antes de comprometer cualquier arquitectura de resiliencia, ya que "sobrevivir a un corte en una sola fibra" puede significar cosas muy distintas según haya una o dos rutas; confirmar el tipo de interfaz óptica de los routers antes de dimensionar transponders.

# Información faltante
- Mecanismo de sobrevivencia esperado y SLA de disponibilidad asociado: el cliente pidió sobrevivir al corte de una sola fibra, pero no indicó si espera protección óptica dedicada, restauración, o resiliencia resuelta en la capa de routing.
- Cantidad de rutas físicas de fibra independientes disponibles entre ambos centros de datos, y si "una sola fibra" implica un corte de ducto completo o solo de un par de fibras dentro de un cable con otros pares disponibles.
- Propiedad y tipo de la fibra: propia, arrendada, o servicio gestionado por un tercero.
- Tipo y velocidad de las interfaces ópticas de los routers Cisco.
- Restricciones de espacio, energía y refrigeración en ambas salas.
- Requisitos de latencia de las aplicaciones que van a usar el enlace.
- Perfil de la fibra en la ruta de 120 km (tipo de fibra, atenuación, empalmes), necesario para validar si hace falta amplificación intermedia.
- Ventanas de mantenimiento aceptables para tareas programadas.

# Preguntas de aclaración
- Cuando dicen que debe "sobrevivir a un corte en una sola fibra", ¿se refieren a la pérdida de un par de fibras dentro de un cable con más pares, o al corte del ducto o cable completo? Esto cambia por completo el diseño de resiliencia.
- ¿El servicio debe continuar sin interrupción perceptible ante ese corte, o se acepta un restablecimiento en un tiempo acotado? ¿Hay un SLA de disponibilidad comprometido con el negocio?
- ¿Cuántas rutas físicas de fibra independientes existen hoy entre ambos centros de datos?
- ¿La fibra es propia, arrendada a un tercero, o piensan contratar un servicio gestionado?
- ¿Qué modelo de router Cisco y qué interfaces ópticas tienen instaladas hoy en cada extremo?
- ¿Qué aplicaciones van a transportar sobre el enlace y qué latencia máxima toleran?
- ¿Se cuenta con información sobre el tipo de fibra y la atenuación esperada en los 120 km de recorrido?
- ¿Hay restricciones de espacio en rack o de energía disponible en alguna de las dos salas?

# Alternativas de solución

## Opción 1 — Transporte DWDM punto a punto con resiliencia en capa óptica
Descripción: Sistema DWDM punto a punto entre ambos centros de datos, dimensionado a 400G iniciales y escalable a 800G mediante el agregado de transponders sobre la misma infraestructura. El mecanismo de resiliencia concreto queda por definir según lo que el cliente entienda por "corte en una sola fibra" y según la cantidad real de rutas físicas disponibles.
Ventajas: cubre el crecimiento a 800G sin obra adicional; la resiliencia se resuelve en la capa de transporte, sin consumir recursos del router; la plataforma estándar admite 120 km, sujeto a confirmación del presupuesto de potencia.
Limitaciones: la resiliencia efectiva depende de qué tan independiente sea la segunda fibra o ruta respecto de la primera, algo que el cliente todavía no confirmó; requiere espacio y energía en ambas salas; a 120 km puede requerir amplificación intermedia, a validar por Ingeniería.
Riesgos: técnico — si "una sola fibra" implica en realidad un corte de ducto completo y solo existe una ruta física, ninguna protección en capa óptica sobre esa misma ruta evita la caída. Operativo — el cliente debe asumir la operación de un equipo de transporte adicional, o contratar su gestión.

## Opción 2 — Resiliencia resuelta en la capa de routing sobre transporte simple
Descripción: Transporte óptico sin protección en capa óptica, con la continuidad resuelta a nivel de los routers Cisco existentes mediante enlaces lógicos redundantes sobre dos caminos.
Ventajas: menor cantidad de equipamiento óptico; aprovecha routers que el cliente ya tiene instalados; el cliente conserva control directo del comportamiento ante falla.
Limitaciones: consume puertos e ingeniería de routing del lado del cliente; los tiempos de recuperación suelen ser mayores que los de una protección en capa óptica; requiere igualmente una segunda ruta física para ser efectiva.
Riesgos: técnico — depende de que los routers tengan puertos libres y licencias para 400G/800G, algo aún no confirmado. Operativo — traslada la responsabilidad de la resiliencia al equipo de redes del cliente.

# Evaluación de la preparación para la cotización o estimación
Estado: Parcialmente listo
Justificación: Hay información suficiente para una estimación presupuestaria preliminar de alto nivel, porque la distancia, la capacidad inicial y el objetivo de crecimiento están definidos. No la hay para una cotización formal: falta precisar qué significa exactamente "sobrevivir a un corte en una sola fibra" y su SLA, la cantidad de rutas físicas de fibra disponibles, el tipo de interfaz de los routers, y la validación del presupuesto de potencia óptica para 120 km. Esos datos determinan la cantidad de transponders, la necesidad de amplificación y la topología, y por lo tanto el precio.

# Borrador de la nota de ingeniería

Resumen de la oportunidad: interconexión de dos centros de datos de ACME Corp separados por unos 120 km, con 400G iniciales, crecimiento a 800G en dos años y requisito de sobrevivencia ante el corte de una sola fibra.

Requisitos del cliente: capacidad 400G con evolución a 800G; sobrevivencia ante corte de una sola fibra; integración con routers Cisco existentes; propuesta en 3 semanas.

Supuestos de diseño: se asume enlace terrestre sobre fibra monomodo estándar; a 120 km se asume que puede requerirse amplificación o compensación intermedia, sujeto a validación del presupuesto de potencia óptica por Ingeniería; se asume disponibilidad de espacio y energía en ambas salas; se asume que "una sola fibra" refiere a un par dentro de un cable, pendiente de confirmación con el cliente.

Solución(es) propuesta(s): DWDM punto a punto (Opción 1) como alternativa preferente por escalabilidad a 800G; resiliencia en capa de routing (Opción 2) como alternativa de menor inversión inicial.

Cuestiones pendientes: alcance exacto del requisito de sobrevivencia y SLA asociado; cantidad de rutas físicas; interfaces de los routers; validación del presupuesto de potencia para 120 km; restricciones de sala.

Riesgos: que exista una única ruta física, lo que limitaría severamente el alcance real de la sobrevivencia solicitada; que los 120 km requieran amplificación intermedia no contemplada inicialmente; interfaces de router incompatibles con el dimensionamiento previsto para 400G/800G; plazo de 3 semanas ajustado si hace falta relevamiento de sitio.

Próximos pasos: enviar las preguntas de aclaración, priorizando el alcance del requisito de sobrevivencia; solicitar relevamiento de ambas salas y de la ruta de fibra; validar el presupuesto de potencia óptica con Ingeniería una vez confirmada la ruta.

(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE antes de circular — Fase 5, nivel L3.)

# Reunión de seguimiento con el cliente

Objetivo: cerrar las brechas que hoy impiden pasar de una estimación preliminar a una cotización formal: alcance real del requisito de sobrevivencia, rutas físicas disponibles e interfaces de los routers.

Orden del día: confirmación del alcance de "sobrevivir a un corte en una sola fibra" y del SLA esperado; revisión de la planta de fibra disponible entre ambos sitios; inventario de interfaces en los routers Cisco; restricciones de sala; próximos pasos y cronograma.

Temas clave de discusión: la diferencia entre perder un par de fibras y perder un ducto completo, y su impacto en el diseño; la dependencia entre sobrevivencia y cantidad de rutas físicas; el camino de crecimiento de 400G a 800G sin obra adicional; el efecto de los 120 km en el presupuesto de potencia óptica.

Decisiones requeridas del cliente: alcance exacto del requisito de sobrevivencia; nivel de disponibilidad objetivo; si la resiliencia se resuelve en capa óptica o en capa de routing; si la fibra será propia o contratada.

Resultados esperados: brechas cerradas y acuerdo sobre el tipo de entregable esperado (estimación preliminar o cotización formal) y su fecha.

# Próximas acciones recomendadas
1. Enviar las preguntas de aclaración a ACME (el SE las revisa antes de enviarlas — nivel L2).
2. Solicitar el relevamiento de la planta de fibra entre ambos centros de datos, incluyendo cuántos pares y rutas independientes existen.
3. Coordinar el inventario de interfaces ópticas de los routers Cisco.
4. Preparar una estimación presupuestaria preliminar con los supuestos declarados como tales.
5. Validar con Ingeniería el presupuesto de potencia óptica para 120 km una vez confirmada la ruta, incluyendo la necesidad de amplificación intermedia.