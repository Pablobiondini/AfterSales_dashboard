# Power BI Report Naming Implementation

## Alcance implementado

Se implementaron las acciones QW-2, QW-7 y QW-8 del plan UX:

- Se cambiaron los nombres visibles de las diez páginas a nombres de negocio
  en inglés.
- Se añadieron encabezados con título y subtítulo en inglés a todas las
  páginas. En las páginas vacías, el encabezado no agrega contenido analítico.
- Se habilitaron títulos en los 40 visuales de contenido existentes,
  incluidos los segmentadores y la imagen estática.
- Se reemplazaron por alias en inglés las etiquetas visibles de campos y
  medidas proyectados en los visuales. Los nombres internos de modelo,
  las fórmulas, medidas y columnas no se modificaron.
- Se mantuvo el orden actual de páginas. Este cambio se limita a
  nomenclatura y encabezados, no implementa el rediseño de navegación o de
  visuales recomendado en el plan.

Las etiquetas entre comillas y en inglés de este documento son los textos
destinados al reporte. Las justificaciones y notas de implementación están en
español.

## Nombres, objetivos y textos de página

### 1. KPI Overall → Executive Overview

**Objetivo de página (English):** Provide leaders with a concise view of
service volume, turnaround, and resolution performance.

**Subtítulo implementado (English):** “A clear view of service volume, speed,
and resolution quality.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Encabezado de página | “Executive Overview” |
| Matriz mensual de indicadores | “How Are Service Volume and Performance Changing by Month?” |
| Gráfico combinado de volumen y tasas | “Are Case Volume and Service-Level Results Moving Together?” |
| Segmentador de año | “Select a Reporting Year” |
| Segmentador de tipo de aviso | “Select a Notice Type” |
| Segmentador de provincia | “Select a Customer Province” |

**Justificación:** reemplaza el nombre técnico “KPI Overall” por una entrada
ejecutiva. Los títulos explican la lectura que se espera de la matriz y del
gráfico, y hacen explícito el propósito de cada filtro. Se tradujeron las
etiquetas visibles de mes y métricas, sin cambiar su cálculo.

### 2. KPI YoY → Year-over-Year Trends

**Objetivo de página (English):** Compare equivalent periods across years to
identify changes in case volume, turnaround, and spare-parts usage.

**Subtítulo implementado (English):** “Compare like-for-like periods to
identify changes in volume and turnaround time.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Matriz mensual de casos, TAT hábil y TAT extendido | “How Are Case Volume and Turnaround Time Changing Year over Year?” |
| Segmentador de tipo de aviso | “Select a Notice Type” |
| Segmentador de años | “Select the Years to Compare” |
| Matriz mensual de casos, repuestos y TAT calendario | “How Are Spare-Parts Usage and Calendar Turnaround Changing?” |
| Matriz anual por estado de repuestos | “How Do Service Results Compare by Spare-Parts Status?” |

**Justificación:** desarrolla la sigla `YoY` para una audiencia internacional.
Los títulos distinguen los tres análisis temporales, aunque el contenido
actual combina métricas de tendencia y de repuestos; no se movieron ni
eliminaron visuales en esta implementación.

### 3. Service Type → Service Delivery Mix

**Objetivo de página (English):** Explain how service modality relates to
monthly turnaround and five-business-day performance.

**Subtítulo implementado (English):** “Understand how service modality
influences volume, speed, and resolution.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Matriz mensual por modalidad y año | “How Do Turnaround and Five-Day Results Compare by Service Modality?” |
| Segmentador de tipo de aviso | “Select a Notice Type” |

**Justificación:** el nuevo nombre comunica una comparación de modalidades,
en vez de describir una categoría técnica. Se usa el mismo alias `Service
Modality` en la matriz para mantener consistencia entre títulos, filtros y
encabezados.

### 4. By Provider (Group) → Service Provider Ranking

**Objetivo de página (English):** Identify high-volume service providers and
compare their turnaround and resolution performance.

**Subtítulo implementado (English):** “Identify high-volume providers and
focus attention on service performance.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Tabla de centros y medidas | “Which Service Providers Have the Highest Volume and Strongest Results?” |
| Segmentador de temporada | “Select a Service Season” |
| Mapa por geolocalización de los casos | “Where Are Customers Receiving Service?” |
| Segmentador de tipo de aviso | “Select a Notice Type” |

