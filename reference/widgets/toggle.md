# Toggle Widget

An accordion-style collapsible content section with title and expandable content.

## Widget Type

```
toggle
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `title` | object | No | Toggle header text (multilingual) |
| `content` | object | No | Collapsible content (multilingual, rich text HTML) |
| `default_open` | boolean | Yes | Whether toggle is expanded by default |

## Example Response

```json
{
  "widget_type": "toggle",
  "uuid": "toggle-123",
  "data": {
    "title": {
      "en": "What are your shipping options?",
      "pl": "Jakie są opcje dostawy?"
    },
    "content": {
      "en": "<p>We offer free shipping on orders over $50. Standard shipping takes 3-5 business days, while express shipping delivers within 1-2 business days.</p>",
      "pl": "<p>Oferujemy darmową dostawę przy zamówieniach powyżej 200 zł. Standardowa dostawa trwa 3-5 dni roboczych, a dostawa ekspresowa 1-2 dni robocze.</p>"
    },
    "default_open": false
  },
  "settings": {
    "borderColor": "#E0E0E0",
    "borderRadius": 8
  }
}
```

## Usage Example

```javascript
// Render toggle widget
function renderToggle(widget, language) {
  const { title, content, default_open } = widget.data;
  const settings = widget.settings || {};

  const titleText = title?.[language] || title?.en || '';
  const contentHtml = content?.[language] || content?.en || '';
  const isOpen = default_open || false;

  const toggleId = `toggle-${widget.uuid}`;

  return `
    <details class="toggle-widget" ${isOpen ? 'open' : ''} style="
      border: 1px solid ${settings.borderColor || '#E0E0E0'};
      border-radius: ${settings.borderRadius || 8}px;
      margin-bottom: 8px;
    ">
      <summary class="toggle-header" style="
        padding: 16px;
        cursor: pointer;
        font-weight: 600;
        display: flex;
        justify-content: space-between;
        align-items: center;
      ">
        ${titleText}
        <span class="toggle-icon">+</span>
      </summary>
      <div class="toggle-content" style="padding: 0 16px 16px;">
        ${contentHtml}
      </div>
    </details>
  `;
}
```

## JavaScript Enhancement

```javascript
// Enhance toggle with smooth animation
document.querySelectorAll('.toggle-widget').forEach(details => {
  const summary = details.querySelector('summary');
  const content = details.querySelector('.toggle-content');
  const icon = details.querySelector('.toggle-icon');

  summary.addEventListener('click', (e) => {
    e.preventDefault();

    if (details.open) {
      // Closing
      content.style.maxHeight = content.scrollHeight + 'px';
      requestAnimationFrame(() => {
        content.style.maxHeight = '0';
        icon.textContent = '+';
      });
      content.addEventListener('transitionend', () => {
        details.open = false;
      }, { once: true });
    } else {
      // Opening
      details.open = true;
      content.style.maxHeight = content.scrollHeight + 'px';
      icon.textContent = '−';
    }
  });
});
```

## CSS Example

```css
.toggle-widget {
  overflow: hidden;
}

.toggle-widget summary {
  list-style: none;
}

.toggle-widget summary::-webkit-details-marker {
  display: none;
}

.toggle-content {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s ease;
}

.toggle-widget[open] .toggle-content {
  max-height: 1000px;
}

.toggle-icon {
  font-size: 20px;
  transition: transform 0.3s;
}

.toggle-widget[open] .toggle-icon {
  transform: rotate(45deg);
}
```

## FAQ Accordion Pattern

For multiple toggles as an FAQ section:

```javascript
function renderFAQ(toggleWidgets, language) {
  return `
    <div class="faq-accordion">
      ${toggleWidgets.map(widget => renderToggle(widget, language)).join('')}
    </div>
  `;
}
```
