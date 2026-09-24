# Datos cargados en OpenSearch

## Resumen

Se cargaron dos conjuntos de datos:

1. **Dataset oficial** - opensearch_dashboards_sample_data_logs
2. **Ejemplo personalizado** - productos

---

## Dataset Oficial: opensearch_dashboards_sample_data_logs

### Información general

**Índice:** opensearch_dashboards_sample_data_logs

**Total documentos:** 2,780

**Método de carga:** OpenSearch Dashboards UI (Sample data)

### Estructura (Mapping)

```json
{
  "properties": {
    "timestamp": {"type": "date"},
    "host": {"type": "keyword"},
    "response": {"type": "integer"},
    "bytes": {"type": "integer"},
    "request": {"type": "text"},
    "agent": {"type": "text"},
    "clientip": {"type": "keyword"},
    "geo": {
      "properties": {
        "coordinates": {"type": "geo_point"},
        "dest": {"type": "keyword"},
        "src": {"type": "keyword"},
        "srcdest": {"type": "keyword"}
      }
    }
  }
}
```

### Campos principales

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| timestamp | date | Fecha/hora del evento | 2026-09-24T10:00:00Z |
| host | keyword | Servidor origen | web-server-01 |
| response | integer | Código HTTP | 200, 404, 500 |
| bytes | integer | Bytes transferidos | 1234 |
| request | text | Endpoint solicitado | /apm, /beats, /opensearch |
| clientip | keyword | IP del cliente | 213.118.26.4 |
| agent | text | User-Agent | Mozilla/5.0 |

### Queries útiles

**Contar documentos:**
```bash
curl -u admin:OpenSearch@2026 -k "https://localhost:9200/opensearch_dashboards_sample_data_logs/_count"
```

**Obtener 1 documento:**
```bash
curl -u admin:OpenSearch@2026 -k "https://localhost:9200/opensearch_dashboards_sample_data_logs/_search?size=1"
```

**Ver mapping:**
```bash
curl -u admin:OpenSearch@2026 -k "https://localhost:9200/opensearch_dashboards_sample_data_logs/_mapping"
```

**Agrupar por host:**
```
GET opensearch_dashboards_sample_data_logs/_search
{
  "size": 0,
  "aggs": {
    "hosts": {
      "terms": {"field": "host"}
    }
  }
}
```

---

## Ejemplo Personalizado: productos

### Información general

**Índice:** productos

**Total documentos:** 5

**Método de carga:** API REST con curl

### Estructura (Mapping)

```json
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  },
  "mappings": {
    "properties": {
      "nombre": {"type": "text"},
      "precio": {"type": "float"},
      "categoria": {"type": "keyword"},
      "stock": {"type": "integer"},
      "fecha_creacion": {"type": "date"}
    }
  }
}
```

### Documentos

| ID | Nombre | Precio | Categoría | Stock | Fecha |
|----|--------|--------|-----------|-------|-------|
| 1 | Laptop Dell XPS 13 | $1200.50 | Electrónica | 15 | 2026-01-15 |
| 2 | Mouse Logitech MX Master | $99.99 | Accesorios | 50 | 2026-01-20 |
| 3 | Monitor Samsung 4K 27 | $450.00 | Electrónica | 8 | 2026-02-01 |
| 4 | Teclado Mecánico RGB | $150.75 | Accesorios | 25 | 2026-02-10 |
| 5 | Webcam Logitech 1080p | $79.99 | Accesorios | 40 | 2026-02-15 |

### Estadísticas

**Por categoría:**
- Accesorios: 3 productos (60%)
- Electrónica: 2 productos (40%)

**Por precio:**
- Mínimo: $79.99
- Máximo: $1200.50
- Promedio Accesorios: $110.24
- Promedio Electrónica: $825.25

**Por stock:**
- Total unidades: 138
- Promedio: 27.6 por producto

### Queries útiles

**Contar documentos:**
```bash
curl -u admin:OpenSearch@2026 -k "https://localhost:9200/productos/_count"
```

**Filtrar por categoría:**
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/productos/_search" \
-H "Content-Type: application/json" \
-d '{"query": {"term": {"categoria": "Electrónica"}}}'
```

**Agrupar por categoría:**
```
GET productos/_search
{
  "size": 0,
  "aggs": {
    "por_categoria": {
      "terms": {"field": "categoria"}
    }
  }
}
```

**Precio promedio por categoría:**
```
GET productos/_search
{
  "size": 0,
  "aggs": {
    "por_categoria": {
      "terms": {"field": "categoria"},
      "aggs": {
        "precio_promedio": {"avg": {"field": "precio"}}
      }
    }
  }
}
```

**Filtrar por rango de precio:**
```
GET productos/_search
{
  "query": {
    "range": {
      "precio": {"gte": 100, "lte": 500}
    }
  }
}
```

### Método de carga

**1. Crear índice:**
```bash
curl -u admin:OpenSearch@2026 -k -X PUT "https://localhost:9200/productos" \
-H "Content-Type: application/json" \
-d '{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  },
  "mappings": {
    "properties": {
      "nombre": {"type": "text"},
      "precio": {"type": "float"},
      "categoria": {"type": "keyword"},
      "stock": {"type": "integer"},
      "fecha_creacion": {"type": "date"}
    }
  }
}'
```

**2. Cargar documentos:**
```bash
curl -u admin:OpenSearch@2026 -k -X POST "https://localhost:9200/productos/_doc" \
-H "Content-Type: application/json" \
-d '{
  "nombre": "Laptop Dell XPS 13",
  "precio": 1200.50,
  "categoria": "Electrónica",
  "stock": 15,
  "fecha_creacion": "2026-01-15"
}'
```

---