**Justificación:** normaliza el nombre de la página y evita llamar al mapa una
ubicación de proveedores; el campo geográfico representa ubicaciones de los
casos/clientes. No se cambió el contenido del mapa, el ranking ni el orden de
la tabla.

### 5. By provider (Individual) → Provider Performance

**Objetivo de página (English):** Review one provider's case volume,
turnaround, and geographic distribution.

**Subtítulo implementado (English):** “Review an individual provider's
results and supporting location detail.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Tarjeta de casos cerrados | “How Many Cases Did This Provider Close?” |
| Tarjeta de casos dentro de 5 días hábiles | “What Share of Cases Were Closed Within 5 Business Days?” |
| Tarjeta de casos dentro de 10 días hábiles | “What Share of Cases Were Closed Within 10 Business Days?” |
| Tarjeta de TAT calendario | “What Was the Average Calendar Turnaround?” |
| Tabla por localidad | “How Do Case Volume and Turnaround Vary by Locality?” |
| Mapa del proveedor seleccionado | “Where Are This Provider's Customers Located?” |
| Segmentador de tipo de aviso | “Select a Notice Type” |
| Segmentador de prestador | “Select a Service Provider” |
| Segmentador de temporada | “Select a Service Season” |

**Justificación:** elimina la inconsistencia de mayúsculas del nombre original.
Los títulos de las tarjetas explican qué pregunta responde cada valor, y los
segmentadores describen explícitamente qué se puede seleccionar. La selección
preexistente de prestador no se cambió en esta tarea.

### 6. By Area → Geographic Performance

**Objetivo de página (English):** Locate case demand and compare service
performance across provinces, localities, and providers.

**Subtítulo implementado (English):** “Explore where service demand is
concentrated across provinces and localities.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Mapa por ubicación del cliente | “Where Are Customers Receiving Service?” |
| Segmentador de tipo de aviso | “Select a Notice Type” |
| Tabla por provincia | “Which Provinces Have the Highest Case Volume and Slowest Turnaround?” |
| Tabla por centro de servicio | “Which Service Providers Serve the Selected Area?” |
| Segmentador de temporada | “Select a Service Season” |
| Tabla por localidad | “How Do Case Volume and Turnaround Vary by Locality?” |

**Justificación:** el nombre abarca los niveles geográficos que ya están en la
página. Los títulos hacen explícita la pregunta de negocio de cada tabla y
describen el mapa como ubicación de atención al cliente, no como ubicación de
una oficina.

### 7. Customer Care → Customer Experience

**Objetivo de página (English):** Show customer sentiment and compare survey
responses across service channels.

**Subtítulo implementado (English):** “Understand customer sentiment across
the Call Center and Technical Service channels.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Tarjeta NPS de Call Center | “What Is the Call Center Net Promoter Score?” |
| Tarjeta NPS de Servicio Técnico | “What Is the Technical Service Net Promoter Score?” |
| Gráfico de columnas con ambos NPS | “How Does Net Promoter Score Compare Across Service Channels?” |
| Imagen estática del medidor | “Service Performance Gauge” |
| Dona de respuestas de Servicio Técnico | “How Are Technical Service Survey Responses Distributed?” |
| Dona de respuestas de Call Center | “How Are Call Center Survey Responses Distributed?” |

**Justificación:** “Customer Experience” presenta la página como una vista de
percepción del cliente, no como un área organizativa. Se aclararon los canales
en cada visual. La imagen estática se tituló como recurso visual existente;
no se quitó ni se alteró.

### 8. Call Center → Call Center Insights

**Objetivo de página (English):** Provide a focused view of Call Center
satisfaction, survey response themes, and improvement opportunities.

**Subtítulo implementado (English):** “Review Call Center satisfaction,
response themes, and opportunities for improvement.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Encabezado de página (página antes vacía) | “Call Center Insights” |

**Justificación:** el nombre mantiene reconocimiento del canal, pero anticipa
una lectura de insights. La página no tenía visuales; solo se agregó un
encabezado y subtítulo, sin inventar indicadores ni contenido analítico.

### 9. Spare Parts → Spare Parts Performance

**Objetivo de página (English):** Provide a focused view of spare-parts usage
by period, service modality, and provider.

**Subtítulo implementado (English):** “Understand spare-parts usage by
period, modality, and service provider.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Encabezado de página (página antes vacía) | “Spare Parts Performance” |

