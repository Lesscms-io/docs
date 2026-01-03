# Grid Widget

A layout container that arranges child widgets in a responsive grid with configurable columns.

## Widget Type

```
grid
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `columns` | array | Yes | Column configuration with width percentages and nested widgets |
| `gap` | number | Yes | Gap between columns in pixels |
| `stack_on_mobile` | boolean | Yes | Stack columns vertically on mobile |

### Column Structure

Each column in the `columns` array contains:

| Property | Type | Description |
|----------|------|-------------|
| `width` | number | Column width percentage (e.g., 50 for 50%) |
| `content` | array | Array of widgets inside this column |

## Example Response

```json
{
  "widget_type": "grid",
  "uuid": "grid-123",
  "data": {
    "columns": [
      {
        "width": 60,
        "content": [
          {
            "widget_type": "heading",
            "uuid": "heading-nested-1",
            "data": {
              "text": { "en": "Main Content", "pl": "Główna treść" },
              "level": 2
            }
          },
          {
            "widget_type": "text",
            "uuid": "text-nested-1",
            "data": {
              "content": {
                "en": "<p>This is the main content area.</p>",
                "pl": "<p>To jest główny obszar treści.</p>"
              }
            }
          }
        ]
      },
      {
        "width": 40,
        "content": [
          {
            "widget_type": "image",
            "uuid": "image-nested-1",
            "data": {
              "image_source": "static",
              "image": {
                "url": "https://cdn.example.com/sidebar-image.jpg",
                "alt": { "en": "Sidebar image" }
              }
            }
          }
        ]
      }
    ],
    "gap": 24,
    "stack_on_mobile": true
  },
  "settings": {
    "paddingTop": 40,
    "paddingBottom": 40
  }
}
```

## Common Layouts

### Two Equal Columns (50/50)

```json
{
  "columns": [
    { "width": 50, "content": [...] },
    { "width": 50, "content": [...] }
  ]
}
```

### Three Columns (33/33/33)

```json
{
  "columns": [
    { "width": 33.33, "content": [...] },
    { "width": 33.33, "content": [...] },
    { "width": 33.33, "content": [...] }
  ]
}
```

### Sidebar Layout (70/30)

```json
{
  "columns": [
    { "width": 70, "content": [...] },
    { "width": 30, "content": [...] }
  ]
}
```

## Usage Example

```javascript
// Render grid widget (recursively renders nested widgets)
function renderGrid(widget, language, renderWidget) {
  const { columns, gap, stack_on_mobile } = widget.data;
  const settings = widget.settings || {};

  if (!columns || columns.length === 0) {
    return '';
  }

  const columnElements = columns.map((column, index) => {
    const nestedContent = (column.content || [])
      .map(nestedWidget => renderWidget(nestedWidget, language))
      .join('');

    return `
      <div class="grid-column" style="
        flex: 0 0 ${column.width}%;
        max-width: ${column.width}%;
      ">
        ${nestedContent}
      </div>
    `;
  }).join('');

  return `
    <div class="grid-widget ${stack_on_mobile ? 'stack-mobile' : ''}" style="
      display: flex;
      flex-wrap: wrap;
      gap: ${gap || 16}px;
    ">
      ${columnElements}
    </div>
  `;
}
```

## CSS for Responsive Behavior

```css
.grid-widget {
  display: flex;
  flex-wrap: wrap;
}

.grid-column {
  box-sizing: border-box;
}

/* Stack columns on mobile when enabled */
@media (max-width: 768px) {
  .grid-widget.stack-mobile .grid-column {
    flex: 0 0 100% !important;
    max-width: 100% !important;
  }
}

/* Tablet: 2-column max for multi-column layouts */
@media (max-width: 1024px) and (min-width: 769px) {
  .grid-widget.stack-mobile .grid-column {
    flex: 0 0 50% !important;
    max-width: 50% !important;
  }
}
```

## Nested Widgets

The Grid widget supports any widget type in its columns, including other Grid widgets for complex layouts:

```json
{
  "widget_type": "grid",
  "data": {
    "columns": [
      {
        "width": 50,
        "content": [
          {
            "widget_type": "grid",
            "data": {
              "columns": [
                { "width": 50, "content": [...] },
                { "width": 50, "content": [...] }
              ]
            }
          }
        ]
      },
      {
        "width": 50,
        "content": [...]
      }
    ]
  }
}
```

> **Note**: Deeply nested grids can become complex. Consider using CSS Grid or Flexbox utilities for simpler responsive layouts.
