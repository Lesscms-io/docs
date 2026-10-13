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
| `widget.number` | object | Number element group |
| `widget.number.number` | number | Target number to count to (default: 0) |
| `widget.number.from` | number\|null | Count-up start value (null = auto: target − 20 if target > 1000, else 0) |
| `widget.number.duration` | number | Animation duration in milliseconds (default: 1600) |
| `widget.number.decimals` | number\|null | Decimal places (null = derived from `number`) |
| `widget.number.separator` | string | Thousands separator: `"none"`, `"space"`, `"dot"`, `"comma"` (default: `"none"`) |
| `widget.number.style` | string | `"fill"` (solid) or `"outline"` (stroked digits, filled on hover) (default: `"fill"`) |
| `widget.number.outline_width` | number | Outline stroke width in px, used when `style` is `"outline"` (default: 1.5) |
| `widget.number.font_stretch` | number\|null | Letter width in percent (null = preset/default) |
| `widget.number.text_style` | string | Project text style preset code (`lcms-ts-<code>` class), empty = none |
| `widget.number.size` | string | Number font size preset: `"md"`, `"lg"`, `"xl"`, `"2xl"` (default: `"xl"`) |
| `widget.number.tag` | string | HTML tag for the number element (default: `"p"`) |
| `widget.number.prefix` | string | Text before the number (multilingual) |
| `widget.number.suffix` | string | Text after the number (multilingual) |
| `widget.number.color` | string\|null | Number text color (hex, `var:` variable, or null) |
| `widget.number.color:hover` | string\|null | Number text color on hover |
| `widget.number.prefix_color` | string\|null | Prefix/suffix text color (hex, `var:` variable, or null) |
| `widget.label` | object | Label element group (legacy `title` is read as fallback) |
| `widget.label.text` | string | Label text under the number (multilingual) |
| `widget.label.tag` | string | HTML tag for the label element (default: `"p"`) |
| `widget.label.color` | string\|null | Label text color (hex, `var:` variable, or null) |
| `widget.label.color:hover` | string\|null | Label text color on hover |
| `widget.config` | object | Configuration group |
| `widget.config.alignment` | string | Text alignment: `"left"`, `"center"`, `"right"` (default: `"center"`) |
| `widget.config.gap` | number | Gap between number and label in pixels (default: 10) |
| `widget.config.animate_on_scroll` | boolean | Count up when the widget enters the viewport; false = static value (default: true) |
| `widget.config.once` | boolean | Count up only the first time (true) or on every entry into the viewport (default: true) |
| `settings` | object | Style settings (see [shared settings](shared-settings.md)) |

## Example Response

```json
{
  "widget_type": "counter",
  "uuid": "counter-123",
  "widget": {
    "number": {
      "number": 5000,
      "duration": 2000,
      "size": "xl",
      "tag": "p",
      "prefix": "",
      "suffix": "+",
      "color": null,
      "color:hover": null,
      "prefix_color": null,
      "from": null,
      "decimals": null,
      "separator": "none",
      "style": "outline",
      "outline_width": 1.5,
      "font_stretch": null,
      "text_style": ""
    },
    "label": {
      "text": "Happy Customers",
      "tag": "p",
      "color": null,
      "color:hover": null
    },
    "config": {
      "alignment": "center",
      "gap": 10,
      "animate_on_scroll": true,
      "once": true
    }
  },
  "settings": {}
}
```

## Example Response (Currency)

```json
{
  "widget_type": "counter",
  "uuid": "counter-456",
  "widget": {
    "number": {
      "number": 1500000,
      "duration": 2500,
      "size": "xl",
      "tag": "p",
      "prefix": "$",
      "suffix": "",
      "color": "#50a5f1",
      "color:hover": null,
      "prefix_color": "#50a5f1"
    },
    "label": {
      "text": "Revenue Generated",
      "tag": "p",
      "color": null,
      "color:hover": null
    },
    "config": {
      "alignment": "center",
      "gap": 10
    }
  },
  "settings": {}
}
```

## Usage Example

```javascript
function renderCounter(widget, language) {
  const { number, label, config } = widget.widget;

  const counterId = `counter-${widget.uuid}`;

  return `
    <div id="${counterId}" class="counter-widget"
      style="text-align: ${config.alignment || 'center'}"
      data-target="${number.number}"
      data-duration="${number.duration || 1600}"
    >
      <div class="counter-value"
        ${number.color ? `style="color: ${number.color}"` : ''}
      >
        <span class="prefix"
          ${number.prefix_color ? `style="color: ${number.prefix_color}"` : ''}
        >${number.prefix || ''}</span>
        <span class="number">0</span>
        <span class="suffix"
          ${number.prefix_color ? `style="color: ${number.prefix_color}"` : ''}
        >${number.suffix || ''}</span>
      </div>
      ${label.text ? `<div class="counter-title"
        ${label.color ? `style="color: ${label.color}"` : ''}
      >${label.text}</div>` : ''}
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
