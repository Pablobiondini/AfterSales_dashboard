# Revisión UX del reporte Power BI

## Resumen ejecutivo

El proyecto tiene una base analítica aprovechable —volumen, TAT, modalidad,
prestadores, geografía, FVR y NPS—, pero hoy se siente como un conjunto de
hojas de análisis ensambladas, no como una experiencia guiada para tomar
decisiones. Los principales problemas de UX son:

1. **Diez páginas sin una narrativa clara:** hay dos páginas vacías y el orden
   alterna resumen, análisis temporal y detalle operativo.
2. **Falta de títulos en los visuales:** en la definición del reporte no
   aparecen títulos explícitos para los visuales de datos. Solo el resumen
   tiene una caja de texto con título de página. Sin encabezados, las tablas y
   gráficos dependen de nombres técnicos, etiquetas de campos o del contexto
   visual para explicar su propósito.
3. **Tablas tipo pivot dominan la experiencia:** el resumen contiene una
   matriz mensual de muchos indicadores y, debajo, un gráfico que repite
   parte de esa información. La página YoY contiene tres matrices; FVR tiene
   dos matrices casi iguales.
4. **Navegación personalizada ausente:** no se encontraron botones de
   navegación, navegación por marcadores ni controles de regreso. La
   experiencia depende de las pestañas nativas del reporte.
5. **Selecciones persistentes difíciles de interpretar:** hay valores fijos
   como año 2025, temporada 2025–2026 y el prestador Bonafede Refrigeración.
   Este último aparece preseleccionado en un segmentador de selección única.
6. **Identidad visual desigual:** la paleta del tema contiene una larga serie
   de colores saturados; algunas páginas usan fondos de marca y otras no. El
   fondo corporativo aporta identidad, pero sus muestras decorativas de color
   consumen margen útil.
7. **Páginas vacías y análisis fragmentado:** `Call Center` y `Spare Parts`
   no contienen visuales; el NPS está en `Customer Care` y los indicadores de
   repuestos aparecen en páginas distintas.

### Prioridad recomendada

- **P0 — Claridad inmediata:** agregar título y subtítulo a cada página,
  nombrar cada visual, hacer visibles los filtros activos y eliminar las
  selecciones accidentales o fijas no intencionales.
- **P1 — Reducir redundancia:** sustituir tablas repetidas por tendencias,
  KPI destacados y jerarquías de detalle; convertir las páginas vacías en
  análisis útiles o retirarlas.
- **P2 — Guiar el recorrido:** agregar navegación ejecutiva/operativa,
  botones de regreso y filtros sincronizados donde el significado sea común.
- **P3 — Normalizar diseño:** aplicar una cuadrícula consistente, paleta
  semántica y reglas de formato para porcentajes, días, volumen y alertas.

## Alcance y límites

La revisión cubre las diez páginas en el orden del proyecto, campos
proyectados por cada visual, títulos configurados, geometría, filtros,
segmentadores, metadatos de navegación y tema visual. No evalúa DAX ni
recalcula resultados.

El informe se basa en la definición PBIR estática del proyecto. No se renderizó
el archivo en Power BI Desktop/Service, así que la legibilidad efectiva,
etiquetas automáticas, interacciones y comportamiento en distintos tamaños de
pantalla deben validarse en una sesión de Power BI antes de rediseñar la
maquetación final.

## Hallazgos globales de diseño

### Páginas, narrativa y nombres

El orden actual es: `KPI Overall`, `KPI YoY`, `Service Type`,
`By Provider (Group)`, `By provider (Individual)`, `By Area`,
`Customer Care`, `Call Center`, `Spare Parts`, `FVR`. La secuencia empieza
bien con un resumen, pero luego salta entre dimensiones y conserva dos hojas
vacías casi al final. La mezcla de nombres en inglés y español, mayúsculas
inconsistentes (`By Provider` / `By provider`) y siglas sin contexto (`FVR`,
`YoY`) aumenta el costo de orientación.

**Convención propuesta:** nombres cortos en español, orientados a la pregunta
que resuelve la página y sin siglas no explicadas:

- Resumen ejecutivo
- Tendencia interanual
- Modalidad de servicio
- Ranking de prestadores
- Ficha del prestador
- Desempeño geográfico
- Voz del cliente
- Call Center | Voz y motivos
- Uso de repuestos
- Efectividad a la primera visita

