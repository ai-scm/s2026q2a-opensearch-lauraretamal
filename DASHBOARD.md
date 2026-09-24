# Dashboard - Visualizaciones y Configuración

## Dashboard creado: "Dashboard Productos y Logs"

### Resumen
Dashboard interactivo con 3 visualizaciones del índice `opensearch_dashboards_sample_data_logs` que permite explorar logs en tiempo real con filtros dinámicos.

## Visualizaciones

### 1. Total de Logs (Métrica)

**Tipo:** Metric

**Índice:** opensearch_dashboards_sample_data_logs

**Configuración:**
- Métrica: Count (conteo total de documentos)
- Agregación: Sumario de todos los documentos

**Valores:**
- Sin filtros: 2,780 documentos
- Con filtro `request: /apm`: 355 documentos

**Propósito:** Mostrar el número total de logs disponibles. Se actualiza dinámicamente cuando se aplican filtros.

---

### 2. Logs por tiempo (Gráfico de líneas)

**Tipo:** Line Chart

**Índice:** opensearch_dashboards_sample_data_logs

**Configuración:**
- Y-axis: Count (conteo de documentos)
- X-axis: Date Histogram por campo `timestamp`
- Intervalo: 30 segundos automático

**Valores:**
- Rango temporal: 2020-01-01 a 2024-01-01
- Distribución: Variable según el período

**Propósito:** Visualizar la tendencia de logs a lo largo del tiempo. Muestra picos y valles de actividad.

---

### 3. Códigos de respuesta HTTP (Gráfico de barras vertical)

**Tipo:** Vertical Bar Chart

**Índice:** opensearch_dashboards_sample_data_logs

**Configuración:**
- Y-axis: Count (conteo de documentos)
- X-axis: Terms aggregation por campo `response`

**Valores principales:**
- /apm: ~460 documentos
- /opensearch/opensearch: ~350 documentos
- /opensearch: ~280 documentos
- /apm-server/apm-server: ~200 documentos
- /beats: ~210 documentos

**Propósito:** Distribuir los logs por endpoints/recursos solicitados. Identificar qué endpoints son más frecuentes.

---

## Búsqueda facetada

### Filtros dinámicos

**Implementación:**
Los filtros se aplican a nivel de dashboard y afectan todas las visualizaciones simultáneamente.

**Ejemplo de uso:**

1. Filtro simple: `request: /apm`
   - Reduce de 2,780 a 355 documentos
   - Todas las gráficas se actualizan

2. Múltiples filtros (combinables):
   - Por request (endpoint)
   - Por response (código HTTP)
   - Por host (servidor)
   - Por rango de fecha

### Navegación

1. Haz clic en `Add filter` (arriba a la izquierda)
2. Selecciona el campo (ej: `request`)
3. Elige el operador (ej: `is`)
4. Ingresa el valor (ej: `/apm`)
5. Haz clic en `Add filter`

### Observaciones

- Los filtros se aplican instantáneamente
- Se pueden combinar múltiples filtros
- El total de logs se recalcula automáticamente
- Las visualizaciones muestran solo datos filtrados

---

## Flujo de exploración

### Caso 1: Analizar actividad de un endpoint específico
1. Agregar filtro: `request: /beats`
2. Observar total de logs: ~210
3. Ver gráfico temporal para detectar picos
4. Verificar distribución de respuestas

### Caso 2: Buscar errores (códigos 4xx, 5xx)
1. Agregar filtro: `response: 404`
2. Ver total de logs con ese código
3. Analizar en qué horarios ocurrieron
4. Identificar patrones

### Caso 3: Monitorear servidor específico
1. Agregar filtro: `host: web-server-01`
2. Observar actividad del servidor
3. Detectar anomalías en el gráfico temporal

---

## Configuración técnica

**Acceso:**
- URL: http://localhost:5601
- Dashboard: "Dashboard Productos y Logs"

**Datos:**
- Índice: opensearch_dashboards_sample_data_logs
- Documentos totales: 2,780
- Período: 2020-01-01 a 2024-01-01

**Visualizaciones guardadas:**
1. Total de Logs
2. Logs por tiempo
3. Códigos de respuesta HTTP

**Filtros:**
- Dinámicos
- Combinables
- Actualizaciones en tiempo real

---
