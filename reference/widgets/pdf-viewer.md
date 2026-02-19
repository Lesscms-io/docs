# PDF Viewer Widget

A flipbook-style PDF viewer with configurable controls.

## Widget Type

```
pdf-viewer
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"pdf-viewer"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.file` | string\|null | URL of the PDF file |
| `config.height` | number | Viewer height in pixels (default: 600) |
| `config.page_mode` | string | Page mode: `"single"`, `"double"` |
| `config.show_controls` | boolean | Show navigation controls |
| `config.show_thumbnails` | boolean | Show page thumbnails |
| `config.show_outline` | boolean | Show document outline |
| `config.show_fullscreen` | boolean | Allow fullscreen mode |
| `config.show_download` | boolean | Allow PDF download |
| `config.background_color` | string | Viewer background color |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "pdf-viewer",
  "uuid": "pdf-viewer-123",
  "config": {
    "file": "https://cdn.example.com/documents/catalog.pdf",
    "height": 700,
    "page_mode": "double",
    "show_controls": true,
    "show_thumbnails": true,
    "show_outline": false,
    "show_fullscreen": true,
    "show_download": true,
    "background_color": "#1a1a1a"
  }
}
```

## Page Mode

| Value | Description |
|-------|-------------|
| `single` | One page at a time |
| `double` | Two-page spread (book view) |

## Usage Example

```javascript
function renderPdfViewer(widget) {
  const { file, height, page_mode, show_controls, show_download, background_color } = widget.config;

  if (!file) {
    return '<div class="pdf-placeholder">No PDF file selected</div>';
  }

  // For dFlip library integration
  return `
    <div class="pdf-viewer"
         style="height: ${height || 600}px; background-color: ${background_color || '#1a1a1a'}; border-radius: 8px; overflow: hidden;"
         data-pdf="${file}"
         data-page-mode="${page_mode || 'double'}"
         data-controls="${show_controls !== false}"
         data-download="${show_download !== false}">
    </div>
  `;
}
```

## Note

The PDF viewer uses the dFlip library for flipbook rendering. Make sure to include the library in your frontend application for full functionality.
