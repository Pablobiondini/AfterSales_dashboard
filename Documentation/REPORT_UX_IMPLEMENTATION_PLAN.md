# Plan de implementación UX del reporte

## Objetivo y criterio de priorización

Este plan traduce la revisión de [REPORT_UX_REVIEW.md](./REPORT_UX_REVIEW.md)
en trabajo incremental de diseño. El alcance es la experiencia del usuario en
Power BI: páginas, visuales, filtros, navegación, formato y jerarquía. No
incluye cambios en fórmulas DAX ni en el modelo semántico.

Los ítems están ordenados por **ROI estimado**, combinando:

- **Beneficio:** mejora en comprensión, velocidad de lectura, confianza o
  capacidad de actuar.
- **Esfuerzo:** tiempo de autoría en Power BI, ajuste de visuales y QA básico.
- **Riesgo:** posibilidad de ocultar contexto, alterar una interpretación o
  requerir aprobación de negocio.

Los tiempos son por mejora, para una persona que conoce el PBIP y puede abrir
el reporte en Power BI Desktop. No incluyen espera de aprobación, publicación
ni investigación de reglas de negocio. Un total de horas de fase no equivale a
una duración calendario si hay dependencias o revisiones.

**Escala de riesgo:** Bajo = principalmente presentación y reversible;
Medio = afecta lectura, filtros o comparación y requiere validar resultados;
Alto = cambia la narrativa, el alcance o el significado y requiere aprobación
de negocio y validación amplia.

---

## 1. Quick Wins — menos de 1 hora por mejora

### Ordenados por ROI estimado

| ROI | ID | Mejora | Página(s) afectada(s) | Tiempo estimado | Beneficio esperado | Riesgo |
|---:|---|---|---|---:|---|---|
| 1 | QW-1 | Quitar el filtro fijo de `Bonafede Refrigeración` de la ficha del prestador, una vez confirmado que no es una vista de muestra intencional. Dejar el selector en “Todos”/sin filtro o explicitar su estado. | By provider (Individual) → Ficha del prestador | 15–30 min | Evita que el usuario confunda el resultado de un único proveedor con el de toda la red. Alto impacto en confianza con muy poco trabajo. | Medio: cambia los valores iniciales mostrados. Validar que el filtro global no esconda una selección requerida. |
| 2 | QW-2 | Agregar títulos a los visuales más visibles y un encabezado claro por página; comenzar por KPI Overall, KPI YoY, ranking, Customer Care y FVR. | Todas; prioridad en Resumen ejecutivo, Tendencia interanual, Ranking de prestadores, Voz del cliente y Efectividad a la primera visita | 30–50 min para encabezados de página y los visuales principales; titular los 41 visuales completos requiere 1–2 h adicionales y se planifica fuera de este Quick Win | Reduce la dependencia de etiquetas técnicas y permite entender tablas/gráficos sin ensayo y error. | Bajo: texto solamente; revisar que los títulos no se corten ni repitan la página. |
| 3 | QW-3 | Retirar la imagen de marca que duplica los logotipos ya presentes en el fondo. | Customer Care → Voz del cliente | 5–10 min | Recupera espacio y elimina decoración redundante sin quitar información analítica. | Bajo. |
| 4 | QW-4 | Normalizar etiquetas de filtros: `Año`, `Tipo de aviso`, `Temporada`, `Prestador`, `Provincia`; añadir un texto corto “Población: visitas técnicas válidas” en el encabezado o pie persistente. | Todas las páginas con filtros; especialmente Resumen ejecutivo, Tendencia, Prestadores y Geografía | 30–45 min | Hace más explícito el contexto y previene interpretaciones del universo filtrado. | Bajo: no modifica el contexto del filtro; verificar que el texto describa fielmente el filtro actual del reporte. |
| 5 | QW-5 | Alinear y dimensionar los segmentadores dentro de una fila de filtros sencilla; ampliar los controles demasiado estrechos en Ranking de prestadores y Desempeño geográfico. | Resumen ejecutivo; Ranking de prestadores; Desempeño geográfico; FVR | 30–50 min | Mejora descubribilidad, legibilidad y reduce fricción al cambiar filtros. | Bajo: maquetación; probar en 1280×720 y lectura con “Fit to page”. |
| 6 | QW-6 | Retirar las páginas vacías de `Call Center` y `Spare Parts` de la navegación visible hasta que tengan contenido aprobado; alternativamente, ocultarlas si deben conservarse en el PBIP. | Call Center; Spare Parts | 10–20 min | Evita callejones sin salida y elimina expectativas de contenido inexistente. | Medio: usuarios habituales pueden depender de las pestañas aunque estén vacías. Avisar el cambio y confirmar que no hay un uso externo previsto. |
| 7 | QW-7 | Homogeneizar títulos visibles de página en español y capitalización, manteniendo nombres descriptivos y sin siglas sin explicar. | Todas las páginas | 30–50 min | Menos carga cognitiva y mejor orientación para audiencias mixtas. | Bajo: comprobar que los nombres acordados no excedan el espacio del tab bar. |
| 8 | QW-8 | Renombrar los visuales principales con lenguaje de negocio: “Casos cerrados”, “TAT calendario promedio (días)”, “Cumplimiento en ≤5 días hábiles”, etc. | Todas las páginas con visuales, empezando por KPI Overall, Ranking de prestadores y FVR | 30–50 min para el primer grupo; completar el inventario restante requiere otro lote de 30–50 min | Traduce nombres de medidas y categorías técnicas a lenguaje comprensible. | Bajo: mantener definiciones y unidades exactas; no llamar “First Time Fix” a una métrica distinta. |

