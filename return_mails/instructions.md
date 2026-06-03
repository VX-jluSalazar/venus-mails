# Instrucciones para crear emails Farmagro en Brevo

## Objetivo

Tomar las capturas de referencia ubicadas en `/mail_mock/` y transformarlas en emails HTML editables y compatibles con Brevo, usando las variables recibidas en el payload.

El resultado debe respetar el diseno visual de cada captura, pero construido como HTML de email: tablas, estilos inline y estructura compatible con clientes como Gmail, Outlook y moviles.

## Fuentes de trabajo

- Mockups visuales: `/mail_mock/`
- Payloads de referencia: `payload.md`
- Banco de imagenes reutilizables: `email_images.md`
- Imagenes locales exportadas: `/images/`

## Reglas generales de HTML para email

- Usar estructura basada en tablas.
- Aplicar estilos inline en cada elemento.
- Mantener un ancho maximo recomendado de `600px`.
- Usar `role="presentation"` en tablas de layout.
- Evitar CSS moderno que tenga soporte limitado en emails: flexbox, grid, position fixed, scripts, videos, formularios y estilos externos.
- Las imagenes deben usar URLs absolutas.
- Agregar siempre `alt`, `width`, `height` cuando aplique y `style="display:block;"` en imagenes principales.
- Siempre usar `background-size: cover` en imagenes.
- Los botones deben ser enlaces `<a>` estilizados, no elementos `<button>`.
- Todo texto dinamico debe venir desde el payload cuando exista.

## Importante sobre bordes redondeados

Las tablas no deben tener bordes redondeados directamente. Para aplicar `border-radius`, fondo, padding, borde o sombra, usar una capa interna.

Estructura correcta:

```html
<table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0">
  <tr>
    <td>
      <div style="border-radius:16px; background:#ffffff; padding:24px;">
        Contenido
      </div>
    </td>
  </tr>
</table>
```

No aplicar `border-radius` directamente en `<table>`, `<tr>` o `<td>` si se busca compatibilidad consistente.

## Variables del payload de Brevo

Usar los atributos enviados en `payload.md`. El objeto base esperado es `params`. En Brevo normalmente se accede a los parametros transaccionales desde `params`, por ejemplo:

```html
{{ params.customer.firstname }}
{{ params.order.reference }}
{{ params.total_paid|floatformat:2 }}
```

Si el template de Brevo recibe los datos con otro namespace, ajustar solo el prefijo y conservar los nombres de atributos del payload.

## Payload: carrito abandonado

Campos disponibles principales:

| Campo | Uso sugerido |
| --- | --- |
| `params.customer.email` | Email del cliente |
| `params.customer.firstname` | Nombre del cliente |
| `params.customer.lastname` | Apellido del cliente |
| `params.customer.id` | ID del cliente |
| `params.customer.is_customer` | Indica si el contacto es cliente registrado |
| `params.currency` | Moneda del carrito |
| `params.cart.id` | ID del carrito |
| `params.cart.abandoned_minutes` | Minutos desde abandono |
| `params.cart.updated_at` | Fecha ISO de ultima actualizacion |
| `params.cart.url` | URL del carrito |
| `params.cart.totals.products` | Total de productos |
| `params.cart.totals.products_tax_amount` | Impuesto de productos |
| `params.cart.totals.products_tax_excl` | Total productos sin impuesto |
| `params.cart.totals.products_tax_incl` | Total productos con impuesto |
| `params.cart.totals.shipping` | Total de envio |
| `params.cart.totals.shipping_tax_amount` | Impuesto de envio |
| `params.cart.totals.shipping_tax_excl` | Envio sin impuesto |
| `params.cart.totals.shipping_tax_incl` | Envio con impuesto |
| `params.cart.totals.discounts` | Total de descuentos |
| `params.cart.totals.discounts_tax_amount` | Impuesto de descuentos |
| `params.cart.totals.discounts_tax_excl` | Descuentos sin impuesto |
| `params.cart.totals.discounts_tax_incl` | Descuentos con impuesto |
| `params.cart.totals.total` | Total del carrito |
| `params.cart.totals.total_tax_amount` | Impuesto total |
| `params.cart.totals.total_tax_excl` | Total sin impuesto |
| `params.cart.totals.total_tax_incl` | Total con impuesto |
| `params.misc.cart_url` | CTA principal para recuperar carrito |
| `params.misc.contact_url` | Link de contacto por WhatsApp |
| `params.misc.shop_url` | Link a la tienda |
| `params.shop_url` | Link a la tienda, tambien disponible en raiz |

