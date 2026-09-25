# OpenSearch - Ejercicio Práctico

Implementar un entorno local de OpenSearch, cargar dataset de ejemplo, crear un dashboard interactivo con visualizaciones y configurar búsqueda facetada con filtros dinámicos.

## Requisitos previos

- Docker y Docker Compose instalados
- Terminal/bash
- Navegador web
- curl (para queries REST)

### 1. Guía de Instalación - OpenSearch Local

## Paso 1: Preparar Entorno

```bash
cd C:\
mkdir opensearch-project
cd opensearch-project
```

## Paso 2: Crear docker-compose.yml

Crear archivo `docker-compose.yml` con la configuración de OpenSearch y Dashboards.

```bash
notepad docker-compose.yml
```

## Paso 3: Levantar Contenedores

```bash
docker-compose up -d
```

Esperar 2-3 minutos a que inicialice.

## Paso 4: Verificar Estado

```bash
docker ps
```

Deberías ver dos contenedores:
- `opensearch-node` (estado: Healthy)
- `opensearch-dashboards` (estado: Up)

## Paso 5: Acceder a OpenSearch Dashboards

1. Abre navegador
2. Ve a: `http://localhost:5601`
3. Usuario: `admin`
4. Contraseña: `OpenSearch@2024Secure`

## Paso 6: Crear Index Pattern

1. Management → Dashboards Management → Index patterns
2. Create index pattern
3. Name: `opensearch_dashboards_sample_data_logs`
4. Timestamp field: `timestamp`
5. Create

## Paso 7: Cargar Datos

Dev Tools → Ejecutar consultas bulk

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