**Criterio de cierre de Quick Wins:** una persona que no conoce el reporte
debe poder identificar la página, el periodo, la población y el propósito de
los visuales principales sin abrir el panel de filtros.

---

## 2. Mejoras de alto impacto — 1 a 3 horas por mejora

### Ordenadas por ROI estimado

| ROI | ID | Mejora | Página(s) afectada(s) | Tiempo estimado | Beneficio esperado | Riesgo |
|---:|---|---|---|---:|---|---|
| 1 | AI-1 | Reordenar el resumen: crear una fila de KPI con Casos cerrados, TAT hábil, Cumplimiento ≤5 días y FVR; debajo dejar una tendencia y un bloque de lectura. Usar medidas/columnas existentes, sin crear DAX nuevo. | KPI Overall → Resumen ejecutivo | 1.5–2.5 h | La primera pantalla responde “cuánto”, “qué tan rápido” y “qué tan eficaz” sin obligar a leer una matriz ancha. | Medio: exige priorizar qué entra al primer nivel; validar formato y población de cada KPI. |
| 2 | AI-2 | Revisar los estados iniciales de año y temporada: reemplazar 2025/2025–2026 fijos por una selección intencional y documentada; añadir una indicación visible de rango activo y control de restablecimiento. No usar “último periodo” dinámico hasta confirmar la regla temporal de negocio. | Resumen ejecutivo; KPI YoY; Ranking de prestadores; Ficha del prestador; Desempeño geográfico | 1–2 h | Evita cifras obsoletas o una comparación silenciosamente incompleta al abrir el reporte. | Medio: una selección nueva cambia resultados por defecto. Aprobación de calendario/temporada requerida. |
| 3 | AI-3 | Sustituir las tres matrices de KPI YoY por una tendencia interanual principal y una matriz compacta de detalle opcional. Retirar el desglose por condición de repuesto de esta página y reservarlo para su análisis. | KPI YoY → Tendencia interanual; Uso de repuestos si se construye | 2–3 h | Reduce duplicación, da prioridad al cambio en el tiempo y hace más fácil comparar años y meses equivalentes. | Medio: comparar solo rangos completos equivalentes; no inducir comparación de año parcial contra año completo. |
| 4 | AI-4 | Consolidar los dos pivots de FVR filtrados por modalidad en una composición comparativa única (barras/líneas o small multiples con escalas iguales) y dejar una matriz de detalle bajo demanda. | FVR → Efectividad a la primera visita | 1.5–2.5 h | Elimina la repetición vertical y facilita comparar Carry In vs. In Home de un vistazo. | Medio: comprobar que ambos servicios usan el mismo periodo, mismos filtros y definiciones visibles. |
| 5 | AI-5 | Simplificar Ranking de prestadores: ordenar los KPI por prioridad, limitar columnas visibles a casos, TAT hábil y cumplimiento ≤5 días; mover el resto a tooltip/tabla expandida. Agregar barras Top N si no se pierde capacidad de consultar todos los prestadores. | By Provider (Group) → Ranking de prestadores | 2–3 h | Mejora la comparación y conserva la tabla para consulta, reduciendo carga visual. | Medio: Top N puede ocultar prestadores pequeños pero críticos; mantener acceso al total y aclarar el criterio de orden. |
| 6 | AI-6 | Reubicar el mapa de la página de ranking a la página geográfica y aumentar su espacio; titularlo como casos por ubicación del cliente hasta confirmar otra semántica. | By Provider (Group); By Area → Desempeño geográfico | 1–2 h | Alinea visual y pregunta de negocio; hace más legible el mapa sin fingir que las coordenadas son ubicaciones de prestadores. | Medio: validar coordenadas y nivel de precisión/privacidad permitido. |
| 7 | AI-7 | En Voz del cliente, quitar la comparación de dos NPS en columnas si solo duplica las tarjetas; añadir cantidad de respuestas y sustituir las dos donas por barras apiladas al 100% si la comparación de composición queda más clara. | Customer Care → Voz del cliente | 1.5–2.5 h | Contextualiza NPS por tamaño de muestra y facilita comparar composición entre canales. | Medio: mostrar cantidad correcta de respuestas y conservar categorías/colores consistentes; no usar porcentajes sin base visible. |
| 8 | AI-8 | Sincronizar filtros comunes de periodo y tipo de aviso entre páginas que comparten análisis; dejar temporada/prestador como filtros propios donde corresponda. Añadir botón `Restablecer filtros` si el patrón de navegación lo permite. | Resumen, Tendencia, Modalidad, Ranking, Ficha, Geografía y FVR | 2–3 h | Mantiene el contexto y reduce errores de interpretación al navegar. | Alto: una sincronización involuntaria puede ocultar datos. Definir una matriz página × filtro y validar interacciones antes de publicar. |
| 9 | AI-9 | Unificar la tabla de provincias, localidades y centros en un único visual jerárquico expandible; mantener el mapa a su lado y retirar las tablas redundantes. | By Area → Desempeño geográfico | 2–3 h | Reduce duplicación y conserva el contexto al bajar de provincia a localidad/prestador. | Medio: validar jerarquía y comportamiento de expandir/contraer; usuarios que exportan tablas deben probar el nuevo formato. |
| 10 | AI-10 | Fijar colores semánticos para NPS y modalidades, y limitar colores de series temporales a la paleta acordada. | Voz del cliente; Modalidad de servicio; Tendencia interanual; Resumen | 1.5–2.5 h | Mejora consistencia, comparación entre páginas y comprensión de estados sin depender solo de la leyenda. | Bajo–medio: un color semántico puede tener significado corporativo distinto; validar contraste y convención de marca. |
| 11 | AI-11 | Completar o retirar la página Uso de repuestos: mover el desglose existente por condición desde YoY, y mostrar porcentaje junto con cantidad de casos; no añadir métricas que no tengan definición aprobada. | Spare Parts → Uso de repuestos; KPI YoY | 1.5–3 h | Centraliza una pregunta dispersa o elimina una página sin propósito. | Medio: validar si “con repuesto” es una tasa por caso y cuál es el universo relevante. |