Las dos últimas páginas vacías no deberían publicarse vacías. Si se decide no
desarrollar un análisis diferenciado, se deben quitar del recorrido y
consolidar su contenido en las páginas temáticas correspondientes.

### Títulos y jerarquía de información

En los metadatos estáticos, los visuales de datos no tienen título
explícitamente configurado. La única excepción visible es el cuadro de texto
`After Sales - KPI Overall`. La página necesita comunicar tres niveles:

1. **Qué estoy viendo:** título de página claro y descriptivo.
2. **Qué debo notar:** KPI principal o mensaje de lectura.
3. **Cómo lo investigo:** tendencia, desglose y tabla de detalle.

No se debe usar el título como una repetición del nombre del campo. Ejemplo:
“Prestadores con mayor volumen de casos cerrados” es más informativo que
“QTY Cerrados”. En KPI, añadir unidad y periodo: `días hábiles`, `%` y
`casos`.

### Organización y densidad

Las páginas son 1280×720 con `FitToPage`. Hay tablas con muchos campos y
controles de filtro angostos o altos; si la vista se escala, es probable que
los encabezados y valores pierdan legibilidad. La matriz de resumen llega a
ocupar la mayor parte del ancho y la altura de contenido; las tablas de FVR
están apiladas y ocupan casi toda la altura. Se recomienda:

- Reservar una franja superior estable para título, periodo y filtros.
- Mostrar entre tres y cinco KPI de primer nivel, no una tabla de métricas
  como sustituto de tarjetas.
- Usar una cuadrícula de dos columnas para tendencia y explicación, dejando
  el detalle tabular al final o en una página de drillthrough.
- Priorizar etiquetas cortas, unidades explícitas y tipografía legible.
- No usar el mapa como decoración: debe responder una pregunta geográfica y
  tener espacio suficiente para explorar.

### Color, filtros y navegación

- El tema usa una paleta de muchos colores intensos. Reservar **azul oscuro /
  azul de marca** para series principales, grises para contexto y **verde,
  ámbar y rojo** solo para estados o cumplimiento. Evitar que una serie
  cambie de color entre páginas.
- Para `Carry In` e `In Home`, fijar siempre los mismos dos colores. Para NPS,
  fijar explícitamente detractor = rojo, neutral = ámbar y promotor = verde,
  con contraste y etiquetas que no dependan solo del color.
- Los filtros globales deberían estar juntos y en una posición repetible:
  periodo, tipo de aviso, modalidad y zona. Los filtros propios (prestador,
  temporada) deben estar dentro del contexto de su página.
- El filtro de visitas técnicas válidas es global al reporte, pero no hay un
  indicador visible que explique esa población. Mostrar una etiqueta
  informativa como **“Población: visitas técnicas válidas”** y permitir que
  el usuario consulte los filtros activos.
- Revisar antes de publicar los valores fijos observados: año `2025` en el
  resumen, temporada `2025–2026` en páginas de prestador y un prestador
  específico en la ficha individual. Si son estados de muestra, quitarlos; si
  son intencionales, comunicarlos en el título/subtítulo y proporcionar un
  botón de restablecimiento.
- No se encontró sincronización explícita de segmentadores ni navegación
  personalizada. Sincronizar filtros comunes cuando el mismo periodo o tipo
  de aviso deba persistir entre páginas; no sincronizar filtros de análisis
  que deban variar independientemente.
- Incorporar una navegación superior o lateral con `Resumen`, `Tendencias`,
  `Prestadores`, `Geografía` y `Calidad`; agregar `Inicio` y `Volver` en las
  páginas de detalle. Evitar que una lista larga de pestañas sea el único
  mapa del reporte.

## Revisión por página

### 1. KPI Overall → **Resumen ejecutivo**

**Qué está bien:** es la página inicial; tiene periodo, tipo de aviso y
provincia como filtros y combina volumen mensual con indicadores operativos.
El orden de sus elementos comienza con un encabezado.

