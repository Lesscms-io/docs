# Authentication

All LessCMS Public API requests require authentication using API keys.

## Overview

The API uses **API key authentication** via the `x-api-key` header. Each API key is tied to a specific workspace and project.

## Getting an API Key

### 1. Generate in Dashboard

1. Log in to your LessCMS dashboard
2. Navigate to your project
3. Go to **Settings** → **API Keys**
4. Click **Generate New API Key**
5. Give your key a descriptive name
6. Copy the key immediately - **it won't be shown again**

### 2. API Key Format

API keys are HMAC-SHA256 signed tokens that look like:

```
workspace123_project456_1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
```

Structure: `{workspace}_{project}_{signature}`

## Making Authenticated Requests

### Using the x-api-key Header

All requests must include the `x-api-key` header:

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/pages
```

### Language Examples

#### JavaScript (fetch)

```javascript
const response = await fetch(
  'https://api.lesscms.com/v1/workspace/project/pages',
  {
    headers: {
      'x-api-key': process.env.LESSCMS_API_KEY
    }
  }
);
```

#### JavaScript (axios)

```javascript
const axios = require('axios');

const response = await axios.get(
  'https://api.lesscms.com/v1/workspace/project/pages',
  {
    headers: {
      'x-api-key': process.env.LESSCMS_API_KEY
    }
  }
);
```

#### PHP

```php
<?php
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, 'https://api.lesscms.com/v1/workspace/project/pages');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'x-api-key: ' . $_ENV['LESSCMS_API_KEY']
]);

$response = curl_exec($ch);
curl_close($ch);
?>
```

#### Python

```python
import requests
import os

response = requests.get(
    'https://api.lesscms.com/v1/workspace/project/pages',
    headers={'x-api-key': os.environ['LESSCMS_API_KEY']}
)
```

## Security Best Practices

### 1. Store Keys Securely

❌ **Never hardcode API keys:**

```javascript
// BAD
const API_KEY = 'workspace_project_1234567890...';
```

✅ **Use environment variables:**

```javascript
// GOOD
const API_KEY = process.env.LESSCMS_API_KEY;
```

### 2. Use Different Keys for Different Environments

Create separate API keys for:

- **Development** - local testing
- **Staging** - pre-production testing
- **Production** - live website

This allows you to revoke keys independently if compromised.

### 3. Never Expose Keys Client-Side

❌ **Don't use API keys in frontend code:**

```html
<!-- BAD -->
<script>
const apiKey = 'workspace_project_1234...';
fetch('https://api.lesscms.com/...', {
  headers: { 'x-api-key': apiKey }
});
</script>
```

✅ **Use a backend proxy:**

```javascript
// GOOD - Backend API route
app.get('/api/pages', async (req, res) => {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/pages',
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  res.json(await response.json());
});
```

### 4. Rotate Keys Regularly

Generate new API keys periodically (e.g., every 90 days) and revoke old ones.

### 5. Monitor Usage

Track API key usage in your dashboard to detect unusual patterns.

## Error Responses

### Missing API Key

```bash
curl https://api.lesscms.com/v1/workspace/project/pages
```

Response:

```json
{
  "error": "Missing API key"
}
```

HTTP Status: `401 Unauthorized`

### Invalid API Key

```bash
curl -H "x-api-key: invalid_key" \
  https://api.lesscms.com/v1/workspace/project/pages
```

Response:

```json
{
  "error": "Invalid API key"
}
```

HTTP Status: `401 Unauthorized`

### Wrong Workspace/Project

If the API key doesn't match the workspace/project in the URL:

```json
{
  "error": "API key does not match workspace or project"
}
```

HTTP Status: `401 Unauthorized`

## Managing API Keys

### List All Keys

View all API keys in your dashboard under **Settings** → **API Keys**.

### Revoke a Key

Click **Revoke** next to any key to immediately invalidate it. This action cannot be undone.

### Rename a Key

Edit the key name to help identify its purpose (e.g., "Production Website", "Mobile App").

## Rate Limiting

Currently, there are no strict rate limits on API keys. However, we monitor usage to prevent abuse.

Best practices:
- Cache responses when possible
- Batch requests efficiently
- Use CDN for static content

[Learn more about caching →](caching.md)

## Troubleshooting

### API Key Not Working

1. **Check the header name**: Must be exactly `x-api-key` (lowercase, with hyphens)
2. **Verify the key**: Copy and paste carefully - no extra spaces
3. **Match workspace/project**: Ensure URL matches the key's workspace and project
4. **Check if revoked**: View your API keys in the dashboard

### Key Compromised?

If you suspect your API key has been compromised:

1. **Revoke immediately** in the dashboard
2. **Generate a new key**
3. **Update your applications** with the new key
4. **Monitor for suspicious activity**

## Need Help?

- [Error Handling Guide](errors.md)
- [Getting Started](../getting-started.md)
- [Report a security issue](mailto:security@lesscms.com)