**Criterio de cierre de alto impacto:** el usuario debe poder comparar
periodos, modalidades, zonas y prestadores sin duplicaciones evidentes, con
contexto de filtro visible y sin perder acceso al detalle.

---

## 3. Rediseños estructurales — más de 3 horas por mejora

### Ordenados por ROI estimado

| ROI | ID | Rediseño | Página(s) afectada(s) | Tiempo estimado | Beneficio esperado | Riesgo |
|---:|---|---|---|---:|---|---|
| 1 | RE-1 | Rediseñar Resumen ejecutivo como una portada de decisión: KPI, tendencia de volumen, cumplimiento, periodo comparable y hasta tres alertas accionables que enlacen a las páginas de diagnóstico. Mantener el detalle mensual en Tendencia. | KPI Overall → Resumen ejecutivo; Tendencia interanual | 4–6 h | Convierte el reporte en una experiencia de “detectar → entender → actuar” y prioriza decisiones sobre inspección de datos. | Alto: cambia la jerarquía y el foco editorial. Requiere aprobación de KPI principales, objetivos y reglas de comparación. |
| 2 | RE-2 | Implementar navegación de dos recorridos —ejecutivo y operativo— con botones, navegación por páginas, retorno al resumen y estados de página claros. Definir las páginas de detalle como destinos de drillthrough, no como pestañas obligatorias. | Todas; en especial Ficha del prestador y páginas de detalle | 4–7 h | Orienta a dos perfiles diferentes y reduce dependencia del tab bar nativo. | Medio–alto: puede cambiar hábitos de usuarios y requiere verificar navegación, filtros persistentes y experiencia en Desktop/Service. |
| 3 | RE-3 | Rediseñar el sistema visual completo: cuadrícula común, encabezado y filtros en la misma ubicación, tipografía, escalas, márgenes, formato/unidades, color semántico y diseño para 1280×720 y pantallas menores. Simplificar el fondo si los elementos de marca desplazan contenido. | Todas las páginas activas | 6–10 h | Aporta coherencia y legibilidad transversal; reduce el costo de aprender cada página. | Medio: afecta el aspecto de todo el reporte; requiere QA visual página por página y aprobación de marca. |
| 4 | RE-4 | Replantear Desempeño geográfico como diagnóstico espacial: mapa de tamaño suficiente, nivel de geografía inequívoco, matriz jerárquica, coverage si la definición de negocio aplica y ranking de zonas accionable. | By Area → Desempeño geográfico; contenido movido desde Ranking | 4–7 h | Conecta concentración, cobertura y rendimiento en la misma vista para localizar dónde actuar. | Alto: depende de validar geocodificación, significado de ubicación, granularidad y definición de cobertura. |
| 5 | RE-5 | Construir Ficha del prestador con encabezado dinámico, selector claro, KPI del proveedor frente a la red, tendencias comparables y detalle geográfico; enlazar desde Ranking mediante drillthrough. | By provider (Individual); By Provider (Group) | 4–8 h | Permite diagnosticar un proveedor sin perder el benchmark ni confundir la selección con un total de red. | Alto: comparar con la red debe respetar periodo, tipo de servicio y población; puede requerir una experiencia de filtros/modelo aprobada. |
| 6 | RE-6 | Convertir Voz del cliente en análisis de experiencia: NPS y tamaño de muestra arriba, composición de respuestas, tendencia en el tiempo y página Call Center solo para motivos/comentarios que aporten diagnóstico. | Customer Care; Call Center | 4–8 h | Pasa de mostrar puntuaciones sueltas a entender volumen, tendencia y causas de satisfacción/insatisfacción. | Alto: depende de disponibilidad, calidad, privacidad y gobernanza de comentarios de encuesta. |
| 7 | RE-7 | Diseñar Uso de repuestos como vista operativa completa: cantidad y porcentaje, tendencia, modalidad, prestador y acción/drillthrough. Consolidar aquí todas las visualizaciones de repuestos que actualmente están dispersas. | Spare Parts; KPI YoY; Resumen; Ranking de prestadores | 4–6 h | Centraliza el análisis y evita decisiones basadas en un porcentaje sin volumen o en cortes no comparables. | Alto: definir población, denominador, granularidad y decisión que debe soportar antes de publicar. |