### Productos del carrito:

Array: `params.cart.items`

| Campo por producto | Uso sugerido |
| --- | --- |
| `item.id_product` | ID del producto |
| `item.id_product_attribute` | ID de combinacion o variante |
| `item.name` | Nombre del producto |
| `item.reference` | Referencia del producto |
| `item.quantity` | Cantidad |
| `item.image_url` | Imagen del producto |
| `item.product_url` | Link al producto |
| `item.category_id` | ID de categoria |
| `item.category_name` | Nombre de categoria |
| `item.tax_name` | Nombre de impuesto |
| `item.tax_rate` | Porcentaje de impuesto |
| `item.unit_price` | Precio unitario |
| `item.unit_price_tax_amount` | Impuesto unitario |
| `item.unit_price_tax_excl` | Precio unitario sin impuesto |
| `item.unit_price_tax_incl` | Precio unitario con impuesto |
| `item.total_price` | Total de la linea |
| `item.total_price_tax_amount` | Impuesto total de la linea |
| `item.total_price_tax_excl` | Total de linea sin impuesto |
| `item.total_price_tax_incl` | Total de linea con impuesto |

Usar `params.misc.cart_url` para el CTA principal de recuperar carrito y `params.misc.contact_url` para contacto por WhatsApp.

## Payload: orden / post-compra

Campos disponibles principales:

| Campo | Uso sugerido |
| --- | --- |
| `params.customer.email` | Email del cliente |
| `params.customer.firstname` | Nombre del cliente |
| `params.customer.lastname` | Apellido del cliente |
| `params.customer.id` | ID del cliente |
| `params.customer.is_customer` | Indica si el contacto es cliente registrado |
| `params.order.id` | ID de orden |
| `params.order.reference` | Referencia de orden |
| `params.order.status` | Estado de orden |
| `params.order.status_id` | ID del estado de orden |
| `params.order.date` | Fecha ISO de creacion |
| `params.order.date_formatted` | Fecha lista para mostrar |
| `params.order.currency` | Moneda de la orden |
| `params.order.totals.products` | Total de productos desde el objeto orden |
| `params.order.totals.shipping` | Total de envio desde el objeto orden |
| `params.order.totals.discounts` | Total de descuentos desde el objeto orden |
| `params.order.totals.paid` | Total pagado desde el objeto orden |
| `params.order.totals.tax_amount` | Impuesto total desde el objeto orden |
| `params.total_products` | Total productos, disponible en raiz |
| `params.total_shipping` | Total envio, disponible en raiz |
| `params.total_discounts` | Total descuentos, disponible en raiz |
| `params.total_paid` | Total pagado, disponible en raiz |
| `params.total_tax_amount` | Impuesto total, disponible en raiz |
| `params.payment.method` | Metodo de pago |
| `params.payment.method_id` | ID del metodo de pago |
| `params.payment.module` | Modulo de pago |
| `params.shipping.carrier.name` | Transportista o metodo de entrega |
| `params.shipping.tracking_code` | Codigo de tracking |
| `params.shipping.tracking_url` | URL de tracking |
| `params.misc.reorder_url` | CTA de recompra |
| `params.misc.shop_review_url` | Link para dejar resena |
| `params.misc.contact_url` | Link de soporte por WhatsApp |
| `params.misc.shop_url` | Link a la tienda |
| `params.shop_url` | Link a la tienda, tambien disponible en raiz |

### Productos de la orden:

Array: `params.order.items`

