# Countdown Widget

A countdown timer that displays remaining time until a target date.

## Widget Type

```
countdown
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"countdown"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.target_date` | string | Target date/time in ISO 8601 format |
| `config.show_days` | boolean | Display days component |
| `config.show_hours` | boolean | Display hours component |
| `config.show_minutes` | boolean | Display minutes component |
| `config.show_seconds` | boolean | Display seconds component |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "countdown",
  "uuid": "countdown-123",
  "config": {
    "target_date": "2025-12-31T23:59:59Z",
    "show_days": true,
    "show_hours": true,
    "show_minutes": true,
    "show_seconds": true
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
function renderCountdown(widget) {
  const { target_date, show_days, show_hours, show_minutes, show_seconds } = widget.config;

  const countdownId = `countdown-${widget.uuid}`;

  return `
    <div id="${countdownId}" class="countdown-widget" data-target="${target_date}">
      ${show_days ? '<div class="countdown-item"><span class="days">00</span><label>Days</label></div>' : ''}
      ${show_hours ? '<div class="countdown-item"><span class="hours">00</span><label>Hours</label></div>' : ''}
      ${show_minutes ? '<div class="countdown-item"><span class="minutes">00</span><label>Minutes</label></div>' : ''}
      ${show_seconds ? '<div class="countdown-item"><span class="seconds">00</span><label>Seconds</label></div>' : ''}
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
