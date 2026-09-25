# Video Lightbox Widget (premium)

A poster image with a play button; the video (YouTube, Vimeo or a file)
opens in a fullscreen lightbox or plays in place. Requires the
`premium_widgets` plan feature.

## Widget Type

```
video-lightbox
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"video-lightbox"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.video` | object | Video source |
| `widget.video.source` | string | `"youtube"`, `"vimeo"`, `"file"` |
| `widget.video.url` | string | Video page URL (YouTube / Vimeo) or file URL |
| `widget.video.start` | number | Start offset in seconds (default `0`) |
| `widget.video.embed_url` | string\|null | Ready-to-use player URL (autoplay, start offset applied); `null` when the URL could not be parsed |
| `widget.poster` | object | Poster frame |
| `widget.poster.image` | string | Poster image URL (YouTube thumbnail is filled in when empty) |
| `widget.poster.alt` | object | Multilingual alt text |
| `widget.poster.ratio` | string | `"16:9"`, `"4:3"`, `"1:1"`, `"21:9"`, `"9:16"` |
| `widget.poster.overlay` | number | Dark overlay opacity `0`–`1` (default `0.25`) |
| `widget.poster.radius` | number | Corner radius in px (default `12`) |
| `widget.play` | object | Play button |
| `widget.play.style` | string | `"circle"`, `"pulse"` (animated halo), `"outline"`, `"minimal"` (icon only) |
| `widget.play.size` | number | Button diameter in px (default `80`) |
| `widget.play.color` | string | Icon color |
| `widget.play.background` | string | Button background |
| `widget.play.background:hover` | string\|null | Button background on hover |
| `widget.text` | object | Caption |
| `widget.text.title` | object | Multilingual title |
| `widget.text.subtitle` | object | Multilingual subtitle (e.g. duration) |
| `widget.text.color` | string | Caption color (overlay positions) |
| `widget.text.position` | string | `"overlay-bottom"`, `"overlay-center"` (under the button), `"below"`, `"none"` |
| `widget.lightbox` | object | Lightbox |
| `widget.lightbox.enabled` | boolean | `true` = open in a fullscreen lightbox, `false` = play in place |
| `widget.lightbox.backdrop` | string | Backdrop color |
| `widget.lightbox.width` | number | Max player width in px (default `1100`) |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "video-lightbox",
  "uuid": "video-lightbox-123",
  "widget": {
    "video": { "source": "youtube", "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ", "start": 0, "embed_url": "https://www.youtube-nocookie.com/embed/dQw4w9WgXcQ?autoplay=1&rel=0" },
    "poster": { "image": "https://i.ytimg.com/vi/dQw4w9WgXcQ/maxresdefault.jpg", "alt": { "en": "Product tour", "pl": "Prezentacja produktu" }, "ratio": "16:9", "overlay": 0.25, "radius": 12 },
    "play": { "style": "pulse", "size": 80, "color": "#ffffff", "background": "var:primary", "background:hover": null },
    "text": { "title": { "en": "Watch the product tour", "pl": "Zobacz prezentację" }, "subtitle": { "en": "2 min", "pl": "2 min" }, "color": "#ffffff", "position": "overlay-bottom" },
    "lightbox": { "enabled": true, "backdrop": "var:dark:92", "width": 1100 }
  },
  "settings": {}
}
```

## Rendering Notes

- Load the player only after the click (no iframe on page load) — better LCP and no third-party cookies before consent.
- Lightbox: teleport to `body`, lock scroll, close on ESC / backdrop click, keep the poster's aspect ratio.
- `source: "file"` renders a `<video controls autoplay playsinline>`; the others an `<iframe allow="autoplay; fullscreen">`.
