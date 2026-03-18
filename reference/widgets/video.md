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
| `widget.source` | string | Video source: `"youtube"`, `"vimeo"`, `"url"` |
| `widget.url` | string | Original video URL |
| `widget.autoplay` | boolean | Auto-start video on load |
| `widget.loop` | boolean | Loop video playback |
| `widget.muted` | boolean | Start video muted |
| `settings` | object | Style settings (optional) |

> **Note**: The API response also includes an `embed_url` field with a pre-computed embed URL for YouTube/Vimeo videos.

## Example Response

```json
{
  "widget_type": "video",
  "uuid": "video-123",
  "widget": {
    "source": "youtube",
    "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
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

## Source Values

| Value | Description |
|-------|-------------|
| `youtube` | YouTube video |
| `vimeo` | Vimeo video |
| `url` | Direct video file URL (MP4, WebM, etc.) |

## Usage Example

```javascript
function renderVideo(widget) {
  const { source, url, autoplay, loop, muted } = widget.widget;
  const embedUrl = widget.widget.embed_url;

  if (source === 'youtube' || source === 'vimeo') {
    const params = new URLSearchParams();
    if (autoplay) params.set('autoplay', '1');
    if (loop) params.set('loop', '1');
    if (muted) params.set(source === 'youtube' ? 'mute' : 'muted', '1');

    const separator = embedUrl.includes('?') ? '&' : '?';

    return `
      <div class="video-wrapper" style="position: relative; padding-bottom: 56.25%;">
        <iframe
          src="${embedUrl}${separator}${params.toString()}"
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
        src="${url}"
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
