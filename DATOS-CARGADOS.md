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

# Dataset Personalizado: E-commerce Orders

## Descripción
Dataset de órdenes de compra en línea con información de clientes, productos, montos y estados.

## Estructura

### Índice: `ecommerce_orders`

| Campo | Tipo | Descripción |
|-------|------|-------------|
| order_id | text | ID de la orden (ORD-001, ORD-002, etc) |
| customer | text | Nombre del cliente |
| email | text | Email del cliente |
| product | text | Nombre del producto |
| category | keyword | Categoría (Electronics, Fashion) |
| amount | long | Monto en euros |
| quantity | long | Cantidad de unidades |
| status | keyword | Estado (completed, pending, failed, cancelled) |
| timestamp | date | Fecha y hora de la orden |
| location | keyword | Ubicación del cliente |

## Datos Cargados

**Total: 15 documentos**

### Por Categoría:
- **Electronics:** 8 órdenes (Laptops, iPhones, TVs, etc)
- **Fashion:** 7 órdenes (Shoes, T-shirts, Dresses, Bags, etc)

### Por Estado:
- **Completed:** 10 órdenes
- **Pending:** 3 órdenes
- **Failed:** 1 orden
- **Cancelled:** 1 orden

### Por Ubicación:
- Madrid: 4 órdenes
- Barcelona: 3 órdenes
- Valencia: 2 órdenes
- Sevilla: 2 órdenes
- Bilbao: 2 órdenes

## Monto Total Generado
**€12,289.90**

### Desglose:
- Electronics: $7,589.93
- Fashion: $4,699.97

---

