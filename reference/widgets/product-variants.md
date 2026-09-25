# Product Variants Widget

**Widget type:** `product-variants`

Renderuje picker wariantów na karcie produktu. Klik w wariant nawiguje do karty produktu-wariantu (`/produkt/:slug`). Widget bierze dane z injected `lcms-product` — działa gdy bieżący produkt jest rodzicem-kontenerem (`has_children: true`). Gdy użytkownik jest już na karcie wariantu, widget (opcjonalnie) może się ukryć — w V1 nie pobiera rodzeństwa z innej karty.

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-variants"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data object |
| `widget.config` | object | Configuration element group |
| `widget.config.display_type` | string | Display mode: `"buttons"`, `"dropdown"`, or `"swatches"` |
| `widget.config.label_source` | string | Label source: `"binding_value"`, `"variant_name"`, or `"custom_attribute"` |
| `widget.config.label_attribute_code` | string | Attribute code when `label_source` is `"custom_attribute"` (e.g. `"material"`) |
| `widget.config.show_group_labels` | boolean | Whether to show the binding attribute name before options |
| `widget.config.hide_if_single` | boolean | Whether to hide the widget when product has only one variant |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text |
| `widget.group_label` | object | Group label styling |
| `widget.group_label.color` | string | Label text color (CSS variable or hex) |
| `widget.group_label.font_weight` | string | Label font weight (CSS value) |
| `widget.option` | object | Option button styling |
| `widget.option.background` | string | Option background color (normal state) |
| `widget.option.color` | string | Option text color (normal state) |
| `widget.option.background:hover` | string | Option background color (hover state) |
| `widget.option.color:hover` | string | Option text color (hover state) |
| `widget.option.border_color` | string | Option border color |
| `widget.selected` | object | Selected option styling |
| `widget.selected.background` | string | Selected option background color |
| `widget.selected.color` | string | Selected option text color |
| `widget.selected.border_color` | string | Selected option border color |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Label source semantics

Dla każdego wariantu `label_source` decyduje co renderować jako tekst/tooltip:

- **`binding_value`** — wartość atrybutu wiążącego z `variant_binding_values`, zresolvowana przez `template_attributes.options` (np. `{size: 'xl'}` → `"XL"`). Gdy wiele binding attrs, łączone przez ` / `.
- **`variant_name`** — `variant.name` wprost (np. `"Album Premium 15x15"`).
- **`custom_attribute`** — wartość z `variant.attributes[label_attribute_code]`. Dzięki `effective_attributes` backend scala atrybuty wariantu z rodzicem, więc działa nawet gdy wariant nie nadpisał atrybutu.

## Display types

- **buttons** — pill-shaped linki, aktywny podświetlony `selected.*` kolorami
- **dropdown** — natywny `<select>` z nawigacją `onchange`
- **swatches** — kółka 36×36, background z `option.color_hex` (binding option) lub miniatury `variant.image`; gdy brak obu, tekst etykiety

## Context requirements

Wymaga `provide('lcms-product', ref(product))` — zwykle zapewniane przez route karty produktu. Odczytuje:
- `product.children[]` — lista wariantów (simple product objects)
- `product.template_attributes[]` — opcje binding (`code` + `value`)
- `product.slug` — do oznaczenia aktywnej opcji

Opcjonalnie `provide('lesscms-project-config')` dla `commerce.routes.product` (wzorzec URL wariantu).

## Example Response

```json
{
  "widget_type": "product-variants",
  "uuid": "pv-101",
  "widget": {
    "config": {
      "display_type": "buttons",
      "label_source": "binding_value",
      "label_attribute_code": "",
      "show_group_labels": true,
      "hide_if_single": true
    },
    "heading": {
      "text": {
        "en": "Choose your size",
        "pl": "Wybierz rozmiar"
      }
    },
    "group_label": {
      "color": "var:dark",
      "font_weight": "600"
    },
    "option": {
      "background": null,
      "color": "var:dark",
      "background:hover": "var:background-alt",
      "color:hover": null,
      "border_color": "var:border"
    },
    "selected": {
      "background": "var:primary",
      "color": "var:white",
      "border_color": "var:primary"
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```
