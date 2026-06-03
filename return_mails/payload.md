# RETURN
```json
{
    "event_code": "vx_return_created",
    "customer": {
      "id": 123,
      "name": "Nombre Cliente",
      "email": "cliente@example.com"
    },
    "order": {
      "entity_id": 456,
      "increment_id": "000000456"
    },
    "rma": {
      "entity_id": 789,
      "increment_id": "000000789",
      "status_id": 1,
      "status_name": "Pending Approval",
      "status_frontend_label": "Pendiente de aprobación",
      "message": "Texto del mensaje u observaciones del return",
      "created_at": "2026-06-03 10:00:00",
      "updated_at": "2026-06-03 10:00:00"
    },
    "store": {
      "store_id": 1
    },
    "transition": {
      "previous_status_id": 1,
      "previous_status_label": "Pendiente de aprobación",
      "current_status_id": 2,
      "current_status_label": "Aprobado"
    },
    "items": [
        {
            "order_item_id": 111,
            "product_id": 222,
            "parent_product_id": 333,
            "sku": "SKU-001",
            "name": "Producto",
            "url": "https://sitio.com/producto.html",
            "image_url": "https://sitio.com/media/catalog/product/x/y/imagen.jpg",
            "qty": 1,
            "price": 10.5,
            "reason_id": 1,
            "reason_label": "Razón de devolución",
            "item_condition_id": 1,
            "item_condition_label": "Condición",
            "resolution_id": 1,
            "resolution_label": "Resolución"
        }
    ],
    "shipping_label_url": ""
  },
  "contact_properties": {
    "FIRSTNAME": "Nombre",
    "LASTNAME": "Cliente"
  },
  "object": {
    "type": "rma",
    "identifiers": {
      "ext_id": "789"
    }
  }
```