# Countdown Widget

A countdown timer that displays remaining time until a target date. Uses nested element-group structure.

## Widget Type

```
countdown
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"countdown"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Configuration element |
| `widget.config.target_date` | string | Target date/time in ISO 8601 format |
| `widget.config.show_days` | boolean | Display days component (default: true) |
| `widget.config.show_hours` | boolean | Display hours component (default: true) |
| `widget.config.show_minutes` | boolean | Display minutes component (default: true) |
| `widget.config.show_seconds` | boolean | Display seconds component (default: true) |
| `widget.config.separator` | string | Separator character between time units (default: `":"`) |
| `widget.value` | object | Value (number) styling element |
| `widget.value.color` | string\|null | Color of the countdown numbers |
| `widget.value.color:hover` | string\|null | Color of the countdown numbers on hover |
| `widget.label` | object | Label styling element |
| `widget.label.color` | string\|null | Color of the unit labels (days, hours, etc.) |
| `widget.label.color:hover` | string\|null | Color of the unit labels on hover |
| `widget.item` | object | Item container styling element |
| `widget.item.background` | string\|null | Background color of each countdown unit box |
| `widget.item.background:hover` | string\|null | Background color of each countdown unit box on hover |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "countdown",
  "uuid": "countdown-123",
  "widget": {
    "config": {
      "target_date": "2025-12-31T23:59:59Z",
      "show_days": true,
      "show_hours": true,
      "show_minutes": true,
      "show_seconds": true,
      "separator": ":"
    },
    "value": {
      "color": "var:dark",
      "color:hover": null
    },
    "label": {
      "color": "var:muted",
      "color:hover": null
    },
    "item": {
      "background": "var:light",
      "background:hover": null
    }
  },
  "settings": {
    "horizontalAlign": "center",
    "paddingTop": 40,
    "paddingBottom": 40,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderCountdown(widget, language) {
  const { config, value, label, item } = widget.widget;

  const countdownId = `countdown-${widget.uuid}`;
  const separator = config.separator || ':';

  const valueStyle = value.color ? `color: ${value.color};` : '';
  const labelStyle = label.color ? `color: ${label.color};` : '';
  const itemStyle = item.background ? `background-color: ${item.background};` : '';

  const units = [];
  if (config.show_days !== false) units.push({ class: 'days', label: 'Days' });
  if (config.show_hours !== false) units.push({ class: 'hours', label: 'Hours' });
  if (config.show_minutes !== false) units.push({ class: 'minutes', label: 'Minutes' });
  if (config.show_seconds !== false) units.push({ class: 'seconds', label: 'Seconds' });

  return `
    <div id="${countdownId}" class="countdown-widget" data-target="${config.target_date}">
      ${units.map((unit, i) => `
        <div class="countdown-item" style="${itemStyle}">
          <span class="countdown-value ${unit.class}" style="${valueStyle}">00</span>
          <span class="countdown-label" style="${labelStyle}">${unit.label}</span>
        </div>
        ${i < units.length - 1 ? `<span class="countdown-separator">${separator}</span>` : ''}
      `).join('')}
    </div>
  `;
}

// Initialize countdown timer
function initCountdown(element) {
  const targetDate = new Date(element.dataset.target);

  function updateCountdown() {
    const now = new Date();
    const diff = targetDate - now;

    if (diff <= 0) {
      element.innerHTML = '<div class="countdown-expired">Event has started!</div>';
      return;
    }

    const days = Math.floor(diff / (1000 * 60 * 60 * 24));
    const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
    const seconds = Math.floor((diff % (1000 * 60)) / 1000);

    const daysEl = element.querySelector('.days');
    const hoursEl = element.querySelector('.hours');
    const minutesEl = element.querySelector('.minutes');
    const secondsEl = element.querySelector('.seconds');

    if (daysEl) daysEl.textContent = String(days).padStart(2, '0');
    if (hoursEl) hoursEl.textContent = String(hours).padStart(2, '0');
    if (minutesEl) minutesEl.textContent = String(minutes).padStart(2, '0');
    if (secondsEl) secondsEl.textContent = String(seconds).padStart(2, '0');
  }

  updateCountdown();
  setInterval(updateCountdown, 1000);
}

document.querySelectorAll('.countdown-widget').forEach(initCountdown);
```
