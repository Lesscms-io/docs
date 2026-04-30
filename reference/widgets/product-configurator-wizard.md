# Product Configurator Wizard Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Step-by-step variant of `product-configurator`. Each option group is rendered on its own step with prev/next navigation and a progress bar. Hidden groups (per visibility rule) are skipped. The final step shows a summary and the Add to cart button.

## Widget Type

```
product-configurator-wizard
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-configurator-wizard"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_progress` | boolean | Show the progress bar above the current step |
| `widget.config.show_step_count` | boolean | Show "Step N of M" label under the progress bar |
| `widget.config.slug_source` | string | Where product slug comes from: `"url"` or `"static"` |
| `widget.config.slug_url_segment` | number | Which URL segment holds the slug (when `slug_source="url"`) |
| `widget.config.slug` | string | Hard-coded product slug (when `slug_source="static"`) |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string \| object | Section heading label (multilingual) |
| `widget.heading.tag` | string | HTML tag for the heading |
| `widget.add_to_cart_button` | object | Add to cart button element group |
| `widget.add_to_cart_button.text` | string \| object | Button label (multilingual) |
| `settings` | object | Common widget styles (padding/margin/background/etc.) |

## Visibility Rules

The wizard reads each option group's and each option's `visibility_rule` field to decide whether to show that step / option. The rule is in CNF form:

```json
{
  "and_groups": [
    ["option-uuid-A", "option-uuid-B"],
    ["option-uuid-C"]
  ]
}
```

The element is visible iff for every `and_group`, at least one of its UUIDs is in the current selection set. An empty/null rule = always visible. Legacy `visible_when_option_uuids` (flat OR list) is honored as a fallback.

## Example Response

```json
{
  "widget_type": "product-configurator-wizard",
  "uuid": "abc-123",
  "widget": {
    "config": {
      "show_progress": true,
      "show_step_count": true,
      "slug_source": "url",
      "slug_url_segment": 2,
      "slug": ""
    },
    "heading": {
      "text": { "pl": "Skonfiguruj produkt", "en": "Configure your product" },
      "tag": "h3"
    },
    "add_to_cart_button": {
      "text": { "pl": "Dodaj do koszyka", "en": "Add to cart" }
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20
  }
}
```

## Notes

- The product itself is fetched at render time by the Vue component from the storefront API — this transformer only shapes the widget's UI config.
- Same `option_groups` payload is consumed as the flat `product-configurator` widget, so switching between layouts is non-destructive.
