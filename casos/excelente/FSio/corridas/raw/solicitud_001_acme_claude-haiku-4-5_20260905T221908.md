<!--
Caso: solicitud_001_acme.md
Modelo: claude-haiku-4-5
Fecha: 2026-09-05T22:19:08
system_prompt.md (sha256, 12 chars): 76d91e443f52
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 15050
Tokens de salida: 3981
Costo estimado de esta corrida (USD): 0.0350
-->

```
# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: No especificado (ACME Corp pide "una propuesta" en 3 semanas; no aclara si espera una cotización formal o una estimación presupuestaria preliminar)
Urgencia declarada por el cliente: "una propuesta en 3 semanas"

ACME Corp evalúa interconectar dos centros de datos propios separados por unos 120 km, con 400G iniciales, crecimiento previsto a 800G en dos años, y con un requisito declarado de continuidad de servicio ante el corte de una sola fibra. Es una oportunidad de transporte óptico punto a punto sobre infraestructura terrestre, dentro del alcance de la plataforma estándar.

# Requisitos del cliente
- Interconexión de 2 centros de datos propios, distancia aproximada 120 km.
- Capacidad inicial 400G, con crecimiento previsto a 800G en un horizonte de 2 años.
- Continuidad de servicio ante el corte de una sola fibra; el cliente no especificó el mecanismo ni el SLA.
- Enrutadores Cisco existentes en ambos extremos (dato del cliente); interfaces ópticas en uso no identificadas.
- Plazo de respuesta esperado: 3 semanas.

# Clasificación de la información
Hechos: distancia aproximada de 120 km entre ambos centros de datos; capacidad inicial 400G; crecimiento previsto a 800G en 2 años; existen enrutadores Cisco en ambos extremos; plazo de propuesta de 3 semanas.
Supuestos: se asume que ambos centros de datos son instalaciones propias del cliente y que hay espacio y energía disponibles en los dos (no confirmado); se asume que existe al menos una ruta de fibra utilizable entre ambos sitios (no confirmado); se asume que 120 km se puede cubrir sin amplificación intermedia, sujeto a validación del presupuesto de potencia óptica por Ingeniería.
Recomendaciones: confirmar cuántas rutas físicas independientes de fibra existen antes de comprometer cualquier arquitectura de resiliencia; confirmar el tipo de interfaz óptica de los enrutadores antes de dimensionar transponders; aclarar si la distancia permite enlace directo o si hace falta amplificación.

# Información faltante
- Mecanismo de resiliencia esperado y SLA de disponibilidad asociado: el cliente pidió no perder el servicio ante un corte de fibra, pero no indicó si espera protección óptica dedicada (1+1, mesh), restauración automática o resiliencia resuelta en la capa de routing.
- Cantidad de rutas físicas de fibra independientes disponibles entre ambos centros de datos.
- Propiedad y tipo de la fibra: propia, arrendada, o servicio gestionado por un tercero.
- Tipo, velocidad y cantidad de las interfaces ópticas de los enrutadores Cisco en cada extremo.
- Restricciones de espacio, energía y refrigeración en ambas salas.
- Requisitos de latencia de las aplicaciones que van a usar el enlace.
- Ventanas de mantenimiento aceptables para tareas programadas.
- Validación de viabilidad técnica para 120 km: presupuesto de potencia óptica y necesidad de amplificación intermedia.

# Preguntas de aclaración
- Cuando dicen que el servicio "debe sobrevivir a un corte en una sola fibra", ¿se refieren a que debe continuar sin interrupción perceptible, o a que debe restablecerse dentro de un tiempo acotado? ¿Hay un SLA de disponibilidad comprometido con el negocio?
- ¿Cuántas rutas físicas de fibra independientes existen hoy entre ambos centros de datos? Si hay una sola, la continuidad ante el corte de esa ruta requiere un camino alternativo con obra civil o contratación adicional.
- ¿La fibra es propia, arrendada a un tercero, o piensan contratar un servicio gestionado?
- ¿Qué modelo de enrutador Cisco y qué interfaces ópticas tienen instaladas hoy en cada extremo? ¿Hay puertos libres disponibles?
- ¿Qué aplicaciones van a transportar sobre el enlace y qué latencia máxima toleran? La distancia de 120 km agrega aproximadamente 1,2 ms de latencia de ida y vuelta por propagación en fibra.
- ¿Hay restricciones de espacio en rack o de energía disponible en alguna de las dos salas?
- ¿Existen ventanas de mantenimiento programadas en las que una interrupción sea tolerable?
- ¿El crecimiento a 800G en 2 años es un requisito firme o una estimación? ¿Se espera realizarlo con la misma infraestructura de fibra?

# Alternativas de solución

## Opción 1 — Transporte DWDM punto a punto con protección óptica dedicada (1+1)
Descripción: Sistema DWDM punto a punto entre ambos centros de datos sobre dos rutas de fibra independientes, con protección 1+1 en capa óptica. Dimensionado a 400G iniciales y escalable a 800G mediante el agregado de transponders sobre la misma infraestructura de fibra.
Ventajas: cubre el crecimiento a 800G sin obra adicional en fibra; la resiliencia se resuelve en la capa de transporte, sin consumir recursos del enrutador; la conmutación ante falla es transparente para las aplicaciones; 120 km es viable con la plataforma estándar, sujeto a validación del presupuesto de potencia óptica.
Limitaciones: requiere dos rutas físicas de fibra completamente independientes; la resiliencia efectiva depende de que no existan tramos compartidos de ducto o cámara; requiere espacio y energía en ambas salas para equipamiento de protección.
Riesgos: técnico — si existe una sola ruta física de fibra, ninguna protección en capa óptica evita la caída ante el corte de esa ruta; si las rutas comparten tramos de infraestructura, el objetivo de resiliencia no se cumple; la validación de presupuesto de potencia óptica a 120 km debe confirmar que no hace falta amplificación intermedia, de lo contrario el costo cambia. Operativo — el cliente debe asumir la operación de un equipo de transporte adicional, o contratar su gestión.

## Opción 2 — Transporte simple sobre ruta principal, con segunda ruta como respaldo gestionado por el cliente
Descripción: Transporte óptico sin protección en capa óptica sobre la ruta principal, con la segunda ruta disponible y la conmutación gestionada por los enrutadores Cisco existentes mediante enlaces lógicos redundantes.
Ventajas: menor cantidad de equipamiento óptico; aprovecha enrutadores que el cliente ya tiene instalados; el cliente conserva control directo del comportamiento ante falla; menor inversión inicial.
Limitaciones: los tiempos de recuperación son mayores (segundos a minutos) y pueden ser visibles para las aplicaciones; consume puertos e ingeniería de routing del lado del cliente; la escalabilidad a 800G puede verse limitada por la cantidad de puertos disponibles.
Riesgos: técnico — depende de que los enrutadores tengan puertos libres y licencias de routing redundante; los tiempos de reconvergencia pueden ser inaceptables para aplicaciones sensibles. Operativo — traslada la responsabilidad de la resiliencia al equipo de redes del cliente; aumenta la complejidad operativa.

## Opción 3 — Transporte DWDM con restauración automática de ruta (APS) en capa óptica
Descripción: Sistema DWDM con dos rutas de fibra independientes, pero con un mecanismo de restauración dinámico (APS — Automatic Protection Switching) en lugar de 1+1 dedicado. El tráfico se envía por la ruta principal y conmuta automáticamente a la respaldo ante falla, sin necesidad de equipamiento de transmisión duplicado.
Ventajas: menor cantidad de transponders y módulos que 1+1, lo que reduce el costo inicial; mantiene transparencia para las aplicaciones; sigue permitiendo crecimiento a 800G.
Limitaciones: requiere dos rutas físicas de fibra independientes; las maniobras de conmutación pueden tomar decenas de milisegundos; no es transparente para aplicaciones de muy baja latencia.
Riesgos: técnico — depende de la confirmación de que existen dos rutas independientes; la velocidad de conmutación debe validarse contra los SLA de las aplicaciones. Operativo — la operación requiere monitoreo continuo de la salud de la ruta respaldo.

# Evaluación de la preparación para la cotización o estimación
Estado: Parcialmente listo
Justificación: Hay información suficiente para una estimación presupuestaria preliminar de alto nivel, porque la distancia (120 km), la capacidad inicial (400G) y el objetivo de crecimiento (800G en 2 años) están definidos. No hay información suficiente para una cotización formal: faltan el mecanismo de resiliencia y su SLA, la cantidad y tipo de rutas físicas de fibra disponibles, el tipo de interfaz de los enrutadores, y la validación de viabilidad técnica para 120 km (presupuesto de potencia óptica, necesidad de amplificación). Esos datos son los que determinan la cantidad de transponders, la topología, la cantidad de módulos de línea y amplificación, y por lo tanto el precio final.

# Borrador de la nota de ingeniería

Resumen de la oportunidad: interconexión de dos centros de datos de ACME Corp separados por unos 120 km, con 400G iniciales, crecimiento a 800G en 2 años y requisito de continuidad de servicio ante el corte de una sola fibra.

Requisitos del cliente: capacidad 400G con evolución a 800G en 2 años; continuidad de servicio ante corte de fibra, mecanismo no especificado; integración con enrutadores Cisco existentes; propuesta en 3 semanas.

Supuestos de diseño: se asume enlace terrestre sobre fibra monomodo estándar; se asume que 120 km puede cubrirse con la plataforma estándar, sujeto a validación del presupuesto de potencia óptica por Ingeniería (presupuesto crítico a esta distancia); se asume que existe al menos una ruta de fibra, y preferiblemente dos independientes para resiliencia; se asume disponibilidad de espacio y energía en ambas salas; se asume que el crecimiento a 800G se realiza sobre la misma infraestructura de fibra.

Solución(es) propuesta(s): DWDM con protección 1+1 sobre dos rutas (Opción 1) como alternativa preferente por transparencia y escalabilidad; DWDM con restauración APS (Opción 3) como alternativa de costo inicial reducido; resiliencia en capa de routing (Opción 2) como alternativa de menor inversión si la arquitectura de la red lo permite.

Cuestiones pendientes: mecanismo de resiliencia y SLA asociado; cantidad y tipo de rutas físicas de fibra; interfaces ópticas de los enrutadores; validación del presupuesto de potencia óptica a 120 km; restricciones de espacio y energía en ambas salas.

Riesgos: técnico — que la validación del presupuesto de potencia óptica a 120 km requiera amplificación intermedia, lo que incrementa el costo y la complejidad; que exista una única ruta física, lo que invalidaría la resiliencia en capa óptica; que las interfaces de los enrutadores sean incompatibles con las opciones propuestas. Operativo — plazo de 3 semanas ajustado si hace falta relevamiento detallado de sitio o validación de fibra.

Próximos pasos: validar el presupuesto de potencia óptica a 120 km con Ingeniería; enviar preguntas de aclaración al cliente sobre rutas de fibra, mecanismo de resiliencia e interfaces de enrutadores; realizar relevamiento de ambas salas si es necesario.

(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE antes de circular — Fase 5, nivel L3.)

# Reunión de seguimiento con el cliente

Objetivo: cerrar las brechas críticas que hoy impiden pasar de una estimación preliminar a una cotización formal: validar viabilidad técnica a 120 km, confirmar mecanismo de resiliencia, identificar rutas de fibra disponibles, e inventariar interfaces de enrutadores.

Orden del día: presentación de las tres opciones de arquitectura y sus implicaciones de costo/resiliencia; validación de viabilidad a 120 km y necesidad de amplificación; revisión de la planta de fibra disponible entre ambos sitios; inventario de interfaces ópticas en los enrutadores Cisco; restricciones de sala; cronograma hacia cotización formal.

Temas clave de discusión: la diferencia práctica entre 1+1 óptico, restauración APS y resiliencia en capa de routing, y sus tiempos de recuperación; la dependencia entre continuidad y cantidad/independencia de rutas físicas; el impacto de la distancia de 120 km en el presupuesto de potencia óptica; el camino de crecimiento de 400G a 800G sin obra de fibra adicional.

Decisiones requeridas del cliente: mecanismo de resiliencia preferido (1+1, APS o routing); validación de disponibilidad de dos rutas físicas independientes; tipo de entregable final (cotización formal o estimación presupuestaria); alcance de la obra de fibra (si la hay).

Resultados esperados: confirmación de viabilidad técnica a 120 km; acuerdo sobre mecanismo de resiliencia; validación de rutas de fibra disponibles e independencia; brechas de información cerradas; acuerdo sobre cronograma hacia cotización formal.

# Próximas acciones recomendadas
1. Validar con Ingeniería el presupuesto de potencia óptica para 120 km y determinar si hace falta amplificación intermedia (paso crítico antes de presentar cualquier opción de arquitectura).
2. Enviar las preguntas de aclaración a ACME Corp, priorizando mecanismo de resiliencia, cantidad de rutas físicas e interfaces de enrutadores (el SE las revisa antes de enviarlas — nivel L2).
3. Solicitar a ACME la documentación de la planta de fibra disponible entre ambos centros de datos, incluyendo trazabilidad física de cada ruta.
4. Coordinar el inventario de interfaces ópticas, velocidades, módulos y puertos disponibles en los enrutadores Cisco de ambos extremos.
5. Preparar una estimación presupuestaria preliminar con los supuestos declarados (viabilidad a 120 km sin amplificación, dos rutas independientes, 1+1 óptico como opción base).
6. Agendar la reunión de seguimiento dentro de los plazos que permitan entregar la propuesta completa en 3 semanas.
```