# Progress Bar Widget

An animated progress bar showing percentage completion.

## Widget Type

```
progress-bar
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `title` | object | No | Progress bar label (multilingual) |
| `percentage` | number | Yes | Progress percentage (0-100) |
| `color` | string | Yes | Bar color (hex code) |
| `show_percentage` | boolean | Yes | Display percentage text |

## Example Response

```json
{
  "widget_type": "progress-bar",
  "uuid": "progress-123",
  "data": {
    "title": {
      "en": "Project Progress",
      "pl": "Postęp projektu"
    },
    "percentage": 75,
    "color": "#28a745",
    "show_percentage": true
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
    "data": {
      "title": { "en": "HTML/CSS", "pl": "HTML/CSS" },
      "percentage": 95,
      "color": "#E34F26",
      "show_percentage": true
    }
  },
  {
    "widget_type": "progress-bar",
    "uuid": "skill-2",
    "data": {
      "title": { "en": "JavaScript", "pl": "JavaScript" },
      "percentage": 85,
      "color": "#F7DF1E",
      "show_percentage": true
    }
  },
  {
    "widget_type": "progress-bar",
    "uuid": "skill-3",
    "data": {
      "title": { "en": "Vue.js", "pl": "Vue.js" },
      "percentage": 80,
      "color": "#4FC08D",
      "show_percentage": true
    }
  }
]
```

## Usage Example

```javascript
// Render progress bar widget
function renderProgressBar(widget, language) {
  const { title, percentage, color, show_percentage } = widget.data;

  const titleText = title?.[language] || title?.en || '';
  const progressId = `progress-${widget.uuid}`;

  return `
    <div id="${progressId}" class="progress-widget" data-percentage="${percentage}">
      ${titleText || show_percentage ? `
        <div class="progress-header">
          ${titleText ? `<span class="progress-title">${titleText}</span>` : ''}
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
