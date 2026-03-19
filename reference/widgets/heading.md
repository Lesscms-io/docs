# Heading Widget

A text heading widget supporting h1-h6 levels with optional dynamic content from collections.

## Widget Type

```
heading
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"heading"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.heading` | object | Heading element-group |
| `widget.heading.html` | object | Multilingual HTML content (`{ "en": "<h2>...</h2>", "pl": "<h2>...</h2>" }`) |
| `widget.heading.color` | string\|null | Heading text color (CSS color or `"var:colorName"` variable reference) |
| `widget.heading.color:hover` | string\|null | Heading text color on hover |
| `widget.config` | object | Configuration settings |
| `widget.config.content_source` | string | `"static"` or `"dynamic"` — content source mode |
| `widget.config.level` | number | Heading level (1-6, default: 2) |
| `widget.config.collection_code` | string\|null | Collection code (dynamic mode, when entry not resolved) |
| `widget.config.field_code` | string\|null | Field code (dynamic mode, when entry not resolved) |
| `widget.config.entry_id` | string\|null | Entry ID (dynamic mode, when entry not resolved) |
| `settings` | object | Style settings (optional) |

## Example Response (Static)

```json
{
  "widget_type": "heading",
  "uuid": "2dbc3aaa-c75f-4141-b547-e5afb921dbb6",
  "widget": {
    "heading": {
      "html": {
        "pl": "<h2 style=\"text-align: center\">Nagłówek</h2>",
        "en": "<h2 style=\"text-align: center\">Heading</h2>"
      },
      "color": null,
      "color:hover": null
    },
    "config": {
      "content_source": "static",
      "level": 2
    }
  },
  "settings": {
    "paddingTop": 8,
    "paddingBottom": 8,
    "horizontalAlign": "center",
    "responsive": {
      "tablet": {
        "horizontalAlign": "center"
      },
      "mobile": {
        "horizontalAlign": "center"
      }
    }
  }
}
```

## Example Response (Dynamic — resolved)

When in dynamic mode and the entry was successfully fetched server-side:

```json
{
  "widget_type": "heading",
  "uuid": "2dbc3aaa-c75f-4141-b547-e5afb921dbb6",
  "widget": {
    "heading": {
      "html": {
        "pl": "Dynamiczny tytuł z kolekcji",
        "en": "Dynamic title from collection"
      },
      "color": "var:primary",
      "color:hover": "var:dark"
    },
    "config": {
      "content_source": "dynamic",
      "level": 1
    }
  },
  "settings": {}
}
```

## Example Response (Dynamic — unresolved)

When in dynamic mode but entry context was not available (e.g. outside collection page), reference fields are passed for client-side resolution:

```json
{
  "widget_type": "heading",
  "uuid": "2dbc3aaa-c75f-4141-b547-e5afb921dbb6",
  "widget": {
    "heading": {
      "color": null,
      "color:hover": null
    },
    "config": {
      "content_source": "dynamic",
      "level": 2,
      "collection_code": "articles",
      "field_code": "title",
      "entry_id": "abc-123"
    }
  },
  "settings": {}
}
```

## Color Values

The `color` and `color:hover` fields support two formats:

- **CSS color**: Any valid CSS color string (e.g. `"#333333"`, `"rgb(0,0,0)"`)
- **Variable reference**: `"var:colorName"` or `"var:colorName:opacity"` — references a project color variable (e.g. `"var:dark"`, `"var:primary:80"`)

## Usage Example

```javascript
async function renderHeading(widget, language, api) {
  const { heading, config } = widget.widget;
  const level = config?.level || 2;
  const tag = `h${level}`;
  let text = '';

  if (config?.content_source === 'dynamic') {
    if (heading?.html) {
      // Entry was resolved server-side
      text = heading.html[language] || heading.html.en || '';
    } else if (config.collection_code && config.entry_id) {
      // Need to fetch client-side
      const entry = await api.getEntry(config.collection_code, config.entry_id);
      text = entry.data[config.field_code]?.[language] || '';
    }
  } else {
    text = heading?.html?.[language] || heading?.html?.en || '';
  }

  const style = heading?.color ? `color: ${resolveColor(heading.color)}` : '';

  return `<${tag} style="${style}">${text}</${tag}>`;
}
```
