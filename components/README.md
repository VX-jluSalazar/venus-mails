# Componentes reutilizables Venus

Estos snippets extraen los patrones comunes encontrados en los correos HTML actuales. Estan pensados para copiarse dentro del `<table class="email-container">` o dentro del cuerpo del email, segun corresponda.

## Estructura base

- `base-styles.html`: resets y media queries comunes para el `<head>`.
- `preheader.html`: preheader oculto.
- `email-shell-open.html` / `email-shell-close.html`: wrapper exterior y contenedor de 600px.
- `header.html`: cabecera con logo Venus.
- `footer.html`: pie completo con redes, enlaces legales y unsubscribe.

## Bloques principales

- `hero.html`: hero generico de estado/campana.
- `products.html`: listado de productos para pedido.
- `products-cart.html`: listado de productos para carrito abandonado.
- `order-summary.html`: resumen del pedido.
- `cart-summary.html`: resumen de carrito.
- `shipping-address.html`: direccion de envio/entrega.
- `benefits.html`: beneficios de envio, cambio y soporte.
- `cta-button.html`: CTA centrado.
- `support-box.html`: bloque de soporte.
- `alert-box.html`: alerta informativa reutilizable.
- `payment-channels.html`: canales de envio de comprobante.
- `payment-steps.html`: pasos para enviar comprobante.
- `tracking-card.html`: numero de seguimiento.
- `review-products.html`: productos con CTA de resena.
- `countdown.html`: contador visual para urgencia.
- `spacer.html`: espaciador vertical.

## Timelines

Los archivos `status-timeline-1.html` a `status-timeline-5.html` representan el avance de 5 pasos:

1. Pedido recibido
2. Pago confirmado
3. En preparacion
4. En camino
5. Entregado

El numero del archivo indica cuantos pasos estan completados o activos en verde.

## Notas

Los snippets conservan variables Liquid/Brevo como `{{ params.order.entity_id }}`, `{{ params.items }}` y `{{ unsubscribe }}`. Los textos marcados como `<!-- Personalizar -->` se pueden ajustar por email.