**Qué debería cambiar:** la matriz mensual reúne TAT hábil, TAT calendario,
casos, umbrales, FVR, repuestos, cobertura y repetición. Es un detalle de
consulta, no una vista ejecutiva. El gráfico combinado repite volumen,
`%5D` y `FVR(10)` de esa matriz, y mezcla conteos y porcentajes en ejes
distintos. Los tres segmentadores están dispersos a la derecha y tienen
espacios desiguales. La selección inicial del año es `2025`; debe validarse
frente al periodo vigente.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Caja de texto “After Sales - KPI Overall” | **Postventa | Resumen ejecutivo** | Conservar como encabezado; añadir periodo seleccionado y una frase ejecutiva breve. |
| Matriz mensual con 10 medidas | **Detalle mensual de indicadores** | Mover a `Tendencia interanual` o a una vista de detalle. No usarla como elemento principal del resumen. |
| Combinado de casos por clase, `%5D` y `FVR(10)` | **Volumen de casos y cumplimiento mensual** | Conservar solo si se rediseña para separar volumen y tasa con escalas claras; reducir series y explicar las dos métricas de cumplimiento. |
| Segmentador de año | **Año / periodo** | Mover a la franja global superior; eliminar o renovar selección fija `2025` según periodo publicado. |
| Segmentador de clase de aviso | **Tipo de aviso** | Mover junto al periodo y sincronizar con páginas que deban compartirlo. |
| Segmentador de provincia | **Provincia** | Ubicar después de filtros globales o sustituirlo por una selección geográfica coherente con la página. |

**Qué eliminaría:** la matriz de 10 métricas de esta página de entrada; no
eliminar sus datos, sino trasladarlos a análisis temporal.

**Qué agregaría:** cuatro tarjetas en primer nivel: **Casos cerrados**,
**TAT hábil promedio (días)**, **Cumplimiento ≤5 días hábiles** y
**First Time Fix**. Debajo, una tendencia mensual de volumen y una tasa de
cumplimiento con referencia/objetivo si existe un objetivo de negocio
aprobado. Añadir una señal de variación vs. periodo anterior, solo si el
periodo de comparación es equivalente.

**Qué movería:** periodo, tipo de aviso y provincia a una misma fila superior.
La matriz existente pasa a una página de detalle temporal.

**KPI a destacar:** Casos cerrados junto con TAT hábil y cumplimiento ≤5 días;
son lectura rápida de escala y velocidad. Coverage debe quedar en el bloque
geográfico, no competir por el primer lugar sin contexto.

### 2. KPI YoY → **Tendencia interanual**

**Qué está bien:** compara valores por año y mes y ofrece filtros de año y
tipo de aviso, apropiados para una lectura temporal.

**Qué debería cambiar:** contiene tres matrices en lugar de una comparación
visual clara. Dos matrices mensuales repiten `QTY Cerrados` y TAT; una tercera
desglosa por año y estado de repuesto, aunque el nombre “YoY” no anticipa ese
análisis. Hay que diferenciar año completo, año parcial y meses comparables:
comparar un año completo con un año en curso puede inducir a una conclusión
errónea.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Matriz por año/mes con casos, TAT hábil y % TAT >30 días | **Volumen y TAT por mes — comparación interanual** | Sustituir por líneas mensuales por año, con selección de métrica y aviso de año parcial. |
| Matriz por año/mes con casos, repuestos y TAT calendario | **Uso de repuestos y TAT calendario por mes** | Mover a `Uso de repuestos` o retirar de esta página para evitar duplicación. |
| Matriz por año y condición de repuesto con seis medidas | **Indicadores según uso de repuestos** | Mover completa a `Uso de repuestos`; no es una comparación YoY limpia. |
| Segmentador de tipo de aviso | **Tipo de aviso** | Sincronizar con la selección global, si se mantiene como filtro común. |
| Segmentador de año | **Años comparados** | Permitir seleccionar dos años comparables; mostrar claramente el periodo incompleto si aplica. |

**Qué eliminaría:** las matrices duplicadas como elemento principal; una tabla
de detalle puede conservarse abajo, pero no tres tablas con métricas que se
repiten.

**Qué agregaría:** visual de líneas por mes con años en color consistente,
tarjeta de variación interanual para casos y TAT, y una nota automática de
comparabilidad (por ejemplo, “enero–septiembre vs. enero–septiembre”). La
comparación de meses debe controlar el mismo rango de fechas.

**Qué movería:** el desglose por condición de repuesto a `Uso de repuestos`;
los datos mensuales del resumen a esta página.

