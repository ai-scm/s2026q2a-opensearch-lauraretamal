# Exploración del API REST de OpenSearch

## Búsquedas básicas

### Obtener 1 documento del índice de logs
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/opensearch_dashboards_sample_data_logs/_search?size=1"
```

**Respuesta (ejemplo):**
```json
{
  "took": 10,
  "hits": {
    "total": {"value": 2780, "relation": "eq"},
    "hits": [
      {
        "_index": "opensearch_dashboards_sample_data_logs",
        "_source": {
          "timestamp": "2026-09-24T10:00:00Z",
          "message": "GET /index.html 200",
          "host": "web-server-01",
          "response": 200,
          "bytes": 1234
        }
      }
    ]
  }
}
```

### Contar documentos
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/opensearch_dashboards_sample_data_logs/_count"
```

**Respuesta:**
```json
{
  "count": 2780,
  "_shards": {"total": 1, "successful": 1, "skipped": 0, "failed": 0}
}
```

### Ver estructura (mapping) del índice
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/opensearch_dashboards_sample_data_logs/_mapping"
```

### Ver configuración (settings)
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/opensearch_dashboards_sample_data_logs/_settings"
```

## Queries equivalentes en Dev Tools (Dashboards)

### Búsqueda simple
```
GET opensearch_dashboards_sample_data_logs/_search
{
  "size": 1
}
```

### Contar documentos
```
GET opensearch_dashboards_sample_data_logs/_count
```

### Ver mapping
```
GET opensearch_dashboards_sample_data_logs/_mapping
```

## Filtros específicos

### Filtrar por host específico
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/opensearch_dashboards_sample_data_logs/_search" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "term": {
      "host": "web-server-01"
    }
  }
}'
```

**Dev Tools equivalente:**
```
GET opensearch_dashboards_sample_data_logs/_search
{
  "query": {
    "term": {
      "host": "web-server-01"
    }
  }
}
```

### Filtrar índice de productos por categoría
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/productos/_search" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "term": {
      "categoria": "Electrónica"
    }
  }
}'
```

**Resultado: 2 documentos**
- Laptop Dell XPS 13
- Monitor Samsung 4K 27

### Filtrar por rango de precios
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/productos/_search" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "range": {
      "precio": {
        "gte": 100,
        "lte": 500
      }
    }
  }
}'
```

## Búsqueda facetada (Agregaciones)

### Agrupar por host y response
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/opensearch_dashboards_sample_data_logs/_search" \
-H "Content-Type: application/json" \
-d '{
  "size": 0,
  "aggs": {
    "por_host": {
      "terms": {
        "field": "host"
      }
    },
    "por_response": {
      "terms": {
        "field": "response"
      }
    }
  }
}'
```

**Dev Tools equivalente:**
```
GET opensearch_dashboards_sample_data_logs/_search
{
  "size": 0,
  "aggs": {
    "por_host": {
      "terms": {
        "field": "host.keyword"
      }
    },
    "por_response": {
      "terms": {
        "field": "response.keyword"
      }
    }
  }
}
```

### Agrupar productos por categoría
```bash
curl -u admin:OpenSearch@2026 -k -X GET "https://localhost:9200/productos/_search" \
-H "Content-Type: application/json" \
-d '{
  "size": 0,
  "aggs": {
    "por_categoria": {
      "terms": {
        "field": "categoria"
      }
    }
  }
}'
```

**Respuesta:**
```json
{
  "aggregations": {
    "por_categoria": {
      "buckets": [
        {
          "key": "Accesorios",
          "doc_count": 3
        },
        {
          "key": "Electrónica",
          "doc_count": 2
        }
      ]
    }
  }
}
```

### Precio promedio por categoría
```
GET productos/_search
{
  "size": 0,
  "aggs": {
    "por_categoria": {
      "terms": {
        "field": "categoria"
      },
      "aggs": {
        "precio_promedio": {
          "avg": {
            "field": "precio"
          }
        }
      }
    }
  }
}
```

**Resultado:**
- Accesorios: promedio $110.24 (3 productos)
- Electrónica: promedio $825.25 (2 productos)

### Filtro combinado: Accesorios con precio > $100
```
GET productos/_search
{
  "query": {
    "bool": {
      "must": [
        {"term": {"categoria": "Accesorios"}},
        {"range": {"precio": {"gte": 100}}}
      ]
    }
  }
}
```

**Resultado: 1 documento**
- Teclado Mecánico RGB ($150.75)

## Interpretación de respuestas JSON

### Estructura básica
```json
{
  "took": 3,                    // Tiempo en ms
  "timed_out": false,           // Si se agotó el timeout
  "_shards": {                  // Estado de shards
    "total": 1,
    "successful": 1,
    "failed": 0
  },
  "hits": {                     // Resultados
    "total": {"value": 5, "relation": "eq"},
    "hits": [                   // Array de documentos
      {
        "_index": "productos",
        "_id": "v_Cj1KAB0HBcR38HD5qN",
        "_score": 1,
        "_source": {            // Datos del documento
          "nombre": "Laptop Dell XPS 13",
          "precio": 1200.5
        }
      }
    ]
  },
  "aggregations": {}            // Resultados de agregaciones
}
```
