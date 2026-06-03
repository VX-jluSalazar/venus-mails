# Lineamientos para Modificar Correos (Brevo)

## 1) Estructura y bordes redondeados
- No aplicar `border-radius` directamente en `table`, `tr` o `td` para bloques principales.
- Usar un `div` contenedor dentro del `td` y aplicar ahí:
  - `border`
  - `border-radius`
  - `overflow:hidden` (si el bloque requiere recorte)
- Patrón recomendado:

```html
<td>
  <div style="border:1px solid #e5e7eb;border-radius:10px;overflow:hidden;">
    <table width="100%" cellpadding="0" cellspacing="0" border="0">
      ...
    </table>
  </div>
</td>
```

## 2) URLs (regla funcional)
- En correos de pedido (`order-*`), toda URL relacionada al pedido debe usar:
  - `{{ params.order.order_url }}`
- En encuestas, usar:
  - `{{ params.order.survey_url }}`
- No hardcodear rutas de pedido ni concatenar `params.source` para tracking/detalle.

## 3) Variables del payload (fuente única)
- Usar únicamente variables de `params` enviadas por evento.
- Referencias comunes:
  - Pedido: `params.order.entity_id`, `params.order.payment_method`, `params.order.grand_total`
  - Totales: `subtotal`, `shipping_amount`, `discount_amount`, `tax_amount`
  - Cliente: `params.customer.first_name`
  - Dirección: `params.shipping_address.*`
- Fecha en resumen:
  - Preferido Brevo-safe: `{{ params.order.created_at }}`
  - Si el parser lo tolera, formato avanzado puede variar según motor.

## 4) Reglas Brevo-safe (guardar sin errores)
- Evitar bloques condicionales MSO/VML en la versión editable en Brevo:
  - `<!--[if mso]> ... <![endif]-->`
  - `<v:roundrect ...>`
- Evitar atributos inválidos HTML como `height="auto"` en `img`.
- Evitar JS inline en atributos (`onerror=...`) dentro del HTML de plantilla.
- Mantener HTML balanceado (sin cierres extra/faltantes de `div`, `td`, `tr`, `table`).
- Minimizar lógica compleja dentro de tablas cuando sea posible.

## 5) Checklist antes de pegar/guardar en Brevo
- `border-radius` aplicado en `div` contenedor, no en `table/tr/td` de bloque.
- URLs de pedido -> `order_url`.
- URL encuesta -> `survey_url`.
- Variables del payload correctas (`params.order.*`, `params.customer.*`, etc.).
- Sin MSO/VML, sin `height="auto"`, sin `onerror`.
- Revisión visual rápida de apertura/cierre de etiquetas.

## 6) Convención recomendada de archivos
- Mantener una versión base editable en repo.
- Si hace falta compatibilidad extra para Brevo, crear variante `*-brevo-safe.html`.
- Documentar diferencias entre base y safe en PR o nota de cambios.