**KPI a destacar:** variación interanual de **Casos cerrados** y
**TAT hábil promedio**, con mismo periodo comparable.

### 3. Service Type → **Modalidad de servicio**

**Qué está bien:** la matriz separa los dos tipos de servicio y ofrece año y
mes como dimensiones; el filtro de tipo de aviso es pertinente.

**Qué debería cambiar:** el visual único con cinco dimensiones/valores ocupa
solo una fracción del lienzo, y el segmentador queda aislado debajo. La página
no explica la diferencia de Carry In frente a In Home ni convierte esa
comparación en una conclusión operativa.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Matriz mensual por modalidad/año con TAT hábil y `%5D` | **TAT y cumplimiento por modalidad y mes** | Conservar como detalle secundario; limitar meses/años a un rango legible. |
| Segmentador de clase de aviso | **Tipo de aviso** | Llevar a la fila común de filtros. |

**Qué eliminaría:** nada imprescindible; simplificaría la matriz antes de
añadir más detalle.

**Qué agregaría:** dos tarjetas o barras comparativas —Carry In e In Home—
para volumen, TAT hábil y cumplimiento ≤5 días; un gráfico de diferencia
mensual. Usar siempre los mismos colores de modalidad. Mostrar una breve
definición de las modalidades.

**Qué movería:** el segmentador hacia el encabezado compartido; tarjetas antes
de la matriz.

**KPI a destacar:** **brecha de TAT hábil y cumplimiento entre modalidades**,
junto al volumen de cada una.

### 4. By Provider (Group) → **Ranking de prestadores**

**Qué está bien:** la tabla está ordenada por volumen descendente e incluye
volumen, cumplimiento, TAT, FVR, repuestos y una etiqueta Pareto. La intención
de detectar prestadores de alto volumen está presente.

**Qué debería cambiar:** la tabla mezcla ocho columnas y no da un orden visual
claro entre KPI y dimensión. La etiqueta `Pareto Flag` por sí sola no explica
la concentración ni cuánto aporta cada prestador. El mapa usa
`Geolocalizacion` de los casos/clientes, no una ubicación explícita del
prestador; el título debería evitar presentarlo como mapa de prestadores. Los
dos filtros laterales tienen anchuras muy pequeñas; la temporada tiene
selección fija `2025–2026`.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Tabla por centro con ocho valores | **Prestadores ordenados por volumen y cumplimiento** | Mantener como ranking; priorizar casos, TAT hábil, cumplimiento ≤5 días y FVR. Mover repuestos/Pareto a columnas secundarias o tooltip. |
| Segmentador de temporada | **Temporada** | Mover a filtros superiores; quitar o confirmar valor fijo. |
| Mapa por geolocalización | **Distribución de clientes atendidos** | Reducir o mover a `Desempeño geográfico`; no etiquetar como localización de prestadores. |
| Segmentador de tipo de aviso | **Tipo de aviso** | Ampliar o convertir en desplegable en la fila superior. |

**Qué eliminaría:** el mapa de esta página si solo repite la distribución de
casos; no aporta ranking del proveedor con la semántica actual.

**Qué agregaría:** barras horizontales de Top prestadores por volumen con un
marcador/color de cumplimiento; gráfico Pareto con acumulado y corte 80%,
si es una regla de negocio vigente. Agregar un tooltip informativo con los
indicadores completos en vez de exponer ocho columnas al mismo tiempo.

**Qué movería:** mapa a `Desempeño geográfico`; temporada y tipo de aviso a
una zona de filtros ordenada.

**KPI a destacar:** **prestadores que concentran el mayor volumen** y su
**cumplimiento ≤5 días**. Volumen y calidad deben verse juntos para no
premiar volumen aislado.

### 5. By provider (Individual) → **Ficha del prestador**

**Qué está bien:** cuatro tarjetas resumen volumen, dos umbrales de
cumplimiento y TAT calendario; la tabla y el mapa permiten bajar al nivel de
localidad.

