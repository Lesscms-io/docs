# Language Switcher Widget

Links to the current page in the other languages of the project. Shown as a
dropdown or an inline list.

## Widget Type

```
language-switcher
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"language-switcher"` |
| `uuid` | string | Unique widget identifier |
| `widget.label` | object | Label element group |
| `widget.label.text` | object | Optional multilingual label shown before the switcher `{ "pl": "Język", "en": "Language" }` |
| `widget.label.color` | string\|null | Label and link color (color variable or hex, default: `"var:dark"`) |
| `widget.label.color:hover` | string\|null | Link color on hover and for the active language (default: `"var:info"`) |
| `widget.config` | object | Behavior configuration |
| `widget.config.display_mode` | string | `"names"` (Polski), `"codes"` (PL) or `"text-codes"` (Polski / PL). Default: `"names"` |
| `widget.config.layout` | string | `"dropdown"` or `"inline"`. Default: `"dropdown"` |
| `widget.config.hide_current` | boolean | Hide the current language from the list (default: `false`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "language-switcher",
  "uuid": "7d0c2f7e-0b8a-4f55-9d0e-3a3f1c2b9e10",
  "widget": {
    "label": {
      "text": { "pl": "Język", "en": "Language" },
      "color": "var:dark",
      "color:hover": "var:info"
    },
    "config": {
      "display_mode": "names",
      "layout": "dropdown",
      "hide_current": false
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0
  }
}
```

## How links are built

The widget does not call the API. The renderer computes the address of the
current page in each language and passes it to the widget (the same map is
used for `<link rel="alternate" hreflang>`):

- Collection entry: `metadata.urls[lang]` (see [Collections](../collections.md)).
- Page: `url_translation[lang]` (or `url`), prefixed with `/{lang}` for
  non-default languages.

Only languages in which the page has content are listed. A language without
content is skipped instead of linking to a 404.

## Single-language pages

If the current page exists in only one language, the widget renders nothing.

## Custom frontends

Build the list from `metadata.urls` of the entry (or `url_translation` of the
page) and show only the languages that have an address.
