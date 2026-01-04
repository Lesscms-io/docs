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
| `config` | object | Widget configuration |
| `config.percentage` | number | Progress percentage (0-100, default: 0) |
| `config.color` | string | Bar color (hex code) |
| `config.show_percentage` | boolean | Display percentage text (default: true) |
| `content` | object | Widget content |
| `content.title` | string | Progress bar label (localized) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "progress-bar",
  "uuid": "progress-123",
  "config": {
    "percentage": 75,
    "color": "#28a745",
    "show_percentage": true
  },
  "content": {
    "title": "Project Progress"
  },
  "settings": {
    "marginTop": 16,
    "marginBottom": 16
  }
}
```

## Multiple Progress Bars Example

```json
[
  {
    "widget_type": "progress-bar",
    "uuid": "skill-1",
    "config": {
      "percentage": 95,
      "color": "#E34F26",
      "show_percentage": true
    },
    "content": {
      "title": "HTML/CSS"
    }
  },
  {
    "widget_type": "progress-bar",
    "uuid": "skill-2",
    "config": {
      "percentage": 85,
      "color": "#F7DF1E",
      "show_percentage": true
    },
    "content": {
      "title": "JavaScript"
    }
  },
  {
    "widget_type": "progress-bar",
    "uuid": "skill-3",
    "config": {
      "percentage": 80,
      "color": "#4FC08D",
      "show_percentage": true
    },
    "content": {
      "title": "Vue.js"
    }
  }
]
```

## Usage Example

```javascript
// Render progress bar widget
function renderProgressBar(widget, language) {
  const { percentage, color, show_percentage } = widget.config;
  const { title } = widget.content;

  const progressId = `progress-${widget.uuid}`;

  return `
    <div id="${progressId}" class="progress-widget" data-percentage="${percentage}">
      ${title || show_percentage ? `
        <div class="progress-header">
          ${title ? `<span class="progress-title">${title}</span>` : ''}
          ${show_percentage ? `<span class="progress-percentage">0%</span>` : ''}
        </div>
      ` : ''}
      <div class="progress-track">
        <div class="progress-bar" style="
          width: 0%;
          background-color: ${color || '#007BFF'};
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