**Qué debería cambiar:** el segmentador de centro está alto y estrecho, y
lleva preseleccionado **Bonafede Refrigeración** con selección estricta
única. Sin un título dinámico, ese estado puede pasar desapercibido y hacer
que se interpreten los KPI como globales. La temporada también parte de
`2025–2026`. El mapa domina el centro mientras la tabla de localidades muestra
parte de la misma información. El filtro de clase de aviso está separado de
los otros.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Tarjeta `QTY Cerrados` | **Casos cerrados del prestador** | Conservar; mostrar el prestador seleccionado en el encabezado. |
| Tarjeta `%5D` | **Casos cerrados en ≤5 días hábiles** | Conservar, con formato porcentual y descripción breve del denominador. |
| Tarjeta `TAT Corridos` | **TAT calendario promedio (días)** | Conservar si la métrica es necesaria; dar prioridad visual a TAT hábil si el objetivo es operativo. |
| Tarjeta `%10D` | **Casos cerrados en ≤10 días hábiles** | Conservar como indicador secundario o mover a tooltip para reducir duplicación con ≤5 días. |
| Mapa por geolocalización | **Distribución de casos por localidad** | Conservar solo si permite seleccionar zonas accionables; reducir para dar espacio al detalle. |
| Tabla por localidad | **Detalle de casos y TAT por localidad** | Conservar como tabla de detalle; filtrar con selección del mapa o habilitar drillthrough. |
| Segmentador de prestador | **Seleccionar prestador** | Hacerlo desplegable, visible en encabezado y quitar el prestador fijo como valor inicial salvo que sea un ejemplo explícito. Mantener selección única si la página exige una ficha. |
| Segmentador de temporada | **Temporada** | Mover a la zona de filtros; revisar valor por defecto. |
| Segmentador de tipo de aviso | **Tipo de aviso** | Ubicar junto a temporada. |

**Qué eliminaría:** uno entre mapa y tabla si en la validación en vivo ambos
solo permiten responder “dónde hay casos”; no mantener ambos por decoración.

**Qué agregaría:** título dinámico “Prestador: [selección]”, comparación con
promedio/red de prestadores en el mismo periodo y, si existe un objetivo
validado, un indicador claro de meta. Agregar enlace `Volver al ranking`.

**Qué movería:** el selector de prestador al encabezado y las cuatro tarjetas
a una fila horizontal inmediatamente debajo.

**KPI a destacar:** **Casos cerrados** y **cumplimiento ≤5 días** frente al
promedio de la red; TAT en días como contexto.

### 6. By Area → **Desempeño geográfico**

**Qué está bien:** presenta tres niveles útiles —provincia, localidad y
centro—, un mapa y filtros de temporada/tipo de aviso.

**Qué debería cambiar:** hay tres tablas separadas con conjuntos de medidas
casi iguales y una geografía por mapa. Esto fuerza al usuario a mantener el
contexto entre paneles y repite casos/TAT/umbrales. El mapa de 230 px es
estrecho, la tabla de provincia ocupa mucho alto y los dos segmentadores están
en una columna delgada.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Tabla por provincia | **Casos y cumplimiento por provincia** | Integrar como primer nivel de una matriz jerárquica; eliminar la tabla separada. |
| Tabla por localidad | **Detalle por localidad** | Integrar como segundo nivel expandible de la misma matriz. |
| Tabla por centro de servicio | **Prestadores de la zona seleccionada** | Integrar como tercer nivel o abrir por drillthrough; eliminar tabla duplicada. |
| Mapa | **Casos atendidos por ubicación del cliente** | Conservar con mayor espacio si la geografía es confiable y aporta selección; aclarar qué representa el punto. |
| Segmentador de temporada | **Temporada** | Subir junto a filtros globales y retirar selección fija si no es intencional. |
| Segmentador de tipo de aviso | **Tipo de aviso** | Alinear con temporada y con las demás páginas. |

**Qué eliminaría:** las dos tablas de nivel localidad/centro como visuales
independientes, una vez integrada la jerarquía en una matriz con expandir/
contraer.

**Qué agregaría:** matriz Provincia → Localidad → Prestador con casos,
cumplimiento y TAT; barra Top zonas por volumen o incumplimiento. Incluir
`Coverage` si la decisión geográfica es cobertura, con un mapa/indicador
explícito `En Radio`.

**Qué movería:** mapa a la derecha con anchura suficiente; filtros a la parte
superior; tabla jerárquica a la izquierda.

**KPI a destacar:** **Casos cerrados por zona** y la zona con mayor
incumplimiento; coverage debe destacarse si define una acción territorial.

