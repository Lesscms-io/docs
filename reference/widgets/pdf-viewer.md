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
| `widget` | object | Widget data |
| `widget.file` | string\|null | URL of the PDF file |
| `widget.height` | number | Viewer height in pixels (default: 600) |
| `widget.page_mode` | string | Page mode: `"single"`, `"double"` (default: `"double"`) |
| `widget.show_controls` | boolean | Show navigation controls (default: true) |
| `widget.show_thumbnails` | boolean | Show page thumbnails (default: true) |
| `widget.show_outline` | boolean | Show document outline (default: true) |
| `widget.show_fullscreen` | boolean | Allow fullscreen mode (default: true) |
| `widget.show_download` | boolean | Allow PDF download (default: true) |
| `widget.background_color` | string | Viewer background color (default: `"#1a1a1a"`) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "pdf-viewer",
  "uuid": "pdf-viewer-123",
  "widget": {
    "file": "https://cdn.example.com/documents/catalog.pdf",
    "height": 700,
    "page_mode": "double",
    "show_controls": true,
    "show_thumbnails": true,
    "show_outline": false,
    "show_fullscreen": true,
    "show_download": true,
    "background_color": "#1a1a1a"
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
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
  const { file, height, page_mode, show_controls, show_download, background_color } = widget.widget;

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
