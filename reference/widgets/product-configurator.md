# Product Configurator Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Renders the configurable options defined on a product in LessCommerce (size, color, material, custom text, etc.) and lets the customer build their selection. Supports all four option display types (`select`, `radio`, `image_swatches`, `color_swatches`), price modifiers per option (`fixed_price` or `percentage`), required-group validation, and conditional visibility (option groups shown only when a specific parent option is selected). Adds the product to cart with the selected configuration stored in the cart item's `metadata.configured_options`.

## Widget Type

```
product-configurator
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-configurator"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_heading` | boolean | Show the section heading above the options |
| `widget.config.show_price_summary` | boolean | Show the running "Total:" row below the options |
| `widget.config.show_required_badge` | boolean | Show the `required` badge on mandatory option groups |
| `widget.config.slug_source` | string | Where product slug comes from: `"url"` or `"static"` |
| `widget.config.slug_url_segment` | number | Which URL segment holds the slug (when `slug_source="url"`) |
| `widget.config.slug` | string | Hard-coded product slug (when `slug_source="static"`) |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string \| object | Section heading label (multilingual) |
| `widget.heading.color` | string \| null | Heading color (CSS color or `var:<name>`) |
| `widget.heading.tag` | string | HTML tag for the heading: `h2`–`h5`, `p`, `span` |
| `widget.group_label` | object | Group label element group |
| `widget.group_label.color` | string \| null | Color for group labels ("Size", "Color"...) |
| `widget.group_label.required_color` | string \| null | Color for the `required` badge |
| `widget.option` | object | Option item element group |
| `widget.option.background` | string\|null | Background color of unselected options (radio/swatches) |
| `widget.option.background:hover` | string\|null | Background color on hover |
| `widget.option.border_color` | string\|null | Border color of unselected options |
| `widget.option.border_color:hover` | string\|null | Border color on hover |
| `widget.option.selected_background` | string\|null | Background of the selected option |
| `widget.option.selected_border_color` | string\|null | Border of the selected option |
| `widget.option.text_color` | string\|null | Text color on radio options |
| `widget.option.text_color:hover` | string\|null | Text color on hover |
| `widget.price_summary` | object | Price summary element group |
| `widget.price_summary.label_text` | string \| object | Label text next to the running total (multilingual) |
| `widget.price_summary.color` | string\|null | Color of the summary label |
| `widget.price_summary.amount_color` | string\|null | Color of the summary amount |
| `widget.add_to_cart_button` | object | Add to cart button element group |
| `widget.add_to_cart_button.text` | string \| object | Button label (multilingual) |
| `widget.add_to_cart_button.background` | string\|null | Button background |
| `widget.add_to_cart_button.background:hover` | string\|null | Button background on hover |
| `widget.add_to_cart_button.color` | string\|null | Button text color |
| `widget.add_to_cart_button.color:hover` | string\|null | Button text color on hover |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-configurator",
  "uuid": "product-configurator-001",
  "widget": {
    "config": {
      "show_heading": true,
      "show_price_summary": true,
      "show_required_badge": true,
      "slug_source": "url",
      "slug_url_segment": 2,
      "slug": ""
    },
    "heading": {
      "text": { "pl": "Skonfiguruj produkt", "en": "Configure your product" },
      "color": null,
      "tag": "h3"
    },
    "group_label": { "color": null, "required_color": null },
    "option": {
      "background": null,
      "background:hover": null,
      "border_color": null,
      "border_color:hover": null,
      "selected_background": null,
      "selected_border_color": null,
      "text_color": null,
      "text_color:hover": null
    },
    "price_summary": {
      "label_text": { "pl": "Razem:", "en": "Total:" },
      "color": null,
      "amount_color": null
    },
    "add_to_cart_button": {
      "text": { "pl": "Dodaj do koszyka", "en": "Add to cart" },
      "background": null,
      "background:hover": null,
      "color": null,
      "color:hover": null
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## LessCommerce Data Shape

The configurator reads `option_groups` from the storefront product response. Each option group declares a display type and a list of selectable options with optional price modifiers:

```json
{
  "uuid": "product-uuid",
  "price": 99.00,
  "option_groups": [
    {
      "uuid": "group-size",
      "name": "Rozmiar",
      "display_type": "radio",
      "is_required": true,
      "sort_order": 0,
      "visible_when_option_uuids": [],
      "options": [
        { "uuid": "opt-s", "name": "S", "price_modifier_type": null, "price_modifier_value": null, "color_hex": null, "thumbnail": null, "is_default": true, "sort_order": 0 },
        { "uuid": "opt-m", "name": "M", "price_modifier_type": null, "price_modifier_value": null, "color_hex": null, "thumbnail": null, "is_default": false, "sort_order": 1 },
        { "uuid": "opt-xl", "name": "XL", "price_modifier_type": "fixed_price", "price_modifier_value": 10.00, "color_hex": null, "thumbnail": null, "is_default": false, "sort_order": 2 }
      ]
    },
    {
      "uuid": "group-color",
      "name": "Kolor",
      "display_type": "color_swatches",
      "is_required": true,
      "sort_order": 1,
      "visible_when_option_uuids": [],
      "options": [
        { "uuid": "opt-red", "name": "Czerwony", "price_modifier_type": null, "price_modifier_value": null, "color_hex": "#e11d48", "thumbnail": null, "is_default": true, "sort_order": 0 },
        { "uuid": "opt-blue", "name": "Niebieski", "price_modifier_type": null, "price_modifier_value": null, "color_hex": "#2563eb", "thumbnail": null, "is_default": false, "sort_order": 1 }
      ]
    }
  ]
}
```

## Display Types

- `select` — native `<select>` dropdown, price delta shown in parentheses.
- `radio` — vertical list of radio cards, price delta shown on the right.
- `color_swatches` — grid of color squares using `color_hex`. Name shown in `title`/`aria-label`.
- `image_swatches` — grid of thumbnails using `thumbnail`. Falls back to first letter of the option name when no thumbnail.

## Price Calculation

Starting from the product's base `price`, each selected option contributes a delta:

- `price_modifier_type = "fixed_price"` → delta = `price_modifier_value`
- `price_modifier_type = "percentage"` → delta = `base_price * (price_modifier_value / 100)`
- `price_modifier_type = null` → delta = 0

The total updates live as the customer changes selections.

## Required Validation

Option groups with `is_required = true` must have a selection before the add-to-cart button enables. If the user clicks the button without filling required groups, a toast warns them.

## Conditional Visibility

When an option group's `visible_when_option_uuids` array is non-empty, the group is shown only if *any* currently-selected option's UUID is in that list (OR semantics). This lets you hide dependent groups (e.g. "engraving text" only after the user chose "add engraving — yes") and supports multiple trigger options across the parent group.

## Plugin Behaviors (CTA override)

LessCommerce plugins can attach a `plugin_behaviors[]` array to the product payload. Each entry targets a specific `(group_uuid, option_uuid)` combination; when the customer selects that option, the configurator replaces the default "Add to cart" button with a plugin-defined CTA.

```json
{
  "plugin_behaviors": [
    {
      "plugin_id": "photo-albums",
      "group_uuid": "group-files-method",
      "option_uuid": "opt-designer",
      "cta": {
        "type": "create_album_flow",
        "label": "Zaprojektuj album",
        "post_url": "/plugins/photo-albums/flows/start"
      }
    },
    {
      "plugin_id": "photo-albums",
      "group_uuid": "group-files-method",
      "option_uuid": "opt-email",
      "cta": {
        "type": "link",
        "label": "Napisz do nas",
        "url": "/kontakt"
      }
    }
  ]
}
```

CTA types:

- `create_album_flow` — widget POSTs to `cta.post_url` (storefront-relative, via `callPluginEndpoint`) with `{ product_id, return_url }` and redirects to the `redirect_url`/`designer_url` returned by the plugin.
- `link` — widget navigates to `cta.url` (any URL — internal or external).

The required-groups gate still applies: if required options are unfilled, the behavior CTA stays disabled.

## Cart Metadata

On add-to-cart, the selected configuration is stored on the cart item:

```json
{
  "product_uuid": "product-uuid",
  "quantity": 1,
  "metadata": {
    "configured_options": [
      {
        "group_uuid": "group-size",
        "group_name": "Rozmiar",
        "option_uuid": "opt-xl",
        "option_name": "XL",
        "price_delta": 10.00
      },
      {
        "group_uuid": "group-color",
        "group_name": "Kolor",
        "option_uuid": "opt-red",
        "option_name": "Czerwony",
        "price_delta": 0
      }
    ],
    "configured_total": 109.00
  }
}
```

## Placement

Typically placed on the product page (auto-generated `/product/:slug` route) below or alongside the `product-detail` widget. Can also be used on a dedicated landing page with `slug_source="static"`.
