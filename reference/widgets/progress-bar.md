# Progress Bar Widget

An animated progress bar showing percentage completion.

## Widget Type

```
progress-bar
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"progress-bar"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.bar` | object | Bar element group |
| `widget.bar.color` | string\|null | Bar color (`var:` variable or hex, default: `"var:primary"`) |
| `widget.bar.color:hover` | string\|null | Bar color on hover |
| `widget.bar.percentage` | number | Progress percentage 0-100 (default: 0) |
| `widget.bar.show_percentage` | boolean | Display percentage text (default: true) |
| `widget.title` | object | Title element group |
| `widget.title.html` | string\|object | Progress bar label (localized) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "progress-bar",
  "uuid": "progress-123",
  "widget": {
    "bar": {
      "color": "var:primary",
      "color:hover": null,
      "percentage": 75,
      "show_percentage": true
    },
    "title": {
      "html": {"en": "Project Progress", "pl": "Postep projektu"}
    }
  },
  "settings": {
    "marginTop": 16,
    "marginBottom": 16
  }
}
```

## Usage Example

```javascript
// Render progress bar widget
function renderProgressBar(widget, language) {
  const { bar, title } = widget.widget;
  const percentage = bar.percentage || 0;
  const color = bar.color || 'var:primary';
  const showPercentage = bar.show_percentage !== false;
  const titleText = typeof title.html === 'object' ? title.html[language] : title.html;

  const progressId = `progress-${widget.uuid}`;

  return `
    <div id="${progressId}" class="progress-widget" data-percentage="${percentage}">
      ${titleText || showPercentage ? `
        <div class="progress-header">
          ${titleText ? `<span class="progress-title">${titleText}</span>` : ''}
          ${showPercentage ? `<span class="progress-percentage">0%</span>` : ''}
        </div>
      ` : ''}
      <div class="progress-track">
        <div class="progress-bar" style="
          width: 0%;
          background-color: ${color};
        "></div>
      </div>
    </div>
  `;
}

// Animate progress bar
function animateProgressBar(element) {
  const percentage = parseInt(element.dataset.percentage);
  const bar = element.querySelector('.progress-bar');
  const percentageText = element.querySelector('.progress-percentage');

  const duration = 1500;
  const startTime = performance.now();

  function update(currentTime) {
    const elapsed = currentTime - startTime;
    const progress = Math.min(elapsed / duration, 1);

    // Ease out quad
    const easeProgress = 1 - (1 - progress) * (1 - progress);
    const currentPercent = Math.round(easeProgress * percentage);

    bar.style.width = currentPercent + '%';
    if (percentageText) {
      percentageText.textContent = currentPercent + '%';
    }

    if (progress < 1) {
      requestAnimationFrame(update);
    }
  }

  requestAnimationFrame(update);
}

// Initialize with Intersection Observer
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      animateProgressBar(entry.target);
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.5 });

document.querySelectorAll('.progress-widget').forEach(el => {
  observer.observe(el);
});
```

## CSS Example

```css
.progress-widget {
  margin: 16px 0;
}

.progress-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.progress-title {
  font-weight: 500;
  color: #333;
}

.progress-percentage {
  font-weight: 600;
  color: #666;
}

.progress-track {
  height: 8px;
  background: #E9ECEF;
  border-radius: 4px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  border-radius: 4px;
  transition: width 0.3s ease;
}
```
