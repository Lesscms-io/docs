# Block Content Widget

A widget that renders a reusable content block defined in LessCMS. Blocks are shared content sections that can be embedded on multiple pages.

## Widget Type

```
block-content
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"block-content"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.block_code` | string | Code of the block to render |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "block-content",
  "uuid": "block-123",
  "config": {
    "block_code": "footer-cta"
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## How It Works

The block-content widget references a block by its code. To render it:

1. Get the `block_code` from the widget config
2. Fetch the block content from the Blocks API:

```
GET /v1/:workspace/:project/blocks/:block_code
```

3. Render the block's sections, columns, and widgets (same structure as pages)

See [Blocks API](../blocks.md) for the full block response structure.

## Usage Example

```javascript
async function renderBlockContent(widget, language, api) {
  const { block_code } = widget.config;

  if (!block_code) return '';

  const block = await api.getBlock(block_code);
  if (!block) return '';

  // Block has the same structure as a page (sections > columns > widgets)
  return renderSections(block.sections, language);
}
```

## Use Cases

- **Shared footers**: Same footer block across all pages
- **CTA sections**: Reusable call-to-action blocks
- **Headers**: Shared navigation/header blocks
- **Sidebars**: Common sidebar content
