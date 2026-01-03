# Error Handling

Learn how to handle errors when working with the LessCMS Public API.

## Error Response Format

All errors return JSON with an `error` field:

```json
{
  "error": "Error type",
  "message": "Detailed error description"
}
```

Some errors may include additional fields for context.

## HTTP Status Codes

| Status Code | Meaning | Description |
|-------------|---------|-------------|
| `200` | OK | Request successful |
| `400` | Bad Request | Invalid parameters or malformed request |
| `401` | Unauthorized | Missing or invalid API key |
| `404` | Not Found | Resource doesn't exist |
| `500` | Internal Server Error | Server-side error |

## Common Errors

### 401 - Missing API Key

**Cause:** No `x-api-key` header provided

```json
{
  "error": "Missing API key"
}
```

**Solution:** Add the `x-api-key` header to your request.

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages
```

### 401 - Invalid API Key

**Cause:** API key is incorrect or has been revoked

```json
{
  "error": "Invalid API key"
}
```

**Solution:**
- Check that your API key is correct
- Ensure the key hasn't been revoked
- Verify the key matches the workspace/project in the URL

### 404 - Resource Not Found

**Cause:** The requested resource doesn't exist or isn't public

```json
{
  "error": "Page not found"
}
```

**Solution:**
- Verify the resource code is correct
- Ensure the resource is marked as `is_public: true`
- Check that the resource exists in your CMS

### 404 - Collection/Entry Not Found

```json
{
  "error": "Collection not found",
  "message": "Collection with code 'blog-posts' not found"
}
```

```json
{
  "error": "Entry not found",
  "message": "Entry with ID 'my-post' not found in collection 'blog-posts'"
}
```

### 500 - Internal Server Error

**Cause:** Server-side issue

```json
{
  "error": "Internal server error"
}
```

**Solution:**
- Retry the request
- Check API status at status.lesscms.com
- Contact support if the issue persists

## Error Handling Best Practices

### 1. Always Check Response Status

```javascript
async function fetchContent(url) {
  const response = await fetch(url, {
    headers: { 'x-api-key': API_KEY }
  });

  if (!response.ok) {
    const error = await response.json();
    throw new Error(`API Error ${response.status}: ${error.error}`);
  }

  return await response.json();
}
```

### 2. Handle Specific Error Cases

```javascript
async function getPage(code) {
  try {
    const response = await fetch(`${baseUrl}/pages/${code}`, {
      headers: { 'x-api-key': API_KEY }
    });

    if (response.status === 404) {
      // Show 404 page
      return null;
    }

    if (response.status === 401) {
      // API key issue - log and alert
      console.error('Invalid API key');
      return null;
    }

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    console.error('Failed to fetch page:', error);
    return null;
  }
}
```

### 3. Implement Retry Logic

For transient errors (500, network issues), implement exponential backoff:

```javascript
async function fetchWithRetry(url, options, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, options);

      if (response.ok) {
        return await response.json();
      }

      // Don't retry client errors (4xx)
      if (response.status >= 400 && response.status < 500) {
        throw new Error(`Client error: ${response.status}`);
      }

      // Retry server errors (5xx)
      if (i < maxRetries - 1) {
        const delay = Math.pow(2, i) * 1000; // Exponential backoff
        await new Promise(resolve => setTimeout(resolve, delay));
        continue;
      }

      throw new Error(`Server error after ${maxRetries} retries`);
    } catch (error) {
      if (i === maxRetries - 1) throw error;
    }
  }
}
```

### 4. Provide Fallback Content

```javascript
async function getPageWithFallback(code) {
  try {
    return await fetchPage(code);
  } catch (error) {
    console.error('Failed to fetch page, using fallback:', error);

    // Return cached or default content
    return {
      code,
      name: 'Page Unavailable',
      content: { sections: [] }
    };
  }
}
```

### 5. Log Errors Properly

```javascript
function logApiError(error, context) {
  console.error('API Error:', {
    message: error.message,
    status: error.status,
    url: context.url,
    timestamp: new Date().toISOString()
  });

  // Send to error tracking service
  if (typeof Sentry !== 'undefined') {
    Sentry.captureException(error, { extra: context });
  }
}
```

## Debugging Tips

### 1. Check Request Headers

Ensure headers are set correctly:

```javascript
console.log('Request headers:', {
  'x-api-key': API_KEY.substring(0, 20) + '...' // Don't log full key
});
```

### 2. Inspect Response

Log the full response for debugging:

```javascript
const response = await fetch(url, options);
console.log('Response status:', response.status);
console.log('Response headers:', Object.fromEntries(response.headers));

const data = await response.json();
console.log('Response body:', data);
```

### 3. Validate URL Structure

Ensure URLs match the expected format:

```
https://api.lesscms.com/v1/:workspace_code/:project_code/:resource
```

### 4. Test with cURL

Isolate issues by testing with cURL:

```bash
curl -v -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages
```

The `-v` flag shows detailed request/response information.

## Getting Help

If you're stuck:

1. Check the [API Reference](../reference/) for correct endpoint usage
2. Review [Authentication Guide](authentication.md) for auth issues
3. Search [Community Forums](https://community.lesscms.com)
4. Contact [Support](mailto:support@lesscms.com)

## Related

- [Authentication](authentication.md)
- [Best Practices](best-practices.md)
- [API Reference](../reference/)
