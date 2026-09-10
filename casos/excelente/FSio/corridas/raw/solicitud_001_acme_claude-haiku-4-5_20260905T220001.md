<!--
Caso: solicitud_001_acme.md
Modelo: claude-haiku-4-5
Fecha: 2026-09-05T22:00:01
system_prompt.md (sha256, 12 chars): ad45bc6c055a
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 14877
Tokens de salida: 4267
Costo estimado de esta corrida (USD): 0.0362
-->

# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: No especificado (ACME pide "una propuesta" en 3 semanas; no aclara si espera una cotización formal o una estimación presupuestaria preliminar)
Urgencia declarada por el cliente: "propuesta en 3 semanas"

ACME Corp evalúa interconectar dos centros de datos propios separados por unos 120 km, con 400G iniciales, crecimiento previsto a 800G en dos años, y con un requisito declarado de sobrevivencia ante el corte de una sola fibra. Es una oportunidad de transporte óptico punto a punto sobre infraestructura terrestre, dentro del alcance potencial de la plataforma estándar, aunque 120 km está en el rango superior y requerirá validación de presupuesto de potencia óptica.

---

# Requisitos del cliente

- Interconexión de 2 centros de datos propios, distancia aproximada 120 km.
- Capacidad inicial 400G, con crecimiento previsto a 800G en dos años.
- Sobrevivencia ante el corte de una sola fibra; el cliente no especificó el mecanismo ni el SLA.
- Enrutadores Cisco existentes en ambos extremos (dato del cliente); interfaces ópticas en uso no identificadas.
- Plazo de propuesta esperado: 3 semanas.

---

# Clasificación de la información

**Hechos:** distancia aproximada de 120 km entre ambos centros de datos; capacidad inicial 400G; crecimiento previsto a 800G en dos años; existen enrutadores Cisco en ambos extremos; plazo de propuesta de 3 semanas.

**Supuestos:** se asume que ambos centros de datos son instalaciones propias del cliente y que hay espacio y energía disponibles en los dos (no confirmado); se asume que existe al menos una ruta de fibra utilizable entre ambos sitios (no confirmado); se asume que 120 km se refiere a distancia en línea recta o de ruta aproximada, no a distancia de fibra desplegada (puede ser mayor).

**Recomendaciones:** confirmar la distancia real de fibra desplegada antes de comprometer cualquier arquitectura, porque a 120 km el presupuesto de potencia óptica entra en el rango crítico y puede requerir amplificación intermedia o fibras especiales; confirmar cuántas rutas físicas independientes de fibra existen, porque la sobrevivencia a un corte depende de ello; confirmar el tipo de interfaz óptica de los enrutadores antes de dimensionar transponders.

---

# Información faltante

- Distancia real de fibra desplegada entre ambos centros de datos (la distancia de 120 km puede aumentar significativamente cuando se mide por la ruta real del cable).
- Mecanismo de sobrevivencia esperado ante el corte de una fibra y SLA de disponibilidad asociado: el cliente pidió no perder el servicio ante un corte, pero no indicó si espera protección óptica dedicada (1+1, 1:N), restauración, o resiliencia resuelta en la capa de routing.
- Cantidad de rutas físicas de fibra independientes disponibles entre ambos centros de datos.
- Propiedad y tipo de la fibra: propia, arrendada, o servicio gestionado por un tercero.
- Tipo, modelo y velocidad de las interfaces ópticas de los enrutadores Cisco en ambos extremos.
- Características de la fibra: tipo (SMF estándar, LEAF, otro), coeficiente de atenuación, dispersión cromática.
- Restricciones de espacio, energía y refrigeración en ambas salas de equipamiento.
- Requisitos de latencia de las aplicaciones que van a usar el enlace.
- Ventanas de mantenimiento aceptables para tareas programadas.

---

# Preguntas de aclaración

- ¿Cuál es la distancia exacta de la ruta de fibra desplegada entre ambos centros de datos? Los 120 km pueden traducirse en 150+ km cuando se mide el cable real.
- Cuando dicen que "debe sobrevivir a un corte en una sola fibra", ¿se refieren a que el servicio debe continuar sin interrupción perceptible, o a que debe restablecerse dentro de un tiempo acotado? ¿Hay un SLA de disponibilidad comprometido con el negocio?
- ¿Cuántas rutas físicas de fibra independientes existen hoy entre ambos centros de datos? Si hay una sola, la continuidad ante el corte de esa ruta requiere un camino alternativo, con obra civil o contratación adicional.
- ¿La fibra es propia, arrendada a un tercero, o piensan contratar un servicio gestionado?
- ¿Qué modelo de enrutador Cisco tienen y qué interfaces ópticas están instaladas hoy en cada extremo (1G, 10G, 25G, 100G, 400G)?
- ¿Cuál es el tipo de fibra desplegada: monomodo estándar (G.652), LEAF, o algún otro tipo?
- ¿Qué aplicaciones van a transportar sobre el enlace y qué latencia máxima toleran?
- ¿Hay restricciones de espacio en rack o de energía disponible en alguna de las dos salas de equipamiento?

