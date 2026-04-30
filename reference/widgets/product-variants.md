# Product Variants Widget

**Widget type:** `product-variants`

Renderuje picker wariantów na karcie produktu. Klik w wariant nawiguje do karty produktu-wariantu (`/produkt/:slug`). Widget bierze dane z injected `lcms-product` — działa gdy bieżący produkt jest rodzicem-kontenerem (`has_children: true`). Gdy użytkownik jest już na karcie wariantu, widget (opcjonalnie) może się ukryć — w V1 nie pobiera rodzeństwa z innej karty.

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `config.display_type` | string | `buttons` | `buttons` / `dropdown` / `swatches` |
| `config.label_source` | string | `binding_value` | `binding_value` / `variant_name` / `custom_attribute` |
| `config.label_attribute_code` | string | `""` | Kod atrybutu gdy `label_source = custom_attribute` (np. `material`) |
| `config.show_group_labels` | bool | `true` | Pokaż nazwę atrybutu binding przed opcjami |
| `config.hide_if_single` | bool | `true` | Ukryj widget gdy produkt ma <2 warianty |
| `heading.text` | string\|object | `""` | Nagłówek widgetu (multilang) |
| `group_label.color` | string | `var:dark` | Kolor etykiety grupy |
| `group_label.font_weight` | string | `600` | Waga fontu etykiety grupy |
| `option.background` | string | `null` | Tło opcji (normal) |
| `option.color` | string | `var:dark` | Tekst opcji (normal) |
| `option.background:hover` | string | `var:background-alt` | Tło opcji (hover) |
| `option.color:hover` | string | `null` | Tekst opcji (hover) |
| `option.border_color` | string | `var:border` | Ramka opcji |
| `selected.background` | string | `var:primary` | Tło wybranej opcji |
| `selected.color` | string | `var:white` | Tekst wybranej opcji |
| `selected.border_color` | string | `var:primary` | Ramka wybranej opcji |

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
