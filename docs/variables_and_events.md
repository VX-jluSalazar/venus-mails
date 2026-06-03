# BrevoCustom Events Documentation

Este documento resume los estados/eventos que el módulo `Velox_BrevoCustom` envía a Brevo y la estructura de `event_properties` para cada uno.

## Eventos enviados

1. `vx_order_created`
2. `vx_pending_order_payment`
3. `vx_order_processing`
4. `vx_order_sent`
5. `vx_order_delivered`
6. `vx_abandoned_cart`
7. `vx_suscriber`
8. `vx_customer_account_verification_requested`
9. `vx_customer_account_confirmed`

## 1) `vx_order_created`

Trigger Magento: `checkout_submit_all_after`  
Entidad: `order`  
Observer: `Observer/OrderPlaceAfter.php`

### event_properties

```json
{
  "source": "https://store.example.com/",
  "customer": {
    "email": "customer@example.com",
    "first_name": "Jane",
    "last_name": "Doe"
  },
  "order": {
    "created_at": "2026-05-05 15:02:00",
    "currency": "USD",
    "discount_amount": -5.0,
    "entity_id": 123,
    "grand_total": 99.9,
    "increment_id": "000000123",
    "order_url": "https://store.example.com/sales/order/view/order_id/123/",
    "payment_method": "Check / Money order",
    "payment_method_code": "checkmo",
    "survey_url": "https://store.example.com/encuestasatisfaccion?id=000000123&ver=2",
    "shipping_amount": 10.0,
    "shipping_method": "Flat Rate - Fixed",
    "shipping_method_code": "flatrate_flatrate",
    "state": "new",
    "status": "pending",
    "subtotal": 89.9,
    "tax_amount": 0.0,
    "updated_at": "2026-05-05 15:02:05"
  },
  "items": [
    {
      "url": "https://store.example.com/product-a",
      "image_url": "https://store.example.com/media/catalog/product/a.jpg",
      "discount_amount": 0.0,
      "final_price": 29.9,
      "is_variation": false,
      "variations": [],
      "name": "Product A",
      "parent_name": null,
      "parent_sku": null,
      "price": 29.9,
      "qty": 1.0,
      "row_total": 29.9,
      "sku": "SKU-A",
      "tax_amount": 0.0
    }
  ],
  "related_products": [],
  "reviews": [],
  "billing_address": {
    "first_name": "Jane",
    "last_name": "Doe",
    "company": null,
    "street": ["Street 1"],
    "city": "Quito",
    "region": "Pichincha",
    "region_code": "EC-P",
    "country_id": "EC",
    "telephone": "0999999999"
  },
  "shipping_address": {
    "first_name": "Jane",
    "last_name": "Doe",
    "company": null,
    "street": ["Street 1"],
    "city": "Quito",
    "region": "Pichincha",
    "region_code": "EC-P",
    "country_id": "EC",
    "telephone": "0999999999"
  },
  "shipment": {
    "entity_id": null,
    "increment_id": null,
    "created_at": null,
    "updated_at": null,
    "tracks": []
  }
}
```

Notas:
- `related_products` se incluye cuando hay candidatos.
- `reviews` se incluye cuando hay reviews elegibles de productos del pedido.

## 2) `vx_pending_order_payment`

Trigger Magento: `sales_order_save_after` (transición de estado/status)  
Entidad: `order`  
Resolver: `Model/Order/OrderStatusEventResolver.php` (status/state `pending`)

### event_properties

Misma estructura de `vx_order_created`, con campos de transición dentro de `order`:

```json
{
  "order": {
    "previous_state": "new",
    "previous_status": "pending_payment",
    "state": "new",
    "status": "pending"
  }
}
```

## 3) `vx_order_processing`

Trigger Magento: `sales_order_save_after`  
Resolver: status/state `processing`

### event_properties

Misma estructura de eventos de orden + transición en `order.previous_state` y `order.previous_status`.

## 4) `vx_order_sent`

Trigger Magento: `sales_order_save_after`  
Resolver: status/state `complete`

### event_properties

Misma estructura de eventos de orden + transición en `order.previous_state` y `order.previous_status`.

## 5) `vx_order_delivered`

Trigger Magento: `sales_order_save_after`  
Resolver: status/state `completado`

### event_properties

Misma estructura de eventos de orden + transición en `order.previous_state` y `order.previous_status`.

## 6) `vx_abandoned_cart`

Trigger Magento: cron `0 * * * *` y comando manual  
Entidad: `quote`  
Cron class: `Cron/SendAbandonedCartEvents.php`

### event_properties

```json
{
  "store": {
    "id": 1,
    "code": "default",
    "name": "Default Store View",
    "base_url": "https://store.example.com/",
    "currency": "USD"
  },
  "cart": {
    "quote_id": 456,
    "is_active": true,
    "items_count": 2,
    "grand_total": 65.5,
    "discounted_grand_total": 58.95,
    "subtotal": 60.0,
    "discounted_subtotal": 54.0,
    "tax_amount": 0.0,
    "shipping_amount": 5.5,
    "discount_amount": -3.0,
    "coupon_discount_amount": -6.55,
    "currency": "USD",
    "updated_at": "2026-05-05 14:25:00"
  },
  "coupon": {
    "code": "VXAB-456-8F3K2P",
    "discount_label": "10% OFF",
    "discount_type": "percent",
    "discount_amount": 10.0,
    "expires_at": "2026-05-25 23:59:59"
  },
  "customer": {
    "id": 77,
    "email": "customer@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "telephone": "0999999999",
    "url": null
  },
  "items": [
    {
      "product_id": 1001,
      "sku": "SKU-A",
      "name": "Product A",
      "parent_name": "Product Parent",
      "parent_sku": "PARENT-SKU",
      "qty": 1.0,
      "price": 29.9,
      "row_total": 29.9,
      "tax_amount": 0.0,
      "discount_amount": 0.0,
      "final_price": 29.9,
      "discounted_price": 26.91,
      "discounted_row_total": 26.91,
      "coupon_discount_amount": -2.99,
      "is_variation": true,
      "variations": [],
      "url": "https://store.example.com/product-a",
      "image_url": "https://store.example.com/media/catalog/product/a.jpg",
      "review_flags": {
        "has_reviews": true,
        "media_rating": 5
      }
    }
  ],
  "related_products": [],
  "reviews": []
}
```

