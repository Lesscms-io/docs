# Counter Widget

An animated number counter that counts up to a target value.

## Widget Type

```
counter
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"counter"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.number` | number | Target number to count to (default: 0) |
| `widget.duration` | number | Animation duration in milliseconds (default: 2000) |
| `widget.alignment` | string | Text alignment: `"left"`, `"center"`, `"right"` (default: `"center"`) |
| `widget.number_size` | string | Number font size preset (default: `"xl"`) |
| `widget.number_color` | string\|null | Number text color (hex code or null for default) |
| `widget.title_color` | string\|null | Title text color (hex code or null for default) |
| `widget.prefix_color` | string\|null | Prefix/suffix text color (hex code or null for default) |
| `widget.hover_number_color` | string\|null | Number text color on hover |
| `widget.hover_title_color` | string\|null | Title text color on hover |
| `widget.hover_prefix_color` | string\|null | Prefix/suffix text color on hover |
| `widget.hover_lift` | number | Hover lift in pixels — translateY offset (default: `0`) |
| `widget.hover_scale` | number | Hover scale factor (default: `1`) |
| `widget.hover_shadow` | string | Hover shadow preset: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"none"`) |
| `widget.transition_duration` | number | Hover transition duration in ms (default: 200) |
| `widget.prefix` | string | Text before the number (localized) |
| `widget.suffix` | string | Text after the number (localized) |
| `widget.title` | string | Counter title/label (localized) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "counter",
  "uuid": "counter-123",
  "widget": {
    "number": 5000,
    "duration": 2000,
    "alignment": "center",
    "number_size": "xl",
    "number_color": null,
    "title_color": null,
    "prefix_color": null,
    "hover_number_color": null,
    "hover_title_color": null,
    "hover_prefix_color": null,
    "hover_lift": 0,
    "hover_scale": 1,
    "hover_shadow": "none",
    "transition_duration": 200,
    "prefix": "",
    "suffix": "+",
    "title": "Happy Customers"
  },
  "settings": {
    "textAlign": "center"
  }
}
```

## Example Response (Currency)

```json
{
  "widget_type": "counter",
  "uuid": "counter-456",
  "widget": {
    "number": 1500000,
    "duration": 2500,
    "alignment": "center",
    "number_size": "xl",
    "number_color": "#50a5f1",
    "title_color": null,
    "prefix_color": "#50a5f1",
    "hover_number_color": null,
    "hover_title_color": null,
    "hover_prefix_color": null,
    "hover_lift": 0,
    "hover_scale": 1,
    "hover_shadow": "none",
    "transition_duration": 200,
    "prefix": "$",
    "suffix": "",
    "title": "Revenue Generated"
  },
  "settings": {}
}
```

## Usage Example

```javascript
function renderCounter(widget, language) {
  const { number, duration, alignment, number_size, number_color, title_color, prefix_color } = widget.widget;
  const { prefix, suffix, title } = widget.widget;

  const counterId = `counter-${widget.uuid}`;

  return `
    <div id="${counterId}" class="counter-widget"
      data-target="${number}"
      data-duration="${duration || 2000}"
    >
      <div class="counter-value">
        <span class="prefix">${prefix || ''}</span>
        <span class="number">0</span>
        <span class="suffix">${suffix || ''}</span>
      </div>
      ${title ? `<div class="counter-title">${title}</div>` : ''}
    </div>
  `;
}

// Animate counter
function animateCounter(element) {
  const target = parseInt(element.dataset.target);
  const duration = parseInt(element.dataset.duration);
  const numberEl = element.querySelector('.number');

  const startTime = performance.now();

  function update(currentTime) {
    const elapsed = currentTime - startTime;
    const progress = Math.min(elapsed / duration, 1);

    // Ease out quad
    const easeProgress = 1 - (1 - progress) * (1 - progress);

    const currentValue = Math.round(easeProgress * target);
    numberEl.textContent = currentValue.toLocaleString();

    if (progress < 1) {
      requestAnimationFrame(update);
    }
  }

  requestAnimationFrame(update);
}

// Initialize with Intersection Observer
function initCounters() {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        animateCounter(entry.target);
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.5 });

  document.querySelectorAll('.counter-widget').forEach(el => {
    observer.observe(el);
  });
}

initCounters();
```

## Multi-Item Support

The counter widget supports displaying multiple counters in a grid. When multiple items are configured, the API returns a multi-item structure:

| Property | Type | Description |
|----------|------|-------------|
| `multi_item` | boolean | `true` when widget has multiple items |
| `multi_columns` | number | Number of grid columns (1-4) |
| `multi_gap` | number | Gap between items in pixels (default: 16) |
| `items` | array | Array of individual counter widgets |

### Multi-Item Example Response

```json
{
  "widget_type": "counter",
  "uuid": "counter-789",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "counter",
      "widget": { "number": 5000, "duration": 2000, "alignment": "center", "number_size": "xl", "prefix": "$", "suffix": "+", "title": "Revenue" }
    },
    {
      "widget_type": "counter",
      "widget": { "number": 1200, "duration": 2000, "alignment": "center", "number_size": "xl", "prefix": "", "suffix": "+", "title": "Projects" }
    },
    {
      "widget_type": "counter",
      "widget": { "number": 50, "duration": 2000, "alignment": "center", "number_size": "xl", "prefix": "", "suffix": "", "title": "Countries" }
    }
  ],
  "settings": {}
}
```

### Rendering Multi-Item

```javascript
function renderCounterWidget(widget) {
  if (widget.multi_item) {
    return `
      <div style="display: grid; grid-template-columns: repeat(${widget.multi_columns}, 1fr); gap: ${widget.multi_gap}px;">
        ${widget.items.map(item => renderCounter(item)).join('')}
      </div>
    `;
  }
  return renderCounter(widget);
}
```

## CSS Example

```css
.counter-widget {
  text-align: center;
  padding: 20px;
}

.counter-value {
  font-size: 48px;
  font-weight: 700;
  color: #333;
}

.counter-value .prefix,
.counter-value .suffix {
  font-size: 36px;
}

.counter-title {
  font-size: 16px;
  color: #666;
  margin-top: 8px;
}
```
