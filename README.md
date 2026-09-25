# OpenSearch - Ejercicio Práctico

Implementar un entorno local de OpenSearch, cargar dataset de ejemplo, crear un dashboard interactivo con visualizaciones y configurar búsqueda facetada con filtros dinámicos.

## Requisitos previos

- Docker y Docker Compose instalados
- Terminal/bash
- Navegador web
- curl (para queries REST)

### 1. Instalación local con Docker Compose
- Configuración de 2 nodos OpenSearch
- OpenSearch Dashboards en puerto 5601
- Cluster name: opensearch-cluster

**Levantar entorno:**
```bash
docker compose up -d
```

**Verificar:**
```bash
docker compose ps
curl -u admin:OpenSearch@2026 -k https://localhost:9200
```

### 2. Ingesta de datos de ejemplo

#### Dataset oficial cargado
- Índice: `opensearch_dashboards_sample_data_logs`
- Total documentos: 2,780
- Método: Instalado desde OpenSearch Dashboards UI

#### Ejemplo adicional personalizado
- Índice: `ecommerce`
- Total documentos: 15

### 3. Dashboard con visualizaciones

**Dashboard:** "Dashboard Productos y Logs"

**Visualizaciones creadas:**

1. **Distribución de Respuestas HTTP** (Pie Chart)
2. **Heatmap Actividad por Hora y Host**
3. **Respuestas por Código HTTP** (Metric)
4. **Tabla de Hosts y Solicitudes** (Data Table)

### 4. Búsqueda facetada

**Filtros dinámicos implementados:**
Dashboard interactivo con filtros dinámicos 
