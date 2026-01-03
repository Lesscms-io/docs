# API Versioning

The LessCMS Public API uses URL-based versioning to ensure backward compatibility.

## Current Version

The current API version is **v1**.

```
https://api.lesscms.com/v1/:workspace_code/:project_code/:resource
```

## Version Format

All API endpoints include the version number in the URL:

```
/v1/workspace123/project456/pages
 ^^
 version
```

## Support Policy

When a new major version is released:

- **Previous version supported for 12 months** after the new version release
- **Deprecation warnings** added to response headers 3 months before sunset
- **Migration guide** provided for upgrading

### Example Timeline

```
Jan 2025: v1 released
Jun 2026: v2 released → v1 supported until Jun 2027
Mar 2027: v1 deprecation warnings added
Jun 2027: v1 sunset (no longer supported)
```

## Breaking vs. Non-Breaking Changes

### Breaking Changes (require new version)

- Removing or renaming fields
- Changing field data types
- Changing URL structures
- Removing endpoints
- Changing authentication methods

These require a new version (e.g., v1 → v2).

### Non-Breaking Changes (no new version needed)

- Adding new fields (optional)
- Adding new endpoints
- Adding new optional parameters
- Performance improvements
- Bug fixes

These are added to the current version.

## Deprecation Headers

When an API version is deprecated, responses include:

```
Deprecation: true
Sunset: 2027-06-30
Link: </v2>; rel="successor-version"
```

### Checking for Deprecation

```javascript
const response = await fetch(url);

if (response.headers.get('Deprecation')) {
  const sunset = response.headers.get('Sunset');
  console.warn(`API version deprecated. Sunset date: ${sunset}`);
}
```

## Migration Guide

When migrating to a new version:

1. **Read the changelog** - Understand what changed
2. **Test in development** - Update your code and test thoroughly
3. **Update API URLs** - Change `/v1/` to `/v2/`
4. **Deploy to staging** - Test in staging environment
5. **Monitor errors** - Watch for issues
6. **Deploy to production** - Roll out gradually
7. **Remove old version references** - Clean up

## Staying Up to Date

### Subscribe to Updates

- [API Changelog](https://lesscms.com/api/changelog)
- [Developer Newsletter](https://lesscms.com/newsletter)
- [Status Page](https://status.lesscms.com)

### Monitor Headers

Check `Deprecation` headers regularly:

```javascript
function checkDeprecation(response) {
  if (response.headers.get('Deprecation')) {
    // Alert your team
    notifyTeam('API version deprecated');
  }
}
```

## Version History

### v1 (Current)

**Released:** January 2025

Initial public release featuring:
- Pages API
- Collections API
- Menus API
- Blocks API
- Elements API
- SEO endpoints

## Future Versions

When v2 is released, both versions will be available:

```
/v1/workspace/project/pages  (legacy, supported for 12 months)
/v2/workspace/project/pages  (new, recommended)
```

Your application can use different versions for different resources if needed.

## Best Practices

### 1. Pin to Specific Version

Always include version in URLs:

```javascript
// Good
const baseUrl = 'https://api.lesscms.com/v1/workspace/project';

// Bad
const baseUrl = 'https://api.lesscms.com/workspace/project';
```

### 2. Use Environment Variables

Make version configurable:

```javascript
const API_VERSION = process.env.LESSCMS_API_VERSION || 'v1';
const baseUrl = `https://api.lesscms.com/${API_VERSION}/workspace/project`;
```

### 3. Test New Versions Early

When a new version is announced, test it in development immediately.

### 4. Don't Wait for Sunset

Migrate before the sunset date to avoid last-minute issues.

## Related

- [Getting Started](../getting-started.md)
- [Best Practices](best-practices.md)
- [API Reference](../reference/)