---

# Alternativas de solución

## Opción 1 — Transporte DWDM punto a punto con protección óptica dedicada (1+1)

**Descripción:** Sistema DWDM punto a punto entre ambos centros de datos con protección 1+1 dedicada (dos transponders por dirección con conmutación automática en capa óptica). Dimensionado a 400G iniciales con escalabilidad a 800G mediante agregado de canales DWDM sobre la misma infraestructura. El mecanismo requiere dos rutas de fibra independientes.

**Ventajas:** cubre el crecimiento a 800G sin obra adicional en fibra; la conmutación en capa óptica es transparente para los enrutadores y las aplicaciones; 400G inicial y 800G a futuro se alcanzan en tiempo real sin cortes; proporciona sobrevivencia guaranteed ante el corte de una ruta.

**Limitaciones:** requiere dos rutas de fibra independientes, que todavía no se confirmó que existan; a 120 km el presupuesto de potencia óptica está en el borde y puede requerir amplificación intermedia, cosa que encarece la solución; consume el doble de transponders que una solución sin protección óptica.

**Riesgos:** técnico — si la distancia real es mayor a 120 km o el tipo de fibra tiene atenuación alta, la protección 1+1 puede no cerrarse sin amplificadores, y eso escala el costo significativamente y requiere validación con Ingeniería; la compatibilidad entre la conmutación óptica automática y los enrutadores debe confirmarse. Operativo — el cliente debe asumir la operación de un equipo de transporte adicional, o contratar su gestión.

---

## Opción 2 — Transporte DWDM punto a punto sin protección en capa óptica, con resiliencia en capa de routing

**Descripción:** Transporte óptico DWDM simple entre ambos centros de datos, sin protección dedicada en capa óptica. La continuidad ante el corte se resuelve a nivel de los enrutadores Cisco existentes mediante enlaces lógicos redundantes o ECMP sobre dos caminos, o mediante arquitectura de enrutamiento que reconozca ambas rutas de fibra.

**Ventajas:** menor cantidad de equipamiento óptico y menor costo inicial; aprovecha enrutadores que el cliente ya tiene instalados; el cliente conserva control directo del comportamiento ante falla; permite crecer a 800G con mayor flexibilidad de encaminamiento.

**Limitaciones:** consume puertos e ingeniería de routing del lado del cliente; los tiempos de recuperación suelen ser mayores que los de una protección en capa óptica (del orden de segundos a decenas de segundos); requiere también dos rutas de fibra independientes para ser efectiva.

**Riesgos:** técnico — depende de que los enrutadores Cisco tengan puertos ópticos libres y las licencias de routing redundante necesarias; los tiempos de convergencia de OSPF/BGP pueden ser visibles para aplicaciones sensibles a latencia. Operativo — traslada la responsabilidad de la resiliencia al equipo de redes del cliente; requiere más ingeniería de diseño de red por parte del cliente.

---

## Opción 3 — Transporte DWDM con amplificación intermedia (si la distancia real lo requiere)

**Descripción:** Si la distancia de fibra desplegada resulta ser significativamente mayor a 120 km o el presupuesto de potencia óptica no cierra a 400G sin amplificación, se incorpora una estación de amplificación intermedia (o amplificadores distribuidos en la línea). Esto extiende el alcance y permite una mejor gestión de la potencia óptica.

**Ventajas:** cierra presupuestos de potencia que de otro modo no cerraría; permite manejar fibras con atenuación mayor; ofrece flexibilidad en la gestión de la potencia de línea.

**Limitaciones:** agrega complejidad operativa e ingeniería de línea fotónica; requiere un sitio intermedio (puede ser un punto de paso sin equipamiento de switching, o un nodo operacional); aumenta el costo significativamente; introduce un punto adicional donde pueden ocurrir fallas.

**Riesgos:** técnico — la estación intermedia se convierte en un punto único de falla que puede interrumpir el servicio si no se la protege; requiere poder, refrigeración y acceso físico en un sitio que podría estar en zona remota. Operativo — aumenta la complejidad de mantención y diagnostico del enlace.

---

# Evaluación de la preparación para la cotización o estimación

**Estado:** Parcialmente listo

**Justificación:** Hay información suficiente para una estimación presupuestaria preliminar de alto nivel, porque la distancia aproximada, la capacidad inicial y el objetivo de crecimiento están definidos. No la hay para una cotización formal: faltan la distancia real de fibra desplegada (crítica a 120 km para el presupuesto de potencia óptica), el mecanismo de sobrevivencia y su SLA, la cantidad de rutas físicas de fibra disponibles y el tipo de interfaz de los enrutadores. Esos cuatro datos determinan si se requiere amplificación, la cantidad de transponders, la topología y, por lo tanto, el precio final. Además, 120 km está en el rango donde la validación de Ingeniería es obligatoria antes de hacer una propuesta.