### 7. Customer Care → **Voz del cliente**

**Qué está bien:** presenta NPS de Call Center y Servicio Técnico, dos
distribuciones de clase de NPS y una comparación entre los dos valores.

**Qué debería cambiar:** el gráfico de columnas compara `NPS CC` y `NPS ST`,
pero las dos tarjetas ya muestran exactamente esos indicadores; sin periodo ni
tendencia, el gráfico duplica las tarjetas. La imagen corporativa aparece
además del fondo de marca y ocupa espacio. Los gráficos de dona no tienen
títulos explícitos, por lo que no es obvio cuál corresponde a cada canal.
Falta mostrar el número de respuestas para contextualizar la representatividad.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Tarjeta `NPS ST` | **NPS — Servicio Técnico** | Conservar como KPI principal y mostrar el periodo. |
| Tarjeta `NPS CC` | **NPS — Call Center** | Conservar como KPI principal y mostrar el periodo. |
| Dona filtrada a Servicio Técnico | **Distribución de respuestas — Servicio Técnico** | Conservar; fijar colores semánticos de detractor/neutral/promotor y mostrar porcentaje y cantidad. |
| Dona filtrada a Call Center | **Distribución de respuestas — Call Center** | Conservar con la misma paleta y orden de categorías. |
| Gráfico de columnas NPS CC/NPS ST | **Comparación de NPS por canal** | Eliminar en su forma actual por redundancia con las tarjetas. Reemplazar solo si añade una tendencia temporal. |
| Imagen de marca | — | Eliminar: los fondos de página ya incluyen logotipos. |

**Qué eliminaría:** gráfico de columnas de NPS actual e imagen de marca
redundante.

**Qué agregaría:** tarjetas de volumen de respuestas por canal; tendencia
mensual/por periodo de NPS con una línea de referencia, si la serie temporal
de respuestas está correctamente disponible. Mostrar desglose en barras
apiladas al 100% puede ser más comparable que dos donas.

**Qué movería:** tarjetas a la primera fila, distribución y tendencia debajo;
si `Call Center` se conserva como detalle, agregar drillthrough a esa página.

**KPI a destacar:** **NPS de Servicio Técnico y NPS de Call Center**, siempre
junto al volumen de respuestas del periodo.

### 8. Call Center → **Call Center | Voz y motivos**

**Qué está bien:** el nombre anticipa un canal concreto que puede requerir
seguimiento diferenciado.

**Qué debería cambiar:** la página no contiene ningún visual. Una pestaña
vacía crea la expectativa de información y rompe la navegación.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Página vacía | **Call Center | Voz y motivos** | No tiene visuales que titular; no publicarla vacía. |

**Qué eliminaría:** la página del recorrido si no se aprueba un análisis
propio de Call Center; integrar su NPS y distribución en `Voz del cliente`.

**Qué agregaría si se mantiene:** NPS, respuestas recibidas, distribución de
promotores/neutrales/detractores, tendencia temporal y principales temas de
mejora/destacados provenientes de las respuestas. Evitar crear una segunda
versión de las mismas donas de `Voz del cliente`.

**Qué movería:** desde `Voz del cliente`, un botón de detalle a esta página;
añadir `Volver a Voz del cliente`.

**KPI a destacar:** **NPS Call Center** con número de respuestas y proporción
de detractores.

### 9. Spare Parts → **Uso de repuestos**

**Qué está bien:** el tema aparece como indicador en el resumen y en el
análisis YoY, por lo que hay una señal existente de uso de repuestos.

**Qué debería cambiar:** la página no contiene visuales; al mismo tiempo,
indicadores de repuestos están distribuidos en `KPI Overall`, `KPI YoY` y la
tabla de prestadores. Esto fragmenta una pregunta de negocio y hace que el
usuario tenga que recordar dónde está cada desglose.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Página vacía | **Uso de repuestos** | No tiene visuales que titular; mover aquí los desgloses que ya existen o retirar la pestaña. |

**Qué eliminaría:** página vacía si no se va a desarrollar un análisis
accionable.

**Qué agregaría si se mantiene:** tarjeta de porcentaje con repuesto y
cantidad de casos; tendencia temporal; barras por modalidad y prestador, con
orden descendente. Usar filtros de temporada, tipo de aviso y prestador con
la misma experiencia del resto del reporte.

