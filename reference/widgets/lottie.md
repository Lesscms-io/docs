# Lottie Widget (premium)

Plays a [Lottie](https://lottiefiles.com) JSON animation. The player
(`lottie-web`) is loaded on the client only for pages that use the widget.
Requires the `premium_widgets` plan feature.

## Widget Type

```
lottie
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"lottie"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.source` | object | Animation file |
| `widget.source.src` | string | URL of the Lottie `.json` |
| `widget.source.alt` | object | Multilingual accessible label |
| `widget.playback` | object | Playback |
| `widget.playback.trigger` | string | `"load"`, `"viewport"` (play while visible), `"hover"`, `"click"` (toggle), `"scroll"` (scrubbed by scroll position) |
| `widget.playback.loop` | boolean | Loop (ignored for `scroll`) |
| `widget.playback.speed` | number | Playback speed multiplier (default `1`) |
| `widget.playback.direction` | number | `1` forward, `-1` reverse |
| `widget.playback.renderer` | string | `"svg"` or `"canvas"` |
| `widget.playback.hover_pause` | boolean | Pause while hovered (not with `hover` trigger) |
| `widget.layout` | object | Box |
| `widget.layout.max_width` | number | Max width in px (default `400`) |
| `widget.layout.align` | string | `"left"`, `"center"`, `"right"` |
| `widget.layout.background` | string\|null | Box background |
| `widget.layout.radius` | number | Corner radius in px |
| `widget.layout.padding` | number | Inner padding in px |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "lottie",
  "uuid": "lottie-123",
  "widget": {
    "source": { "src": "https://img.lesscms.io/…/rocket.json", "alt": { "en": "Rocket launch", "pl": "Start rakiety" } },
    "playback": { "trigger": "viewport", "loop": true, "speed": 1, "direction": 1, "renderer": "svg", "hover_pause": false },
    "layout": { "max_width": 400, "align": "center", "background": null, "radius": 0, "padding": 0 }
  },
  "settings": {}
}
```

## Rendering Notes

- Load `lottie-web` (e.g. `https://cdnjs.cloudflare.com/ajax/libs/lottie-web/5.12.2/lottie.min.js`) once per page, then `lottie.loadAnimation({ container, renderer, loop, autoplay, path })`.
- `viewport`: play / pause with an `IntersectionObserver`; `scroll`: `goToAndStop(progress * totalFrames, true)` on scroll.
- Respect `prefers-reduced-motion` (do not start the animation).
