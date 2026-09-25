# Animated Headline Widget (premium)

A heading with one rotating, typed or highlighted word list:
*"We build websites that are **fast** / **beautiful** / **effective**"*.
Requires the `premium_widgets` plan feature.

## Widget Type

```
animated-headline
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"animated-headline"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.text` | object | Text element group |
| `widget.text.before` | object | Multilingual text before the animated word |
| `widget.text.words` | object | Multilingual word list, one word/phrase per line (`\n`) |
| `widget.text.after` | object | Multilingual text after the animated word |
| `widget.animation` | object | Animation group |
| `widget.animation.type` | string | `"rotate"`, `"typewriter"`, `"slide"`, `"flip"`, `"clip"`, `"none"` |
| `widget.animation.interval` | number | Milliseconds a word stays visible (default `2500`) |
| `widget.animation.speed` | number | Transition duration in ms; for typewriter the time to type one word (default `600`) |
| `widget.animation.loop` | boolean | Restart from the first word after the last (default `true`) |
| `widget.animation.cursor` | boolean | Show a blinking caret (typewriter only) |
| `widget.highlight` | object | Highlight drawn around / under the word |
| `widget.highlight.style` | string | `"none"`, `"underline"`, `"double"`, `"circle"`, `"marker"`, `"strike"`, `"zigzag"` |
| `widget.highlight.color` | string | Highlight color (`var:code` or hex) |
| `widget.highlight.weight` | number | Stroke thickness in px (default `4`) |
| `widget.highlight.animate` | boolean | Draw the highlight in when the word appears |
| `widget.heading` | object | Typography group |
| `widget.heading.tag` | string | HTML tag: `"h1"`, `"h2"`, `"h3"`, `"p"` |
| `widget.heading.size` | number | Font size in px (default `44`) |
| `widget.heading.weight` | string | Font weight `"300"`–`"900"` |
| `widget.heading.align` | string | `"left"`, `"center"`, `"right"` |
| `widget.heading.color` | string | Text color |
| `widget.heading.word_color` | string | Color of the animated word |
| `widget.heading.font_family` | string | Font family name (empty = project heading font) |
| `widget.heading.line_height` | number | Line height (default `1.15`) |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "animated-headline",
  "uuid": "animated-headline-123",
  "widget": {
    "text": {
      "before": { "en": "We build websites that are", "pl": "Tworzymy strony, które są" },
      "words": { "en": "fast\nbeautiful\neffective", "pl": "szybkie\npiękne\nskuteczne" },
      "after": { "en": "", "pl": "" }
    },
    "animation": { "type": "rotate", "interval": 2500, "speed": 600, "loop": true, "cursor": true },
    "highlight": { "style": "marker", "color": "var:primary", "weight": 4, "animate": true },
    "heading": {
      "tag": "h2", "size": 44, "weight": "700", "align": "left",
      "color": "var:dark", "word_color": "var:primary", "font_family": "", "line_height": 1.15
    }
  },
  "settings": {}
}
```

## Rendering Notes

- Server-side render the **first** word; cycle on the client. Respect `prefers-reduced-motion` (show the first word, no cycling).
- Reserve the width of the longest word so the line does not reflow while rotating (`inline-grid` with a hidden ghost word).
- `typewriter`: type the word character by character over `speed` ms, wait `interval`, delete, type the next.
- Highlights are pseudo-elements positioned on the word; `marker` sits behind the text with reduced opacity.
- `settings` follow the common widget style structure (see [Widget Settings](../../guides/ai-frontend-generation.md)).
