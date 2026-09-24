# OpenSearch - Ejercicio Práctico

## Objetivo
Implementar un entorno local de OpenSearch, cargar dataset de ejemplo, crear un dashboard interactivo con visualizaciones y configurar búsqueda facetada con filtros dinámicos.

## Fases completadas

### 1. Instalación local con Docker Compose ✓
- Configuración de 2 nodos OpenSearch
- OpenSearch Dashboards en puerto 5601
- Credenciales: admin / OpenSearch@2026
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

### 2. Ingesta de datos de ejemplo ✓

#### Dataset oficial cargado
- Índice: `opensearch_dashboards_sample_data_logs`
- Total documentos: 2,780
- Método: Instalado desde OpenSearch Dashboards UI

#### Ejemplo adicional personalizado
- Índice: `productos`
- Total documentos: 5
- Estructura:
  - nombre (text)
  - precio (float)
  - categoria (keyword)
  - stock (integer)
  - fecha_creacion (date)

**Documentos cargados:**
1. Laptop Dell XPS 13 - $1200.50 - Electrónica - Stock: 15
2. Mouse Logitech MX Master - $99.99 - Accesorios - Stock: 50
3. Monitor Samsung 4K 27 - $450.00 - Electrónica - Stock: 8
4. Teclado Mecánico RGB - $150.75 - Accesorios - Stock: 25
5. Webcam Logitech 1080p - $79.99 - Accesorios - Stock: 40

### 3. Dashboard con visualizaciones ✓

**Dashboard:** "Dashboard Productos y Logs"

**Visualizaciones creadas:**

1. **Total de Logs** (Métrica)
   - Tipo: Metric
   - Valor sin filtros: 2,780 documentos
   - Descripción: Conteo total de registros de logs

2. **Logs por tiempo** (Gráfico de líneas)
   - Tipo: Line
   - Configuración: Date Histogram por timestamp (30 segundos)
   - Descripción: Conteo de logs agrupados por intervalos de tiempo

3. **Códigos de respuesta HTTP** (Gráfico de barras vertical)
   - Tipo: Vertical Bar
   - Configuración: Terms aggregation por response
   - Descripción: Distribución de códigos de respuesta HTTP

### 4. Búsqueda facetada ✓

**Filtros dinámicos implementados:**
- Filtro ejemplo: `request: /apm`
- Resultado: Datos reducidos de 2,780 a 355 documentos
- Todas las visualizaciones se actualizan automáticamente

**Navegación:**
- Los filtros se combinan dinámicamente
- Cambios en tiempo real en el dashboard

### 5. Exploración del API REST ✓

Todas las queries ejecutadas vía:
- **curl** desde línea de comandos
- **Dev Tools** en OpenSearch Dashboards

Consulta `API-REST-QUERIES.md` para detalles completos.

## Acceso

**OpenSearch API:**
```
https://localhost:9200
Usuario: admin
Contraseña: OpenSearch@2026
```

**OpenSearch Dashboards:**
```
http://localhost:5601
Usuario: admin
Contraseña: OpenSearch@2026
```

## Estructura del repositorio

```
s2026q2a-opensearch-jia/
├── README.md                    (este archivo)
├── docker-compose.yml           (configuración de contenedores)
├── .env                         (variables de entorno)
├── API-REST-QUERIES.md          (queries REST ejecutadas)
├── DASHBOARD.md                 (documentación del dashboard)
├── DATOS-CARGADOS.md            (información de índices)
└── scripts/
    └── queries-productos.sh     (scripts de ejemplo)
```

## Requisitos previos

- Docker y Docker Compose instalados
- Terminal/bash
- Navegador web
- curl (para queries REST)

## Tiempo empleado

2-3 horas según lo estimado
