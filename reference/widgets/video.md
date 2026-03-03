# Video Widget

A video embed widget supporting YouTube, Vimeo, and direct URLs.

## Widget Type

```
video
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"video"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.url` | string | Original video URL |
| `widget.embed_url` | string | Generated embed URL (YouTube/Vimeo) |
| `widget.source` | string | Video source: `"youtube"`, `"vimeo"`, `"url"` |
| `widget.autoplay` | boolean | Auto-start video on load |
| `widget.loop` | boolean | Loop video playback |
| `widget.muted` | boolean | Start video muted |
| `settings` | object | Style settings (optional) |

## Example Response (YouTube)

```json
{
  "widget_type": "video",
  "uuid": "video-123",
  "widget": {
    "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
    "embed_url": "https://www.youtube.com/embed/dQw4w9WgXcQ",
    "source": "youtube",
    "autoplay": false,
    "loop": false,
    "muted": false
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Example Response (Vimeo)

```json
{
  "widget_type": "video",
  "uuid": "video-456",
  "widget": {
    "url": "https://vimeo.com/123456789",
    "embed_url": "https://player.vimeo.com/video/123456789",
    "source": "vimeo",
    "autoplay": false,
    "loop": true,
    "muted": true
  },
  "settings": {}
}
```

## Source Values

| Value | Description |
|-------|-------------|
| `youtube` | YouTube video |
| `vimeo` | Vimeo video |
| `url` | Direct video file URL (MP4, WebM, etc.) |

## Usage Example

```javascript
function renderVideo(widget) {
  const { embed_url, source, autoplay, loop, muted } = widget.widget;

  if (source === 'youtube' || source === 'vimeo') {
    const params = new URLSearchParams();
    if (autoplay) params.set('autoplay', '1');
    if (loop) params.set('loop', '1');
    if (muted) params.set(source === 'youtube' ? 'mute' : 'muted', '1');

    const separator = embed_url.includes('?') ? '&' : '?';

    return `
      <div class="video-wrapper" style="position: relative; padding-bottom: 56.25%;">
        <iframe
          src="${embed_url}${separator}${params.toString()}"
          style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></iframe>
      </div>
    `;
  } else {
    return `
      <video
        src="${widget.widget.url}"
        ${autoplay ? 'autoplay' : ''}
        ${loop ? 'loop' : ''}
        ${muted ? 'muted' : ''}
        controls
        style="width: 100%;"
      ></video>
    `;
  }
}
```
