# Table Widget

A data table with customizable headers and rows.

## Widget Type

```
table
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"table"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.headers` | array | Table headers |
| `widget.headers[].text` | object | Multilingual header text |
| `widget.rows` | array | Table rows (array of arrays) |
| `widget.header_bg` | string\|null | Header background color |
| `widget.header_text` | string | Header text color: `"light"`, `"dark"` |
| `widget.striped` | boolean | Alternating row colors |
| `widget.bordered` | boolean | Show cell borders |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "table",
  "uuid": "table-123",
  "widget": {
    "headers": [
      { "text": { "en": "Feature", "pl": "Funkcja" } },
      { "text": { "en": "Free", "pl": "Darmowy" } },
      { "text": { "en": "Pro", "pl": "Pro" } }
    ],
    "rows": [
      [
        { "en": "Projects", "pl": "Projekty" },
        { "en": "1", "pl": "1" },
        { "en": "Unlimited", "pl": "Bez limitu" }
      ],
      [
        { "en": "Storage", "pl": "Pamięć" },
        { "en": "500 MB", "pl": "500 MB" },
        { "en": "50 GB", "pl": "50 GB" }
      ]
    ],
    "header_bg": "#343a40",
    "header_text": "light",
    "striped": true,
    "bordered": false
  }
}
```

## Usage Example

```javascript
function renderTable(widget, language) {
  const { headers, rows, header_bg, header_text, striped, bordered } = widget.widget;

  const headerCells = headers.map(h => {
    const text = h.text?.[language] || h.text?.en || '';
    return `<th>${text}</th>`;
  }).join('');

  const bodyRows = rows.map((row, index) => {
    const cells = row.map(cell => {
      const text = typeof cell === 'object' ? (cell?.[language] || cell?.en || '') : cell;
      return `<td>${text}</td>`;
    }).join('');
    return `<tr>${cells}</tr>`;
  }).join('');

  return `
    <table class="${striped ? 'striped' : ''} ${bordered ? 'bordered' : ''}">
      <thead style="background-color: ${header_bg || '#343a40'}; color: ${header_text === 'dark' ? '#212529' : '#fff'}">
        <tr>${headerCells}</tr>
      </thead>
      <tbody>${bodyRows}</tbody>
    </table>
  `;
}
```
