# Collection Grid Widget

A grid display of collection entries with customizable layout and fields.

## Widget Type

```
collection-grid
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"collection-grid"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.collection_code` | string\|null | Collection code to display |
| `widget.layout` | string | Grid layout mode: `"grid"`, `"list"`, `"cards"` (default: `"grid"`) |
| `widget.columns` | number | Number of columns (default: 3) |
| `widget.gap` | number | Grid gap in pixels (default: 16) |
| `widget.posts_count` | number | Maximum entries to display (default: 6) |
| `widget.order_by` | string | Order field: `"created_at"`, `"title"`, `"random"` (default: `"created_at"`) |
| `widget.order_dir` | string | Order direction: `"asc"`, `"desc"` (default: `"desc"`) |
| `widget.exclude_current_entry` | boolean | Exclude current entry from results (useful on detail pages) |
| `widget.title_field` | string\|null | Field code for title |
| `widget.title_limit` | number | Character limit for title (0 = no limit) |
| `widget.excerpt_field` | string\|null | Field code for excerpt |
| `widget.excerpt_limit` | number | Character limit for excerpt (0 = no limit) |
| `widget.image_field` | string\|null | Field code for image |
| `widget.date_field` | string\|null | Field code for date |
| `widget.show_title` | boolean | Display title (default: true) |
| `widget.show_excerpt` | boolean | Display excerpt (default: true) |
| `widget.show_image` | boolean | Display image (default: true) |
| `widget.show_date` | boolean | Display date (default: true) |
| `widget.show_read_more` | boolean | Display read more link (default: true) |
| `widget.read_more` | object | Read more element group |
| `widget.read_more.html` | object | Multilingual read more text `{ "en": "...", "pl": "..." }` |
| `widget.tags_field` | string\|null | Field code for tags |
| `widget.show_tags` | boolean | Display tags (default: false) |
| `widget.card_background_color` | string\|null | Card background color (hex or `var:*`) |
| `widget.card_text_color` | string\|null | Card text color (hex or `var:*`) |
| `widget.card_border_radius` | number\|null | Card border radius in pixels |
| `widget.card_padding` | number\|null | Card inner padding in pixels |
| `widget.content_gap` | number | Gap between content elements in pixels (default: 8) |
| `widget.route_uuid` | string\|null | Route UUID for entry detail pages |
| `widget.columns_tablet` | number\|null | Number of columns on tablet breakpoint |
| `widget.columns_mobile` | number\|null | Number of columns on mobile breakpoint |
| `widget.card_style` | string | Card visual style (e.g. `"default"`, `"bordered"`) |
| `widget.entry_template` | string\|null | Entry template code for custom layouts |
| `widget.extra_field` | string\|null | Extra field code to display |
| `widget.extra_field_2` | string\|null | Second extra field; rendered after the first one joined with ` • ` (e.g. `2026 • Redesign`) |
| `widget.link_field` | string\|null | Field code holding a URL — cards link there instead of the entry page (portfolio, partners); values without a scheme get `https://` |
| `widget.link_target` | string | `_self` (default) or `_blank` (adds `rel="noopener"`) |
| `widget.show_pagination` | boolean | Show pagination controls |
| `widget.show_extra` | boolean | Show extra field |
| `widget.exclude_url_segment` | string\|null | URL segment to exclude from results |
| `widget.filter_field` | string\|null | Field code to filter by |
| `widget.filter_source` | string | Filter value source: `"static"`, `"url"` or `"current_entry"` (default: `"static"`). With `current_entry`, the renderer uses the `entry_id` of the collection entry currently displayed on the page (typical use: a relation field pointing to that page's entry). |
| `widget.filter_value` | string\|null | Static filter value |
| `widget.filter_url_segment` | string\|null | URL segment for dynamic filter value |
| `widget.filters_enabled` | boolean | **Premium.** Visitor-facing AJAX filters (no reload) |
| `widget.filter_fields` | array | Field codes exposed as filters (select-like and relation fields) |
| `widget.filters_style` | string | `"chips"`, `"select"`, `"sidebar"` (chips stacked vertically) |
| `widget.filters_all_text` | object | Multilingual label of the "all" chip / option |
| `widget.search_enabled` | boolean | Free-text search box (`?q=` across the collection's text fields) |
| `widget.search_placeholder` | object | Multilingual search placeholder |
| `widget.show_results_count` | boolean | Show the number of matching entries |
| `widget.filters` | array | Resolved filter definitions (only when `filters_enabled`): `{ code, name, type, options: [{ value, label }] }` — options come from the field's select options or, for relations, the target collection's entries (`value` = entry id) |
| `widget.pagination_mode` | string | `"none"`, `"load_more"` (button appends the next page), `"infinite"` (auto-load on scroll) |
| `widget.load_more_text` | object | Multilingual "Load more" label |
| `widget.load_more_style` | string | Button style for "Load more" (`primary`, `outline-primary`, …) |
| `widget.entries_meta` | object | Pagination of the enriched first page: `{ total, page, pageSize, totalPages }` |
| `widget.use_custom_layout` | boolean | Use custom entry layout template |
| `widget.layout_config` | object\|null | Custom layout configuration |
| `widget.field_order` | array\|null | Order of displayed fields (managed via drag & drop in CMS) |
| `widget.entries` | array\|undefined | Pre-fetched collection entries (server-side enrichment) |
| `widget.entries_meta` | object\|undefined | Pagination metadata for enriched entries (total, page, etc.) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-grid",
  "uuid": "grid-123",
  "widget": {
    "collection_code": "blog",
    "layout": "grid",
    "columns": 3,
    "gap": 24,
    "posts_count": 6,
    "order_by": "created_at",
    "order_dir": "desc",
    "exclude_current_entry": false,
    "title_field": "title",
    "title_limit": 100,
    "excerpt_field": "excerpt",
    "excerpt_limit": 200,
    "image_field": "featured_image",
    "date_field": "published_at",
    "show_title": true,
    "show_excerpt": true,
    "show_image": true,
    "show_date": true,
    "show_read_more": true,
    "read_more": {
      "html": {
        "en": "Read More",
        "pl": "Czytaj wiecej"
      }
    },
    "tags_field": "tags",
    "show_tags": true,
    "card_background_color": "#ffffff",
    "card_text_color": null,
    "card_border_radius": 8,
    "card_padding": 16,
    "content_gap": 8,
    "route_uuid": "route-uuid-789",
    "columns_tablet": 2,
    "columns_mobile": 1,
    "card_style": "default",
    "entry_template": null,
    "extra_field": null,
    "extra_field_2": null,
    "link_field": null,
    "link_target": "_self",
    "show_pagination": false,
    "show_extra": false,
    "exclude_url_segment": null,
    "filter_field": null,
    "filter_source": null,
    "filter_value": null,
    "filter_url_segment": null,
    "filters_enabled": true,
    "filter_fields": ["category"],
    "filters_style": "chips",
    "filters_all_text": { "en": "All", "pl": "Wszystkie" },
    "search_enabled": true,
    "search_placeholder": { "en": "Search…", "pl": "Szukaj…" },
    "show_results_count": true,
    "filters": [
      { "code": "category", "name": { "en": "Category", "pl": "Kategoria" }, "type": "select", "options": [ { "value": "news", "label": { "en": "News", "pl": "Aktualności" } } ] }
    ],
    "pagination_mode": "load_more",
    "load_more_text": { "en": "Load more", "pl": "Pokaż więcej" },
    "load_more_style": "outline-primary",
    "entries_meta": { "total": 48, "page": 1, "pageSize": 6, "totalPages": 8 },
    "use_custom_layout": false,
    "layout_config": null,
    "field_order": ["image", "date", "title", "excerpt", "tags", "read_more"],
    "entries": [],
    "entries_meta": {}
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Order Options

| order_by | Description |
|----------|-------------|
| `created_at` | Order by creation date |
| `title` | Order alphabetically by title |
| `random` | Random order |

## Usage Example

```javascript
function renderCollectionGrid(widget, language) {
  const {
    columns,
    show_title, show_excerpt, show_image, show_date, show_read_more,
    title_field, excerpt_field, image_field, date_field,
    title_limit, excerpt_limit,
    read_more, gap
  } = widget.widget;

  // Entries may be pre-fetched by the API (server-side enrichment).
  // If widget.entries exists, use it directly. Otherwise, fetch client-side.
  const entries = widget.widget.entries || []; // Fallback: fetch from /api/:project_code/collections/:collection_code
  const readMoreLabel = read_more?.html?.[language] || 'Read More';

  if (entries.length === 0) return '<p>No entries found.</p>';

  const items = entries.map(entry => {
    let title = title_field ? (entry.data[title_field]?.[language] || '') : '';
    let excerpt = excerpt_field ? (entry.data[excerpt_field]?.[language] || '') : '';
    const image = image_field ? entry.data[image_field] : '';
    const date = date_field ? entry.data[date_field] : '';
    const url = entry.url || '#';

    // Apply character limits
    if (title_limit > 0 && title.length > title_limit) {
      title = title.substring(0, title_limit) + '...';
    }
    if (excerpt_limit > 0 && excerpt.length > excerpt_limit) {
      excerpt = excerpt.substring(0, excerpt_limit) + '...';
    }

    return `
      <article class="collection-item">
        ${show_image && image ? `<div class="item-image"><img src="${image}" alt="${title}" loading="lazy"></div>` : ''}
        <div class="item-content">
          ${show_date && date ? `<time class="item-date">${date}</time>` : ''}
          ${show_title && title ? `<h3 class="item-title">${title}</h3>` : ''}
          ${show_excerpt && excerpt ? `<p class="item-excerpt">${excerpt}</p>` : ''}
          ${show_read_more ? `<a href="${url}" class="item-read-more">${readMoreLabel}</a>` : ''}
        </div>
      </article>
    `;
  }).join('');

  return `
    <div class="collection-grid" style="display: grid; grid-template-columns: repeat(${columns}, 1fr); gap: ${gap}px;">
      ${items}
    </div>
  `;
}
```

## AJAX filters, search and load more (premium)

When `filters_enabled` is on, render the `filters` definitions as chips or
selects and the search box; on change call the collection endpoint with the
same parameters the page API used:

```
GET /collections/{code}?page=1&pageSize={posts_count}&lang={lang}&{field}={value}&q={query}&orderBy={order_by}&orderDir={order_dir}
```

- `{field}={value}` — one param per active filter (select code or related entry id); the API resolves the field type from the schema.
- `q` — case-insensitive search across the collection's text fields (plain and multilingual values).
- `orderBy` / `orderDir` — sorting (`created_at`, `updated_at` or a field code). Without `orderBy` the natural order is kept.
- Use `meta.total` / `meta.totalPages` for the results count and to decide whether to show "Load more"; `pagination_mode: "infinite"` appends the next page when a sentinel below the grid enters the viewport.
