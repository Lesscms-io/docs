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
| `config` | object | Widget configuration |
| `config.number` | number | Target number to count to (default: 0) |
| `config.duration` | number | Animation duration in milliseconds (default: 2000) |
| `content` | object | Widget content |
| `content.prefix` | string | Text before the number (localized) |
| `content.suffix` | string | Text after the number (localized) |
| `content.title` | string | Counter title/label (localized) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "counter",
  "uuid": "counter-123",
  "config": {
    "number": 5000,
    "duration": 2000
  },
  "content": {
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
  "config": {
    "number": 1500000,
    "duration": 2500
  },
  "content": {
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
  const { number, duration } = widget.config;
  const { prefix, suffix, title } = widget.content;

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
