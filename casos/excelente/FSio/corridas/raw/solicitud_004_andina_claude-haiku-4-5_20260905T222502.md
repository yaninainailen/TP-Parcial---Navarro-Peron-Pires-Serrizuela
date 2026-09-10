<!--
Caso: solicitud_004_andina.md
Modelo: claude-haiku-4-5
Fecha: 2026-09-05T22:25:02
system_prompt.md (sha256, 12 chars): 76d91e443f52
max_tokens solicitado: 28000
stop_reason: end_turn
Tokens de entrada: 15098
Tokens de salida: 3056
Costo estimado de esta corrida (USD): 0.0304
-->

```
# Resumen de la oportunidad
Tipo de entregable solicitado por el cliente: Cotización formal (ANDINA LITIO solicitó explícitamente "una cotización")
Urgencia declarada por el cliente: "propuesta preliminar en 20 días"

ANDINA LITIO, cliente potencial del sector minero, busca interconectar una planta de procesamiento en Salta, Argentina, con oficinas centrales en Antofagasta, Chile, mediante fibra óptica terrestre de 340 km. La ruta cruza la Cordillera de los Andes por un paso de alta montaña, con tramos de más de 120 km sin infraestructura eléctrica ni sitios para amplificación. Capacidad inicial 100G.

# Inconsistencias detectadas
No se detectaron inconsistencias internas en la información proporcionada. La descripción de la ruta, las distancias y las limitaciones de infraestructura son coherentes.

# Requisitos del cliente
- Interconexión entre Salta (Argentina) y Antofagasta (Chile), distancia total aproximada 340 km.
- Ruta terrestre por la Cordillera de los Andes, mediante paso de alta montaña.
- Capacidad inicial 100G.
- Capacidad para operar en tramos de más de 120 km sin infraestructura eléctrica disponible ni sitios intermedios de amplificación.
- Cotización formal solicitada.
- Plazo de propuesta preliminar: 20 días.
- El cliente asume que los permisos de derecho de paso y regulatorios son "un trámite estándar".

# Clasificación de la información
Hechos: distancia total 340 km; ruta transfronteriza Argentina-Chile; topografía de Cordillera de los Andes; tramos de más de 120 km sin infraestructura eléctrica; capacidad 100G inicial; plazo de 20 días; cliente potencial, no cliente existente.
Supuestos: se asume que existe al menos una ruta de fibra físicamente disponible o que puede contratarse; se asume que los 100G iniciales son el único requisito de tráfico (no se menciona crecimiento futuro).
Recomendaciones: esta oportunidad debe escalarse a especialistas en ingeniería de líneas fotónicas y a equipo de regulatorio y derecho de paso. No es un proyecto que pueda dimensionarse dentro del flujo estándar.

# ESCALACIÓN REQUERIDA

**Esta oportunidad cumple múltiples criterios de escalación. Se detiene el flujo de análisis estándar y se remite a especialistas.**

Factores que activan escalación:
1. **Ruta transfronteriza con estatus regulatorio y de derecho de paso poco claro:** la solicitud de una propuesta preliminar en 20 días, combinada con la asunción del cliente de que los permisos son "un trámite estándar", indica un gap crítico de descubrimiento regulatorio. La interconexión Argentina-Chile atraviesa jurisdicciones nacionales, acuerdos binacionales, regulaciones de telecomunicaciones distintas en cada país, y posiblemente requisitos de seguridad/defensa en zona fronteriza de alta montaña. Esto no es un trámite estándar.

2. **Ingeniería de línea fotónica a medida:** tramos de más de 120 km sin infraestructura eléctrica ni sitios intermedios exigen una ingeniería de amplificación excepcional. El cálculo presupuestario de potencia óptica no puede hacerse sobre supuestos de amplificación estándar. Se requiere evaluación de:
   - Presupuesto de potencia para recorridos sin regeneración intermedia.
   - Opciones de amplificación remota o alimentada por energía solar/aislada en los mismos tramos.
   - Características de la fibra disponible (pérdida, dispersión, atenuación) en esas distancias.
   - Viabilidad técnica de instalar y mantener equipamiento en zonas de alta montaña sin acceso eléctrico.

3. **Plazo vs. complejidad:** el cliente pide una "propuesta preliminar en 20 días". Una propuesta preliminar que elida los riesgos regulatorios y técnicos no cumpliría con la obligación de diligencia: sería una promesa sin fundamento. La escalación es necesaria para definir si el plazo es realista o si debe negociarse con el cliente.

---

# Información faltante

- **Estatus regulatorio y de derecho de paso:** ¿Existe ya algún acuerdo de reciprocidad Argentina-Chile para tendido de fibra transfronterizo? ¿Qué autoridades (ISED, SUBTEL, Aduana, Defensa) intervienen? ¿Hay antecedentes de proyectos similares en la zona?
- **Disponibilidad de fibra:** ¿Existe fibra tendida ya en la ruta, o se debe construir desde cero? ¿Quién es propietario y bajo qué términos se puede usar?
- **Especificación de la fibra existente (si aplica):** tipo, atenuación, dispersión, edad y estado de la fibra en la ruta propuesta.
- **Perfiles de elevación y clima:** mapa de altura, temperatura extrema, velocidad del viento, nieve, y otras condiciones ambientales de los 340 km.
- **Infraestructura eléctrica real:** ubicación exacta de los tramos sin energía; distancia a la fuente eléctrica más cercana para cada tramo; viabilidad de generar energía in situ (solar, diésel, otros).
- **Acceso logístico:** caminos de acceso, períodos de navegabilidad, restricciones de movimiento en zona fronteriza de alta montaña.
- **Interfaces tributarias:** tipo de equipo en Salta y Antofagasta que será punto terminal del enlace.
- **Requisitos de servicio:** SLA de disponibilidad, latencia tolerada, ventanas de mantenimiento permitidas.
- **Presupuesto y restricciones del cliente:** si existe capacidad presupuestaria para une solución de alta complejidad.

---

# Preguntas de aclaración

**Nota:** Estas preguntas son insumo para que los especialistas en regulatorio y líneas fotónicas profundice el descubrimiento. El Sales Engineer debe validarlas con esos especialistas antes de enviarlas al cliente.

- ¿Existe actualmente fibra en la ruta propuesta, o debe construirse desde cero? Si existe, ¿quién es propietario y bajo qué términos es accesible?
- ¿Ha habido algún proyecto previo de telecomunicaciones transfronterizo Argentina-Chile en la zona de Salta-Antofagasta? Si es así, ¿qué lecciones hay sobre permisos y logística?
- ¿Cuál es el presupuesto aproximado que el cliente está dispuesto a invertir? (Esto orienta si la solución será económicamente viable.)
- ¿Los 100G iniciales incluyen previsión de crecimiento, o son la proyección final? ¿Hay horizonte de escalabilidad mencionado?
- En los tramos de más de 120 km sin energía, ¿el cliente está abierto a soluciones de amplificación remota, solar, o prefiere evitar sitios intermedios?
- ¿Hay ventanas de mantenimiento permitidas? En una ruta de alta montaña, ¿hay períodos en los que la accesibilidad es nula?
- ¿Cuál es el SLA de disponibilidad esperado para un enlace de esta complejidad?

---

# Recomendaciones inmediatas

1. **Escalar a especialista en Regulatorio y Derecho de Paso (telecom transfronteriza):** validar si existe marco legal para la interconexión Argentina-Chile, qué trámites intervienen, cuál es el plazo realista de permisos, y si hay restricciones de seguridad nacional en zona de frontera de alta montaña.

2. **Escalar a especialista en Ingeniería de Líneas Fotónicas (sistemas de larga distancia):** evaluar:
   - Viabilidad técnica de los tramos de 120+ km sin regeneración intermedia.
   - Presupuesto de potencia óptica en esas distancias.
   - Opciones de amplificación (en sitio, remota, solar).
   - Características de fibra requerida y disponibilidad.
   - Impacto ambiental de la ruta (dispersión, PMD, pérdida por climática).

3. **Coordinar con Producto/Pricing sobre la realidad del plazo:** ¿es posible entregar una propuesta preliminar seria en 20 días considerando que faltan la validación regulatoria y la ingeniería de línea? Si no, el SE debe negociar con el cliente un plazo más realista para esta fase de descubrimiento.

4. **En paralelo, el Sales Engineer debe:**
   - Confirmar con el cliente el tipo de entregable esperado: ¿propuesta preliminar de alto nivel indicativa, o propuesta técnica con dimensionamiento?
   - Validar el presupuesto aproximado disponible.
   - Documentar qué sitios específicos en Salta y Antofagasta son los puntos terminales exactos.
   - Solicitar cualquier información existente del cliente sobre topografía, fibra, o permisos previos.

---

# Próximas acciones recomendadas

1. **No avanzar con análisis de soluciones estándar (Fases 3–5) hasta que los especialistas validen la viabilidad regulatoria y técnica.** Este proyecto no puede cotizarse con supuestos de plataforma estándar.

2. **Preparar un resumen de escalación interno** que incluya: el factor de riesgo (transfronterizo + línea fotónica excepcional), el plazo del cliente vs. realidad del descubrimiento, y los actores que deben intervenir.

3. **Coordinar con Regulatorio:** enviar a ese equipo las preguntas de regulatorio y solicitar una evaluación preliminar de viabilidad en semana 1.

4. **Coordinar con Ingeniería de Líneas Fotónicas:** entregar el mapa de la ruta, distancias, limitaciones de infraestructura eléctrica, y solicitar un análisis técnico preliminar en semana 1–2.

5. **El Sales Engineer recontacta al cliente dentro de 3–5 días** con las preguntas de aclaración (priorizando estatus regulatorio previo, fibra disponible, presupuesto), sin comprometerse a la propuesta preliminar en 20 días hasta que tenga input de los especialistas.

6. **Si los especialistas confirman viabilidad técnica y regulatoria realista**, se replantea el plazo con el cliente y se procede a Fases 3–5 con arquitectura definida por Ingeniería de Líneas Fotónicas, no por flujo estándar.
```

---

### Nota de gobernanza

Este análisis **no incluye las Fases 3–5** (Alternativas de solución, Evaluación de preparación para cotización, Nota de ingeniería, Reunión de seguimiento) porque la entrada cumple criterios de escalación que impiden continuar por el flujo estándar. Las Fases 1–2 se completaron para preservar el trabajo de descubrimiento; los hallazgos de esas fases son el insumo que los especialistas necesitan para profundizar.

El Sales Engineer debe usar este documento para **justificar internamente** por qué no puede entregar una cotización formal en 20 días, y para **estructurar** la conversación con los especialistas y con el cliente. El plazo negociado con el cliente y la validación de viabilidad técnica-regulatoria son los gatillos para retomar el análisis estándar de alternativas y costing.