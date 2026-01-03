# Best Practices

Guidelines for building robust applications with the LessCMS Public API.

## Security

### Store API Keys Securely

❌ **Never** hardcode API keys or commit them to version control:

```javascript
// BAD
const API_KEY = 'workspace_project_abc123...';
```

✅ **Always** use environment variables:

```javascript
// GOOD
const API_KEY = process.env.LESSCMS_API_KEY;
```

### Never Expose Keys Client-Side

Don't use API keys in frontend JavaScript. Create a backend proxy instead:

```javascript
// Backend API route
app.get('/api/pages/:code', async (req, res) => {
  const response = await fetch(
    `${LESSCMS_API_URL}/pages/${req.params.code}`,
    { headers: { 'x-api-key': process.env.LESSCMS_API_KEY } }
  );

  res.json(await response.json());
});
```

## Performance

### Cache Aggressively

Content doesn't change often. Cache responses:

```javascript
const cache = new Map();

async function getPage(code) {
  if (cache.has(code)) {
    return cache.get(code);
  }

  const page = await fetchPage(code);
  cache.set(code, page);

  return page;
}
```

### Use Pagination

Don't fetch all entries at once:

```javascript
// Good
const url = `${baseUrl}/collections/blog-posts?page=1&pageSize=20`;

// Bad
const url = `${baseUrl}/collections/blog-posts`; // Could be thousands of entries
```

### Batch Requests

Minimize API calls by fetching multiple resources:

```javascript
// Fetch all needed data in parallel
const [pages, menus, config] = await Promise.all([
  fetchPages(),
  fetchMenus(),
  fetchConfig()
]);
```

## Error Handling

### Always Check Response Status

```javascript
if (!response.ok) {
  throw new Error(`API Error: ${response.status}`);
}
```

### Implement Retry Logic

For transient errors:

```javascript
async function fetchWithRetry(url, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url);
      if (response.ok) return await response.json();

      if (response.status >= 500 && i < maxRetries - 1) {
        await new Promise(r => setTimeout(r, 1000 * (i + 1)));
        continue;
      }

      throw new Error(`HTTP ${response.status}`);
    } catch (error) {
      if (i === maxRetries - 1) throw error;
    }
  }
}
```

### Provide Fallbacks

Always have fallback content:

```javascript
try {
  return await fetchPage(code);
} catch (error) {
  console.error('Failed to fetch page:', error);
  return defaultPage; // Fallback content
}
```

## Content Handling

### Handle Multilingual Fields

Extract language-specific values:

```javascript
function getLocalized(field, lang = 'en') {
  if (typeof field === 'object' && !Array.isArray(field)) {
    return field[lang] || field.en || Object.values(field)[0];
  }
  return field;
}

const title = getLocalized(page.content.title, 'pl');
```

### Filter Public Content Only

Only show published content:

```javascript
const publicPages = pages.filter(p => p.is_public);
```

### Validate Data

Don't assume data structure:

```javascript
function safeGetTitle(page, lang = 'en') {
  return page?.content?.title?.[lang] || 'Untitled';
}
```

## Development Workflow

### Use Different Keys Per Environment

- Development: `dev_api_key`
- Staging: `staging_api_key`
- Production: `prod_api_key`

### Version Control

Add `.env` to `.gitignore`:

```
# .gitignore
.env
.env.local
```

Provide `.env.example` for teammates:

```
# .env.example
LESSCMS_API_KEY=your_api_key_here
LESSCMS_WORKSPACE=workspace_code
LESSCMS_PROJECT=project_code
```

### Monitor Usage

Track API calls and errors:

```javascript
function trackApiCall(endpoint, status) {
  console.log({
    endpoint,
    status,
    timestamp: new Date().toISOString()
  });

  // Send to analytics
  analytics.track('api_call', { endpoint, status });
}
```

## Architecture Patterns

### Centralized API Client

Create a single API client module:

```javascript
// api/client.js
class LessCMSClient {
  constructor(apiKey, workspace, project) {
    this.baseUrl = `https://api.lesscms.com/v1/${workspace}/${project}`;
    this.headers = { 'x-api-key': apiKey };
  }

  async get(endpoint) {
    const response = await fetch(`${this.baseUrl}/${endpoint}`, {
      headers: this.headers
    });

    if (!response.ok) {
      throw new Error(`API Error: ${response.status}`);
    }

    return await response.json();
  }

  async getPages() {
    const { pages } = await this.get('pages');
    return pages;
  }

  async getPage(code) {
    return await this.get(`pages/${code}`);
  }

  async getCollection(code) {
    const { data } = await this.get(`collections/${code}`);
    return data;
  }
}

export default new LessCMSClient(
  process.env.LESSCMS_API_KEY,
  process.env.LESSCMS_WORKSPACE,
  process.env.LESSCMS_PROJECT
);
```

### Repository Pattern

Encapsulate data access:

```javascript
// repositories/PageRepository.js
class PageRepository {
  constructor(apiClient) {
    this.client = apiClient;
  }

  async findAll() {
    return await this.client.getPages();
  }

  async findByCode(code) {
    return await this.client.getPage(code);
  }

  async findPublic() {
    const pages = await this.findAll();
    return pages.filter(p => p.is_public);
  }
}
```

## Testing

### Mock API Responses

```javascript
// __mocks__/api.js
export const mockPages = [
  {
    code: 'homepage',
    name: 'Homepage',
    is_public: true,
    content: { /* ... */ }
  }
];

export async function fetchPages() {
  return mockPages;
}
```

### Test Error Scenarios

```javascript
test('handles API errors gracefully', async () => {
  fetch.mockRejectedValueOnce(new Error('Network error'));

  const result = await getPageWithFallback('homepage');

  expect(result).toEqual(defaultPage);
});
```

## Related

- [Authentication](authentication.md)
- [Error Handling](errors.md)
- [Caching Strategy](caching.md)