**Criterio de cierre estructural:** recorrido probado con representantes de
gerencia y operación; resultados conciliados con los visuales anteriores;
filtros probados en todos los destinos; y revisión de legibilidad en Power BI
Desktop y Service.

---

## Orden de ejecución recomendado

### Lote 1 — confianza y orientación

1. QW-1 — quitar o aclarar prestador fijo.
2. QW-2 y QW-8 — títulos y etiquetas de negocio.
3. QW-4 — población filtrada y contexto de selección.
4. QW-6 — resolver páginas vacías.
5. AI-2 — aprobar y corregir valores iniciales de año/temporada.

Este lote evita el riesgo mayor de UX: que el usuario lea un resultado correcto
como si representara otra población o periodo.

### Lote 2 — lectura diaria

1. AI-1 — jerarquía del Resumen ejecutivo.
2. AI-3 — Tendencia interanual.
3. AI-4 — comparación FVR por modalidad.
4. AI-5 — Ranking de prestadores.
5. AI-7 — Voz del cliente.

Este lote prioriza las páginas que explican desempeño y suelen visitarse con
mayor frecuencia. Debe seguir el orden de ROI de la tabla salvo que el uso real
del reporte indique una prioridad operativa distinta.

### Lote 3 — navegación y diagnóstico

1. AI-8 — filtros comunes y restablecimiento.
2. AI-6 y AI-9 — geografía.
3. RE-2 y RE-5 — recorridos y ficha de prestador.
4. AI-11 / RE-7 — decidir si desarrollar o retirar Uso de repuestos.
5. RE-3 — aplicar el sistema visual en todas las páginas activas.
6. RE-4 y RE-6 — profundizar geografía y voz del cliente.