| Campo por producto | Uso sugerido |
| --- | --- |
| `item.id_product` | ID del producto |
| `item.id_product_attribute` | ID de combinacion o variante |
| `item.is_variant` | Indica si es variante |
| `item.name` | Nombre del producto |
| `item.reference` | Referencia del producto |
| `item.quantity` | Cantidad |
| `item.image_url` | Imagen del producto |
| `item.product_url` | Link al producto |
| `item.category_id` | ID de categoria |
| `item.category_name` | Nombre de categoria |
| `item.tax_name` | Nombre de impuesto |
| `item.tax_rate` | Porcentaje de impuesto |
| `item.unit_price` | Precio unitario |
| `item.unit_price_tax_amount` | Impuesto unitario |
| `item.unit_price_tax_excl` | Precio unitario sin impuesto |
| `item.unit_price_tax_incl` | Precio unitario con impuesto |
| `item.total_price` | Total de la linea |
| `item.total_price_tax_amount` | Impuesto total de la linea |
| `item.total_price_tax_excl` | Total de linea sin impuesto |
| `item.total_price_tax_incl` | Total de linea con impuesto |

Direccion de envio:

| Campo | Uso sugerido |
| --- | --- |
| `params.shipping.address.firstname` | Nombre del destinatario |
| `params.shipping.address.lastname` | Apellido del destinatario |
| `params.shipping.address.dni` | Identificacion |
| `params.shipping.address.address1` | Direccion principal |
| `params.shipping.address.address2` | Direccion secundaria |
| `params.shipping.address.city` | Ciudad |
| `params.shipping.address.state` | Provincia o estado |
| `params.shipping.address.country` | Pais |
| `params.shipping.address.phone` | Telefono |

Usar `params.misc.reorder_url` para recompra, `params.misc.shop_review_url` para resenas y `params.misc.contact_url` para soporte.

## Payload: suscripcion

Campos disponibles principales:

| Campo | Uso sugerido |
| --- | --- |
| `params.customer.email` | Email del cliente |
| `params.customer.firstname` | Nombre del cliente |
| `params.customer.lastname` | Apellido del cliente |
| `params.customer.id` | ID del cliente |
| `params.customer.is_customer` | Indica si el contacto es cliente registrado |
| `params.misc.contact_url` | Link de contacto por WhatsApp |
| `params.misc.shop_url` | CTA principal para visitar la tienda |
| `params.misc.faq_yrl` | URL de FAQ enviada por el payload |
| `params.shop_url` | Link a la tienda, tambien disponible en raiz |

Categorias principales:

Array: `params.main_categories`

| Campo por categoria | Uso sugerido |
| --- | --- |
| `category.id_category` | ID de categoria |
| `category.name` | Nombre de categoria |
| `category.description` | Descripcion |
| `category.image_url` | Imagen de categoria, si viene disponible |
| `category.category_url` | Link a categoria |

Usar `params.misc.shop_url` para el CTA principal de visitar la tienda y `params.misc.contact_url` para WhatsApp.

## Listas dinamicas

Cuando un mockup incluya productos, categorias, productos relacionados o resenas, renderizar esos bloques desde los arrays del payload:

| Lista | Array |
| --- | --- |
| Productos del carrito abandonado | `params.cart.items` |
| Productos de orden | `params.order.items` |
| Productos relacionados | `params.related_products` |
| Categorias principales de suscripcion | `params.main_categories` |
| Resenas de tienda | `params.shop_reviews` |

### Productos relacionados

Array: `params.related_products`

| Campo por producto | Uso sugerido |
| --- | --- |
| `product.id_product` | ID del producto |
| `product.name` | Nombre del producto |
| `product.price` | Precio |
| `product.currency` | Moneda |
| `product.image_url` | Imagen del producto |
| `product.product_url` | Link al producto |
| `product.category_id` | ID de categoria |
| `product.category_name` | Nombre de categoria |

### Resenas de tienda

Array: `params.shop_reviews`

| Campo por resena | Uso sugerido |
| --- | --- |
| `review.id_review` | ID de resena |
| `review.title` | Titulo |
| `review.summary` | Texto de la resena |
| `review.rating` | Calificacion |
| `review.customer_name` | Nombre completo del cliente |
| `review.customer_firstname` | Nombre del cliente |
| `review.customer_lastname` | Apellido del cliente |
| `review.customer_initials` | Iniciales para avatar textual |
| `review.created_at` | Fecha de creacion |
| `review.updated_at` | Fecha de actualizacion |

