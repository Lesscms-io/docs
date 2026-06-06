# Audio Widget

An audio player widget with optional playlist. Tracks reference files uploaded to
the project file manager and served through `img.lesscms.io`, which supports HTTP
Range requests so playback and seeking work on mobile (iOS Safari) and large files
stream progressively instead of downloading in full.

## Widget Type

```
audio
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"audio"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.tracks` | array | Playlist of tracks (see below) |
| `widget.tracks[].url` | string | Public URL of the audio file |
| `widget.tracks[].title` | string | Display title (may be empty — fall back to file name) |
| `widget.show_playlist` | boolean | Show the track list / next-prev controls (default `true`) |
| `widget.autoplay` | boolean | Auto-start the first track on load |
| `widget.loop` | boolean | Loop the whole playlist |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "audio",
  "uuid": "audio-123",
  "widget": {
    "tracks": [
      { "url": "https://img.lesscms.io/projects/abc/audio/intro.m4a", "title": "Intro" },
      { "url": "https://img.lesscms.io/projects/abc/audio/episode-1.m4a", "title": "Odcinek 1" }
    ],
    "show_playlist": true,
    "autoplay": false,
    "loop": false
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Supported Formats

Any audio format the browser can play and that the file manager accepts:
`m4a` (audio/mp4), `mp3`, `aac`, `ogg`, `wav`, `flac`.

> **Streaming note**: Audio files are served with `Accept-Ranges: bytes` and answer
> `206 Partial Content` for ranged requests. This is required for iOS Safari to start
> playback and to support seeking. Render tracks with `preload="metadata"` so the
> player fetches duration without downloading the whole file up front.

## Usage Example

```javascript
function renderAudio(widget) {
  const { tracks, show_playlist } = widget.widget;
  if (!tracks?.length) return '';

  const playlist = (show_playlist !== false && tracks.length > 1)
    ? `<ul class="audio-playlist">${tracks.map((t, i) =>
        `<li data-index="${i}">${t.title || t.url.split('/').pop()}</li>`).join('')}</ul>`
    : '';

  return `
    <div class="audio-widget">
      <audio src="${tracks[0].url}" controls preload="metadata" style="width:100%"></audio>
      ${playlist}
    </div>
  `;
}
```