---

# Borrador de la nota de ingeniería

**Resumen de la oportunidad:** interconexión de dos centros de datos de ACME Corp separados por unos 120 km, con 400G iniciales, crecimiento a 800G en dos años y requisito declarado de sobrevivencia ante el corte de una fibra.

**Requisitos del cliente:** capacidad 400G con evolución a 800G en 2 años; sobrevivencia ante el corte de una fibra; integración con enrutadores Cisco existentes; propuesta en 3 semanas.

**Supuestos de diseño:** se asume enlace terrestre sobre fibra monomodo estándar; se asume que la distancia de 120 km es aproximada y debe confirmarse con la distancia real de fibra desplegada; se asume que los 120 km pueden requerir validación de presupuesto de potencia óptica y posible amplificación intermedia; se asume disponibilidad de espacio y energía en ambas salas (no confirmado).

**Solución(es) propuesta(s):** DWDM punto a punto con protección 1+1 (Opción 1) como alternativa preferente si existen dos rutas y el presupuesto de potencia cierra; DWDM sin protección óptica con resiliencia en routing (Opción 2) como alternativa de menor inversión inicial; amplificación intermedia (Opción 3) si la distancia real de fibra supera significativamente 120 km.

**Cuestiones pendientes:** distancia real de fibra desplegada; mecanismo de sobrevivencia y SLA asociado; cantidad de rutas físicas independientes; interfaces ópticas de los enrutadores Cisco; tipo y características de la fibra disponible; presupuesto de potencia óptica validado por Ingeniería.

**Riesgos:** técnico — 120 km es distancia crítica; si la fibra desplegada es mayor o la fibra tiene atenuación alta, puede requerirse amplificación intermedia, lo que cambia la solución y el costo significativamente; la compatibilidad de la conmutación óptica con los enrutadores debe confirmarse. Operativo — el cliente debe asumir operación del equipo de transporte o contratar su gestión; plazo de 3 semanas es ajustado si hace falta relevamiento de sitio y validación de Ingeniería.

**Próximos pasos:** enviar las preguntas de aclaración priorizando distancia de fibra desplegada y rutas disponibles; solicitar relevamiento de ambas salas; validar el presupuesto de potencia óptica con Ingeniería una vez confirmada la ruta y características de fibra.

*(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE antes de circular — Fase 5, nivel L3.)*

---

# Reunión de seguimiento con el cliente

**Objetivo:** cerrar las cuatro brechas críticas que hoy impiden pasar de una estimación preliminar a una cotización formal: distancia real de fibra desplegada, cantidad de rutas físicas independientes, mecanismo de sobrevivencia y SLA asociado, interfaces ópticas de los enrutadores.

**Orden del día:** confirmación de la distancia real de fibra entre ambos centros de datos; disponibilidad de rutas físicas independientes; requisito de sobrevivencia y SLA esperado; inventario de interfaces ópticas de los enrutadores Cisco; restricciones de sala en ambas ubicaciones; próximos pasos y cronograma.

**Temas clave de discusión:** por qué la distancia real de fibra desplegada es crítica a 120 km y puede determinar si hace falta amplificación intermedia; la diferencia práctica entre "sin interrupción" y "restablecimiento rápido", y su impacto en el costo; la dependencia entre sobrevivencia y cantidad de rutas físicas independientes; el camino de crecimiento de 400G a 800G sin obra adicional en fibra; el rango de plazo realista si hay relevamiento de sitio y validación de Ingeniería.

**Decisiones requeridas del cliente:** mecanismo de sobrevivencia objetivo (protección óptica vs. resiliencia en routing); si la fibra será propia, arrendada, o contratada como servicio; confirmación de la ubicación de ambos centros de datos para cotizar la distancia real; confirmación de los modelos y interfaces Cisco.

**Resultados esperados:** distancia real de fibra confirmada; cantidad de rutas físicas definida; mecanismo de sobrevivencia y SLA acordados; interfaces ópticas identificadas; acuerdo sobre el tipo de entregable esperado (estimación preliminar o cotización formal) y su fecha.

---

# Próximas acciones recomendadas

1. Enviar las preguntas de aclaración a ACME, priorizando la distancia real de fibra desplegada y la cantidad de rutas disponibles (el SE las revisa antes de enviarlas — nivel L2).
2. Solicitar el relevamiento de la planta de fibra entre ambos centros de datos, con distancia medida y tipo de fibra.
3. Coordinar el inventario de interfaces ópticas de los enrutadores Cisco y modelos exactos.
4. Solicitar información sobre el sitio intermedio (si aplica) en caso de que la distancia real requiera amplificación.
5. Validar con Ingeniería el presupuesto de potencia óptica para 120 km una vez confirmada la ruta y características de fibra; esto puede determinar si la solución requiere amplificación.
6. Preparar una estimación presupuestaria preliminar con los supuestos declarados como tales, indicando que la cotización formal depende de la validación de Ingeniería.