Evitar sincronizar filtros antes de acordar el comportamiento por página.
Primero definir y aprobar una matriz de filtros; después configurar la
sincronización y probar combinaciones.

## Dependencias y decisiones previas

1. Acordar si el calendario de análisis usa año calendario, temporada
   abril–marzo, o ambas vistas en contextos distintos.
2. Confirmar si las selecciones 2025, temporada 2025–2026 y Bonafede
   Refrigeración son intencionales o residuo de edición.
3. Validar metas de TAT/FVR y si deben mostrarse semáforos. No inventar
   objetivos ni colorear estados como buenos/malos sin una regla aprobada.
4. Confirmar que `Geolocalizacion` representa la ubicación del cliente antes
   de titular o interpretar el mapa.
5. Definir significado, fórmula y universo de cada KPI visible. El plan no
   modifica DAX; si una cifra no representa la definición acordada, documentar
   y resolver el requisito funcional antes de rediseñar su visual.
6. Determinar si existe una necesidad de análisis de respuestas abiertas de
   NPS y cuál es el marco de privacidad.

## Matriz breve de beneficio y riesgo

| Prioridad | Cambios | Beneficio principal | Riesgo que debe controlarse |
|---|---|---|---|
| Más alto | QW-1, QW-2, QW-4, AI-1, AI-2 | Confianza, orientación y lectura inmediata | Valores por defecto, definición/unidad de KPI |
| Alto | AI-3, AI-4, AI-5, AI-7 | Comparación clara y menor redundancia | Periodos no comparables, métricas repetidas |
| Medio–alto | AI-8, AI-9, RE-1, RE-2 | Continuidad de contexto y navegación accionable | Filtros propagados e impacto en hábitos |
| Estratégico | RE-3, RE-4, RE-5, RE-6, RE-7 | Coherencia integral y análisis/diagnóstico más profundo | Aprobación de marca, geografía, benchmark y reglas de negocio |

## Plan de validación por entrega

- **Revisión de estado inicial:** abrir cada página en una sesión nueva y
  confirmar los valores preseleccionados, filtros report/page/visual y
  población mostrada.
- **Prueba de lectura de cinco segundos:** pedir a un usuario que explique
  qué mide la página, periodo y principal conclusión sin ayuda.
- **Prueba de tarea operativa:** encontrar el prestador, modalidad o zona con
  oportunidad, abrir su detalle y volver al resumen sin perder el contexto
  temporal.
- **Prueba de comparación:** validar que los periodos interanuales usen
  rangos equivalentes y las comparaciones Carry In/In Home usen iguales
  filtros.
- **QA de presentación:** revisar 1280×720, escalado de página, etiquetas,
  unidades, contraste, estados sin datos, tooltips y accesibilidad de color.
- **Conciliación:** comparar las medidas antes/después con los mismos filtros;
  un rediseño UX no debe cambiar silenciosamente el resultado.
- **Publicación gradual:** compartir primero con un grupo pequeño de usuarios,
  recoger fricciones y solo después reemplazar el reporte habitual.
