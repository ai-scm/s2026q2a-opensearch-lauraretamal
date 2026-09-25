# Consultas API REST de OpenSearch

## Acceso a API REST

Las consultas pueden ejecutarse desde:
1. **Dev Tools** en OpenSearch Dashboards
2. **curl** desde CMD
3. Cualquier herramienta HTTP (Postman, etc)

---

## Consulta 1: Obtener 1 Documento
```json
GET opensearch_dashboards_sample_data_logs/_search
{
"size": 1
}
```


**Resultado:** Retorna 1 documento con toda su estructura.

**Ejemplo de respuesta:**
```json
{
  "took": 9,
  "hits": {
    "total": {"value": 10000, "relation": "gte"},
    "hits": [
      {
        "_index": "opensearch_dashboards_sample_data_logs",
        "_id": "Qcda1qABPOGvEEdrE-Hq",
        "_source": {
          "host": "artifacts.opensearch.org",
          "response": 200,
          "bytes": 6219,
          "timestamp": "2026-09-13T00:39:02.912Z"
        }
      }
    ]
  }
}
```

---

## Consulta 2: Contar Documentos

```json
GET opensearch_dashboards_sample_data_logs/_count
```

**Resultado:** 
```json
{
  "count": 14074,
  "_shards": {...}
}
```

Total de documentos: **14,074**

---

## Consulta 3: Ver Estructura del Índice (Mapping)
```json
GET opensearch_dashboards_sample_data_logs/_mapping
```

**Campos principales:**
- `agent` (text)
- `bytes` (long)
- `clientip` (ip)
- `geo` (geo_point)
- `host` (text/keyword)
- `response` (text/keyword)
- `timestamp` (date)
- `url` (text/keyword)

---

## Consulta 4: Filtrar por Host Específico
```json
GET opensearch_dashboards_sample_data_logs/_search
{
"query": {
"term": {
"host.keyword": "artifacts.opensearch.org"
}
}
}
```


**Resultado:**
```json
{
  "hits": {
    "total": {"value": 6488, "relation": "eq"},
    "hits": [...]
  }
}
```

Encontrados: **6,488 documentos** para ese host.

---

## Consulta 5: Faceted Search - Agregaciones por Host y Response
```json
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


**Resultado - Hosts:**
```json
"por_host": {
  "buckets": [
    {"key": "artifacts.opensearch.org", "doc_count": 6488},
    {"key": "www.opensearch.org", "doc_count": 4779},
    {"key": "cdn.opensearch-opensearch-opensearch.org", "doc_count": 2255},
    {"key": "opensearch-opensearch-opensearch.org", "doc_count": 552}
  ]
}
```

**Resultado - Response Codes:**
```json
"por_response": {
  "buckets": [
    {"key": "200", "doc_count": 12832},
    {"key": "404", "doc_count": 801},
    {"key": "503", "doc_count": 441}
  ]
}
```

---

## Ejecutar desde CMD con curl

```bash
curl -u admin:OpenSearch@2024Secure -X GET "https://localhost:9200/opensearch_dashboards_sample_data_logs/_count" -k
```

**Nota:** `-k` ignora certificados SSL auto-firmados.

---