Ejemplo orientativo para productos:

```html
{% for product in params.related_products %}
  <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0">
    <tr>
      <td>
        <div style="background:#ffffff; border-radius:12px; padding:16px;">
          <a href="{{ product.product_url }}">
            <img src="{{ product.image_url }}" alt="{{ product.name }}" width="120" style="display:block; width:120px; height:auto;">
          </a>
          <p style="margin:12px 0 4px; font-family:Arial, sans-serif;">{{ product.name }}</p>
          <p style="margin:0; font-family:Arial, sans-serif;">{{ product.currency }} {{ product.price|floatformat:2 }}</p>
        </div>
      </td>
    </tr>
  </table>
{% endfor %}
```

Ejemplo orientativo para productos del carrito:

```html
{% for item in params.cart.items %}
  <p style="margin:0; font-family:Arial, sans-serif;">
    {{ item.quantity }} x {{ item.name }} - {{ params.currency }} {{ item.total_price|floatformat:2 }}
  </p>
{% endfor %}
```

Ejemplo orientativo para categorias:

```html
{% for category in params.main_categories %}
  <a href="{{ category.category_url }}" style="font-family:Arial, sans-serif; color:#1f3d2b; text-decoration:none;">
    {{ category.name }}
  </a>
{% endfor %}
```

Verificar la sintaxis final del loop dentro del editor de Brevo antes de enviar.

## Formato de precios

- Mostrar siempre la moneda desde el payload: `params.currency`, `params.order.currency` o `product.currency`, segun el tipo de email.
- Usar los totales del payload, no calcularlos manualmente si ya existen.
- Para carrito abandonado usar `params.cart.totals.products`, `params.cart.totals.shipping`, `params.cart.totals.discounts` y `params.cart.totals.total`.
- Para orden usar `params.total_products`, `params.total_shipping`, `params.total_discounts` y `params.total_paid`. Si se prefiere mantener todo dentro de `order`, usar `params.order.totals.products`, `params.order.totals.shipping`, `params.order.totals.discounts` y `params.order.totals.paid`.
- En lineas de producto usar `item.total_price` o `item.total_price_tax_incl` cuando el precio mostrado deba incluir impuestos.

## Imagenes reutilizables

Estas URLs fueron extraidas de `email_images.md` y se pueden usar en cualquier email.

| Nombre | URL |
| --- | --- |
| Header y footer logo | `https://img.mailinblue.com/11204223/images/content_library/original/6a10d121a2ad2362aa5e5bb3.png` |

Usar siempre el logo de header y footer como imagen, no como texto HTML. Ejemplo:

```html
<img src="https://img.mailinblue.com/11204223/images/content_library/original/6a10d121a2ad2362aa5e5bb3.png" alt="Hemp Ecuador Labs" width="112" height="49" style="display:block; width:112px; height:auto; border:0;">
```

Tambien se pueden usar las imagenes absolutas incluidas en el payload para productos:

| Uso | Campo |
| --- | --- |
| Producto de carrito | `item.image_url` dentro de `params.cart.items` |
| Producto de orden | `item.image_url` dentro de `params.order.items` |
| Producto relacionado | `product.image_url` dentro de `params.related_products` |
| Categoria | `category.image_url` dentro de `params.main_categories`, si no viene vacio |

## Checklist antes de entregar un email

- El email replica el mockup correspondiente de `/mail_mock/`.
- Todos los textos variables usan atributos del payload.
- Todos los links importantes vienen del payload: tienda, carrito, producto, recompra, resena o WhatsApp.
- Las imagenes usan URLs absolutas.
- Las tablas no tienen estilos de borde redondeado aplicados directamente.
- Los estilos visuales con `border-radius`, fondo, borde o padding estan en un `<div>` interno.
- El HTML no depende de archivos CSS, scripts ni assets locales.
- El email fue revisado en desktop y mobile dentro de Brevo o un preview compatible.
