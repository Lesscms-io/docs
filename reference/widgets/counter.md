# Counter Widget

An animated number counter that counts up to a target value.

## Widget Type

```
counter
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `number` | number | Yes | Target number to count to |
| `prefix` | object | No | Text before the number (multilingual) |
| `suffix` | object | No | Text after the number (multilingual) |
| `title` | object | No | Counter title/label (multilingual) |
| `duration` | number | Yes | Animation duration in milliseconds |

## Example Response

```json
{
  "widget_type": "counter",
  "uuid": "counter-123",
  "data": {
    "number": 5000,
    "prefix": {
      "en": "",
      "pl": ""
    },
    "suffix": {
      "en": "+",
      "pl": "+"
    },
    "title": {
      "en": "Happy Customers",
      "pl": "Zadowolonych klientów"
    },
    "duration": 2000
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
  "data": {
    "number": 1500000,
    "prefix": {
      "en": "$",
      "pl": ""
    },
    "suffix": {
      "en": "",
      "pl": " zł"
    },
    "title": {
      "en": "Revenue Generated",
      "pl": "Wygenerowany przychód"
    },
    "duration": 2500
  },
  "settings": {}
}
```

## Usage Example

```javascript
// Render counter widget
function renderCounter(widget, language) {
  const { number, prefix, suffix, title, duration } = widget.data;

  const prefixText = prefix?.[language] || prefix?.en || '';
  const suffixText = suffix?.[language] || suffix?.en || '';
  const titleText = title?.[language] || title?.en || '';

  const counterId = `counter-${widget.uuid}`;

  return `
    <div id="${counterId}" class="counter-widget"
      data-target="${number}"
      data-duration="${duration || 2000}"
    >
      <div class="counter-value">
        <span class="prefix">${prefixText}</span>
        <span class="number">0</span>
        <span class="suffix">${suffixText}</span>
      </div>
      ${titleText ? `<div class="counter-title">${titleText}</div>` : ''}
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

// Initialize with Intersection Observer (start animation when visible)
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

## Number Formatting

For large numbers, consider using locale-specific formatting:

```javascript
// Format with thousand separators
numberEl.textContent = currentValue.toLocaleString('en-US');
// Output: 1,500,000

// Format with European style
numberEl.textContent = currentValue.toLocaleString('de-DE');
// Output: 1.500.000
```