**Justificación:** el nuevo nombre es más específico que `Spare Parts`, pero
la página sigue sin visuales de análisis. Solo se agregó un encabezado y
subtítulo; la implementación no simula contenido que aún no existe.

### 10. FVR → First-Visit Resolution

**Objetivo de página (English):** Compare first-visit resolution and
turnaround performance across service modalities.

**Subtítulo implementado (English):** “Compare first-visit resolution and
turnaround performance across service modalities.”

**Visuales y títulos implementados (English):**

| Visual actual | Título en el reporte |
|---|---|
| Matriz mensual filtrada a Carry In | “How Do First-Visit Resolution and Turnaround Compare for Carry-In Service?” |
| Segmentador de tipo de aviso | “Select a Notice Type” |
| Matriz mensual filtrada a In Home | “How Do First-Visit Resolution and Turnaround Compare for In-Home Service?” |

**Justificación:** reemplaza la sigla no explicada por la descripción del
análisis y especifica la modalidad que distingue cada matriz. Se ajustó la
posición vertical de ambas matrices para dejar espacio al encabezado añadido;
no se cambiaron sus campos ni filtros.

## Etiquetas de encabezados visibles en inglés

Se aplicaron alias de presentación en los visuales para sustituir etiquetas
técnicas o en español. Entre ellos:

| Etiqueta actual del modelo | Etiqueta visible en el reporte |
|---|---|
| `Año` | “Reporting Year” |
| `Mes` | “Month” |
| `Clase de Aviso` | “Notice Type” |
| `Service Type` | “Service Modality” |
| `Centro de Servicio` | “Service Provider” |
| `Season` | “Service Season” |
| `Provincia del Cliente` | “Customer Province” |
| `Localidad cliente` | “Customer Locality” |
| `ConRepuestoTXT` | “Spare-Parts Status” |
| `QTY Cerrados` | “Closed Cases” |
| `TAT Laboral` | “Average Business-Day Turnaround (Days)” |
| `TAT Corridos` | “Average Calendar Turnaround (Days)” |
| `%5D` / `%10D` | “Closed Within 5 Business Days” / “Closed Within 10 Business Days” |
| `FVR(T)` / `FVR(5/10/15)` | “First-Visit Resolution Rate” / “First-Visit Resolution Within 5/10/15 Business Days” |
| `% Con Repuesto` | “Spare-Parts Rate” |
| `%Coverage Range` | “In-Range Service Rate” |
| `%TATcorr+30` | “Turnaround of 30 or More Calendar Days” |
| `%RRR-60` | “60-Day Repeat-Repair Rate” |
| `Pareto Flag` | “Top 80% Volume Group” |
| `NPS CC` / `NPS ST` | “Call Center Net Promoter Score” / “Technical Service Net Promoter Score” |
| `NPS_Clase` / `Tipo_NPS` | “Customer Sentiment” / “Survey Channel” |

Los identificadores internos de los campos permanecen intactos; por tanto,
las expresiones y el modelo no cambian. Los nombres que el panel de filtros
genere directamente desde el modelo pueden seguir mostrando su nombre de
campo original. Cambiarlos globalmente requeriría una decisión de metadatos
del modelo, fuera del alcance aplicado aquí.

Los valores de categoría que provienen de los datos también conservan su
idioma original (por ejemplo, clases de NPS o tipos de aviso). Traducir esos
valores de presentación y mantener su filtrado correcto requiere una capa de
etiquetas traducidas; no se reemplazaron valores de negocio ni se modificó
DAX o Power Query en esta tarea.

## Validaciones y observaciones

- Se verificará que las diez páginas tengan nombre y encabezado en inglés.
- Los 40 visuales de contenido reciben un título visible y los encabezados de
  datos usan alias de presentación en inglés.
- La matriz mensual existente y el gráfico combinado de `Executive Overview`
  conservan su contenido; el título los orienta, pero no elimina la
  redundancia identificada en la revisión UX.
- Los nombres de medidas, fórmulas DAX, columnas del modelo, filtros y orden
  general de páginas no se cambiaron. Excepción de maquetación necesaria: las
  dos matrices de `First-Visit Resolution` se desplazan ligeramente para dejar
  lugar al encabezado nuevo.
- Power BI Desktop debe validar visualmente los títulos largos en
  segmentadores estrechos, los dos encabezados en `First-Visit Resolution`,
  el uso de alias en tablas y el ajuste a 1280×720 antes de publicar.