**Qué movería:** matriz de YoY por condición de repuesto desde `Tendencia
interanual`; columna de repuestos del ranking a tooltip o detalle de esta
página.

**KPI a destacar:** **porcentaje y cantidad de casos con repuesto**. No
mostrar el porcentaje aislado de su volumen base.

### 10. FVR → **Efectividad a la primera visita**

**Qué está bien:** ambas matrices comparan `Carry In` e `In Home` con el
mismo conjunto de métricas (TAT hábil, umbral de cinco días y FVR total/5/10/15).
La separación por modalidad puede hacer visible una diferencia operativa.

**Qué debería cambiar:** las dos matrices están apiladas, casi llenan la
altura disponible y repiten encabezados, medidas y dimensiones. Los filtros
por modalidad están aplicados a cada visual (uno `Carry In`, otro `In Home`)
pero el título no explica la división. La página solo tiene un segmentador
angosto de clase de aviso, no un título de página visible.

| Elemento actual | Título propuesto | Acción UX |
|---|---|---|
| Matriz superior, filtrada a Carry In | **Carry In — efectividad y TAT mensual** | Sustituir con comparación por modalidad en un solo visual, o mover a una matriz de detalle con encabezado explícito. |
| Matriz inferior, filtrada a In Home | **In Home — efectividad y TAT mensual** | Igual que la superior; no mantener como una repetición vertical sin título. |
| Segmentador de clase de aviso | **Tipo de aviso** | Llevar a la fila común de filtros y sincronizar cuando corresponda. |

**Qué eliminaría:** una de las dos matrices como visual independiente; no
eliminar la modalidad, sino comparar ambos servicios en un único objeto o
small multiples con escalas y periodos idénticos.

**Qué agregaría:** tarjetas de FVR total y FVR ≤5/10/15, una tendencia mensual
por modalidad y una matriz compacta de apoyo. Definir en una nota la métrica
“resuelto en la primera visita” y la población incluida.

**Qué movería:** filtros al encabezado; KPI a la primera fila; tendencia
comparativa debajo; tabla detallada al final o a drillthrough.

**KPI a destacar:** **FVR total** y diferencia entre Carry In e In Home;
mostrar cantidad de casos junto al porcentaje.

## Navegación propuesta

### Recorrido gerencial

1. **Resumen ejecutivo** — escala del negocio y estado de KPI.
2. **Tendencia interanual** — dirección, estacionalidad y comparación justa.
3. **Desempeño geográfico** — dónde se concentra volumen y brechas.
4. **Ranking de prestadores** — quién explica el resultado y dónde actuar.
5. **Modalidad de servicio** — comparar Carry In e In Home.
6. **Efectividad a la primera visita** — calidad de resolución.
7. **Voz del cliente** — resultado percibido y tamaño de muestra.
8. **Uso de repuestos** — análisis de costo/consumo operativo si se desarrolla.

`Ficha del prestador` y `Call Center | Voz y motivos` son páginas de detalle,
abiertas desde botones/drillthrough, no pasos obligatorios del recorrido
ejecutivo. Si la página de repuestos queda sin contenido accionable, retirarla
de la navegación hasta completarla.

### Recorrido operativo

1. **Resumen ejecutivo** — detectar periodo o indicador fuera de expectativa.
2. **Ranking de prestadores** — localizar prestador con mayor volumen o brecha.
3. **Ficha del prestador** — seleccionar el proveedor y profundizar.
4. **Desempeño geográfico** — localizar concentración por provincia/localidad.
5. **Modalidad de servicio** — separar tipos de operación.
6. **Efectividad a la primera visita** — investigar FVR y TAT.
7. **Uso de repuestos** — revisar los casos que requieren repuesto.
8. **Voz del cliente** — consultar señal de satisfacción asociada.

El orden físico de pestañas no debería ser la única forma de implementar los
dos recorridos. Usar botones o una portada con dos entradas (“Vista ejecutiva”
y “Vista operativa”), indicadores de página actual y un botón `Volver al
resumen`. Mantener el mismo filtro de periodo al navegar y señalar cualquier
filtro específico activo.

## Versión ideal del reporte

### A. Encabezado y filtros persistentes

- Encabezado uniforme con nombre del reporte, página, periodo activo y
  población de datos.