Notas:
- `items[].review_flags` indica si existen reviews elegibles para el item.
- `reviews` se agrega a nivel superior cuando hay reviews elegibles del producto hijo o del producto padre.
- `coupon`, `cart.discounted_*` e `items[].discounted_*` se agregan cuando la generacion de cupones de carrito abandonado esta habilitada.
- Solo se envía cuando el estado interno resuelto es `abandoned`.

## 7) `vx_suscriber`

Trigger Magento: `newsletter_subscriber_save_after`  
Entidad: `newsletter`  
Observer: `Observer/NewsletterSubscriberSaveAfter.php`

### event_properties

```json
{
  "store": {
    "id": 1,
    "code": "default",
    "name": "Default Store View",
    "base_url": "https://store.example.com/",
    "currency": "USD"
  },
  "newsletter": {
    "subscriber_id": 55,
    "email": "customer@example.com",
    "status": "subscribed",
    "store_id": 1,
    "customer_id": 77,
    "created_at": "2026-05-05 15:02:00",
    "updated_at": "2026-05-05 15:02:05",
    "source_metadata": {
      "source": "footer_form",
      "source_url": "https://store.example.com/",
      "source_page": "home",
      "signup_source": "web",
      "ip": "127.0.0.1",
      "user_agent": "Mozilla/5.0"
    }
  },
  "customer": {
    "id": 77,
    "email": "customer@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "telephone": "0999999999",
    "url": null
  },
  "product_reviews": [
    {
      "review_id": 11,
      "product_id": 1001,
      "sku": null,
      "product_name": null,
      "rating": 5,
      "title": "Excelente",
      "summary": "Muy buen producto",
      "customer_name": "Ana",
      "created_at": "2026-04-20 10:00:00",
      "source_context": "order_item"
    }
  ]
}
```

Notas:
- `product_reviews` se agrega cuando hay candidatas disponibles.
- Solo se envía para transición a estado `subscribed`.

## 8) `vx_customer_account_verification_requested`

Trigger Magento: `customer_register_success`  
Entidad: `customer`  
Observer: `Observer/CustomerRegisterSuccess.php`

Se envía solo cuando el cliente recién creado tiene `confirmation` no vacío, es decir, cuando Magento requiere confirmar la cuenta por correo.

### event_properties

```json
{
  "store": {
    "id": 1,
    "code": "default",
    "name": "Default Store View",
    "base_url": "https://store.example.com/",
    "currency": "USD"
  },
  "source": "https://store.example.com/",
  "customer": {
    "id": 123,
    "email": "customer@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "full_name": "Jane Doe",
    "group_id": 1,
    "website_id": 1,
    "store_id": 1,
    "created_at": "2026-05-08 10:00:00",
    "account_url": "https://store.example.com/customer/account/"
  },
  "account": {
    "requires_confirmation": true,
    "is_confirmed": false,
    "created_at": "2026-05-08 10:00:00"
  },
  "verification": {
    "confirmation_url": "https://store.example.com/customer/account/confirm/?id=123&key=..."
  }
}
```

Notas:
- `confirmation_url` permite replicar en Brevo el CTA de `Plasticaucho_EmailsModule::account_new_confirmation.html`.
- El hash de confirmación se usa para dedupe, pero la key cruda no se registra en logs.
- En logs normales `verification.confirmation_url` se enmascara.

## 9) `vx_customer_account_confirmed`

Trigger Magento: `customer_save_after_data_object` con fallback `customer_save_after`  
Entidad: `customer`  
Observer: `Observer/CustomerSaveAfterDataObject.php`

Se envía solo cuando `confirmation` cambia de valor no vacío a vacío/null.

### event_properties

```json
{
  "store": {
    "id": 1,
    "code": "default",
    "name": "Default Store View",
    "base_url": "https://store.example.com/",
    "currency": "USD"
  },
  "source": "https://store.example.com/",
  "customer": {
    "id": 123,
    "email": "customer@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "full_name": "Jane Doe",
    "group_id": 1,
    "website_id": 1,
    "store_id": 1,
    "created_at": "2026-05-08 10:00:00",
    "account_url": "https://store.example.com/customer/account/"
  },
  "account": {
    "requires_confirmation": false,
    "is_confirmed": true,
    "created_at": "2026-05-08 10:00:00"
  },
  "transition": {
    "previous_confirmation_present": true,
    "current_confirmation_present": false
  }
}
```

Notas:
- Replica en Brevo el flujo funcional de `Plasticaucho_EmailsModule::account_new_confirmed.html`.
- Re-guardar el cliente confirmado no debe reenviar el evento porque no existe transición de `confirmation`.

## Campos comunes importantes

1. `event_name` (fuera de `event_properties`) es el código del evento Brevo.
2. `identifiers.email_id` (fuera de `event_properties`) es obligatorio para envío exitoso.
3. `contact_properties` normalmente incluye:
   - `FIRSTNAME`
   - `LASTNAME`
