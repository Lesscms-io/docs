# Video Widget

A video embed widget supporting YouTube, Vimeo, and direct URLs.

## Widget Type

```
video
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `source` | string | Yes | Video source: `youtube`, `vimeo`, `url` |
| `url` | string | Yes | Video URL or embed URL |
| `autoplay` | boolean | Yes | Auto-start video on load |
| `loop` | boolean | Yes | Loop video playback |
| `muted` | boolean | Yes | Start video muted |

## Example Response (YouTube)

```json
{
  "widget_type": "video",
  "uuid": "video-123",
  "data": {
    "source": "youtube",
    "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
    "autoplay": false,
    "loop": false,
    "muted": false
  },
  "settings": {
    "width": "100%",
    "maxWidth": 800
  }
}
```

## Example Response (Vimeo)

```json
{
  "widget_type": "video",
  "uuid": "video-456",
  "data": {
    "source": "vimeo",
    "url": "https://vimeo.com/123456789",
    "autoplay": false,
    "loop": true,
    "muted": true
  },
  "settings": {}
}
```

## Example Response (Direct URL)

```json
{
  "widget_type": "video",
  "uuid": "video-789",
  "data": {
    "source": "url",
    "url": "https://cdn.example.com/videos/intro.mp4",
    "autoplay": true,
    "loop": true,
    "muted": true
  },
  "settings": {}
}
```

## Source Values

| Value | Description |
|-------|-------------|
| `youtube` | YouTube video (supports watch URL or video ID) |
| `vimeo` | Vimeo video |
| `url` | Direct video file URL (MP4, WebM, etc.) |

## Usage Example

```javascript
// Render video widget
function renderVideo(widget) {
  const { source, url, autoplay, loop, muted } = widget.data;

  if (source === 'youtube') {
    return renderYouTube(url, autoplay, loop, muted);
  } else if (source === 'vimeo') {
    return renderVimeo(url, autoplay, loop, muted);
  } else {
    return renderDirectVideo(url, autoplay, loop, muted);
  }
}

function renderYouTube(url, autoplay, loop, muted) {
  // Extract video ID from URL
  const videoId = extractYouTubeId(url);
  if (!videoId) return '';

  const params = new URLSearchParams();
  if (autoplay) params.set('autoplay', '1');
  if (loop) params.set('loop', '1');
  if (muted) params.set('mute', '1');

  return `
    <div class="video-wrapper" style="position: relative; padding-bottom: 56.25%;">
      <iframe
        src="https://www.youtube.com/embed/${videoId}?${params.toString()}"
        style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
      ></iframe>
    </div>
  `;
}

function renderVimeo(url, autoplay, loop, muted) {
  const videoId = url.match(/vimeo\.com\/(\d+)/)?.[1];
  if (!videoId) return '';

  const params = new URLSearchParams();
  if (autoplay) params.set('autoplay', '1');
  if (loop) params.set('loop', '1');
  if (muted) params.set('muted', '1');

  return `
    <div class="video-wrapper" style="position: relative; padding-bottom: 56.25%;">
      <iframe
        src="https://player.vimeo.com/video/${videoId}?${params.toString()}"
        style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
        frameborder="0"
        allow="autoplay; fullscreen; picture-in-picture"
        allowfullscreen
      ></iframe>
    </div>
  `;
}

function renderDirectVideo(url, autoplay, loop, muted) {
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

function extractYouTubeId(url) {
  const match = url.match(
    /(?:youtube\.com\/(?:watch\?v=|embed\/)|youtu\.be\/)([a-zA-Z0-9_-]{11})/
  );
  return match ? match[1] : null;
}
```