- Una banda de filtros compacta: periodo, tipo de aviso, modalidad y
  geografía. `Temporada` puede reemplazar o complementar el periodo si el
  calendario estacional es la convención aprobada.
- Botones claros para restablecer filtros y volver al resumen.
- Mostrar chips de filtros activos, especialmente prestador, provincia,
  modalidad y año. El selector de prestador debe distinguir claramente
  “Todos” de un proveedor seleccionado.
- Mantener visible que el reporte se limita a visitas técnicas válidas.

### B. Resumen ejecutivo

Fila superior de KPI: casos cerrados, TAT hábil, cumplimiento ≤5 días y FVR.
Cada tarjeta indica periodo, unidad y variación contra un periodo comparable.
Fila media: volumen por mes y cumplimiento por mes; dos visuales separados o
una composición con ejes muy explícitos. Fila inferior: tres alertas/insights
accionables —zona, prestador y modalidad con mayor oportunidad— con botones
hacia el detalle.

### C. Tendencia y causas

Una tendencia interanual por mes con selección de medida y años comparables,
aviso visible para año parcial y tabla de detalle bajo demanda. El desglose por
repuestos sale de esta página y pasa a `Uso de repuestos`.

### D. Desempeño operativo

`Ranking de prestadores` empieza con barras de Top prestadores por volumen y
cumplimiento, luego Pareto real (volumen y acumulado) y tabla ordenable con
menos columnas. El drillthrough abre `Ficha del prestador`, cuyo encabezado
dinámico muestra el proveedor, KPI, comparación con la red y detalle por
localidad.

### E. Geografía y modalidad

`Desempeño geográfico` usa una matriz jerárquica Provincia → Localidad →
Prestador junto a un mapa espacioso que identifica inequívocamente si muestra
ubicación del cliente o del servicio. `Modalidad de servicio` compara Carry In
e In Home con las mismas medidas, escalas, colores y periodos.

### F. Calidad y experiencia

`Efectividad a la primera visita` centra FVR, volumen base y TAT por modalidad,
con una tendencia compacta y sin dos tablas apiladas idénticas. `Voz del
cliente` muestra NPS y número de respuestas, seguido de distribución y
tendencia. La página de Call Center solo se mantiene como detalle si ofrece
motivos/respuestas y no duplica el resumen de NPS.

### G. Repuestos

`Uso de repuestos` combina porcentaje, casos y tendencia; compara modalidad y
prestador. Si la información disponible no respalda una decisión nueva, es
mejor incorporar un único visual de repuestos en `Desempeño operativo` y no
conservar una página temática vacía.

### H. Sistema visual

- Fondo claro, contenido en tarjetas discretas y márgenes comunes; evitar
  decoraciones que ocupen zonas de datos.
- Color principal de marca para volumen/serie seleccionada; neutros para
  comparadores; verde/ámbar/rojo para desempeño relativo a reglas aprobadas.
- Colores constantes para modalidades y clases de NPS en todas las páginas.
- Títulos que declaren dimensión, métrica y periodo; subtítulos que expliquen
  la población o comparación.
- Unidades y formatos constantes: días hábiles vs. calendario, porcentaje,
  casos, separadores de miles y precisión decimal controlada.
- Tablas para buscar/consultar detalle; gráficos para comparar tendencias,
  rankings y composición.
- Tooltips informativos y drillthrough para profundidad, en vez de colocar
  todos los campos en la página inicial.

## Decisiones que requieren validación del negocio

1. ¿El reporte debe iniciar en el año calendario o en la temporada
   `abril–marzo`?
2. ¿Carry In e In Home son las únicas modalidades que deben compararse y cuál
   debe ser el orden/color permanente?
3. ¿Existe una meta oficial para TAT/FVR/cumplimiento que permita mostrar
   semáforos y brechas, o se debe limitar a comparación histórica?
4. ¿La página `Call Center` requiere análisis de comentarios/temas separado o
   basta con la comparación de canales de `Voz del cliente`?
5. ¿La geolocalización representa domicilio del cliente, ubicación de
   servicio u otro punto? El título y uso del mapa deben reflejarlo.
6. ¿La selección fija de Bonafede Refrigeración y las temporadas/años
   predeterminados son intencionales o estados de edición que deben limpiarse?
