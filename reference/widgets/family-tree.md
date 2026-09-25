# Family Tree Widget

A MyHeritage-style family chart. People are stored as a flat list with parent / partner
references; the client computes the layout (home person's siblings and descendants
downwards, direct ancestors upwards), draws rounded connectors and provides a pan / zoom
viewport with an optional details panel.

## Widget Type

```
family-tree
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"family-tree"` |
| `uuid` | string | Unique widget identifier |
| `widget.persons` | array | Flat list of people |
| `widget.persons[].id` | string | Person identifier referenced by relations |
| `widget.persons[].first_name` | string | First name |
| `widget.persons[].last_name` | string | Last name |
| `widget.persons[].maiden_name` | string | Maiden name (shown after a comma on the card, e.g. `Antonina Szarlej, Jancia`) |
| `widget.persons[].gender` | string | `"m"`, `"f"` or `""` (unknown) — drives the card border colour |
| `widget.persons[].birth` | string | Birth date as typed (`1942` or `1942-03-01`); the card shows the year |
| `widget.persons[].death` | string | Death date as typed |
| `widget.persons[].deceased` | boolean | Person is deceased (card shows a localized "Deceased" label when there are no dates) |
| `widget.persons[].photo` | string | Photo URL (empty → silhouette) |
| `widget.persons[].note` | string | Free text shown in the details panel |
| `widget.persons[].father_id` | string\|null | `id` of the father |
| `widget.persons[].mother_id` | string\|null | `id` of the mother |
| `widget.persons[].partner_ids` | array | `id`s of partners; couples are also inferred from shared children |
| `widget.card` | object | Card styling |
| `widget.card.background` | string\|null | Card background |
| `widget.card.background:hover` | string\|null | Card background on hover |
| `widget.card.male_color` | string\|null | Border colour for men |
| `widget.card.female_color` | string\|null | Border colour for women |
| `widget.card.neutral_color` | string\|null | Border colour for unknown gender |
| `widget.card.radius` | number | Card border radius in px |
| `widget.name` | object | Name styling |
| `widget.name.color` | string\|null | Name colour |
| `widget.dates` | object | Dates styling |
| `widget.dates.color` | string\|null | Dates colour |
| `widget.line` | object | Connector styling |
| `widget.line.color` | string\|null | Connector colour |
| `widget.config` | object | Behaviour configuration |
| `widget.config.root_id` | string\|null | Home person; `null` → first person in the list |
| `widget.config.height` | number | Viewport height in px |
| `widget.config.initial_view` | string | `"fit"` (whole tree) or `"home"` (centred on the home person at 100%) |
| `widget.config.show_photos` | boolean | Render photos / silhouettes on cards |
| `widget.config.show_dates` | boolean | Render life span on cards |
| `widget.config.show_controls` | boolean | Render zoom / fit / home buttons |
| `widget.config.show_details` | boolean | Clicking a card opens a details panel (photo, dates, note, relatives) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Layout rules

* The chart is built around `config.root_id`. If that person has parents, their family
  (parents, all siblings, everyone's partners and descendants) is laid out top-down.
* Direct ancestors of the home person's parents form a pedigree growing upwards; in-law
  parents are not drawn.
* A person with several partners is rendered as `[person][partner 1][partner 2]`, with each
  couple's children hanging under the gap between the two cards.
* People not connected to the home person are appended to the right of the main tree.
* Pan by dragging, zoom with the buttons, pinch or `Ctrl` + wheel.

## Example Response

```json
{
  "widget_type": "family-tree",
  "uuid": "family-tree-001",
  "widget": {
    "persons": [
      { "id": "p1", "first_name": "Kazimierz", "last_name": "Szarlej", "maiden_name": "", "gender": "m", "birth": "1942", "death": "2003", "deceased": false, "photo": "", "note": "", "father_id": null, "mother_id": null, "partner_ids": ["p2"] },
      { "id": "p2", "first_name": "Irena", "last_name": "Szarlej", "maiden_name": "Wiecheć", "gender": "f", "birth": "1949", "death": "", "deceased": false, "photo": "https://img.lesscms.io/…/irena.jpg", "note": "", "father_id": null, "mother_id": null, "partner_ids": [] },
      { "id": "p3", "first_name": "Przemysław", "last_name": "Szarlej", "maiden_name": "", "gender": "m", "birth": "1987", "death": "", "deceased": false, "photo": "", "note": "", "father_id": "p1", "mother_id": "p2", "partner_ids": [] }
    ],
    "card": {
      "background": "#ffffff",
      "background:hover": null,
      "male_color": "#5bb3d9",
      "female_color": "#ef8a8a",
      "neutral_color": "#b8bec7",
      "radius": 10
    },
    "name": { "color": "var:dark" },
    "dates": { "color": "var:muted" },
    "line": { "color": "var:border" },
    "config": {
      "root_id": "p3",
      "height": 520,
      "initial_view": "fit",
      "show_photos": true,
      "show_dates": true,
      "show_controls": true,
      "show_details": true
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0
  }
}
```
