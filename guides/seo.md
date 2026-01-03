# SEO & Server-Side Rendering

How to implement proper SEO with LessCMS headless architecture.

## The Challenge

When building with a headless CMS and client-side rendering, search engines may struggle to index your content properly. This is because:

- **JavaScript rendering delays** - Crawlers may not wait for content to load
- **No initial HTML** - Client-side apps serve empty HTML shells
- **Poor page load performance** - Impacts Core Web Vitals and rankings

## The Solution: Server-Side Rendering

With LessCMS Public API, you **must use server-side rendering (SSR)** or **static site generation (SSG)** for proper SEO.

### ✅ Recommended Approaches

#### 1. Static Site Generation (SSG)

**Best for:** Marketing sites, blogs, documentation

Generate HTML at build time using frameworks like:
- **Next.js** (React)
- **Nuxt.js** (Vue)
- **SvelteKit** (Svelte)
- **Astro** (framework-agnostic)
- **Gatsby** (React)

**Example with Next.js:**

```javascript
// pages/[slug].js
export async function getStaticProps({ params }) {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/pages/${params.slug}`,
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const page = await response.json();

  return {
    props: { page },
    revalidate: 3600 // Revalidate every hour
  };
}

export async function getStaticPaths() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/pages',
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const { pages } = await response.json();

  return {
    paths: pages
      .filter(p => p.is_public)
      .map(p => ({ params: { slug: p.code } })),
    fallback: 'blocking'
  };
}

export default function Page({ page }) {
  return (
    <>
      <Head>
        <title>{page.seo.title.en}</title>
        <meta name="description" content={page.seo.description.en} />
        <meta property="og:image" content={page.seo.og_image} />
      </Head>

      {/* Render page content */}
      <div>{/* ... */}</div>
    </>
  );
}
```

**Benefits:**
- ✅ Perfect SEO - fully rendered HTML
- ✅ Blazing fast - pre-built pages
- ✅ Great Core Web Vitals
- ✅ Works without JavaScript

#### 2. Server-Side Rendering (SSR)

**Best for:** Dynamic content, personalized experiences, real-time data

Render HTML on each request:

```javascript
// pages/blog/[slug].js
export async function getServerSideProps({ params }) {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/collections/blog-posts/${params.slug}`,
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const post = await response.json();

  return {
    props: { post }
  };
}

export default function BlogPost({ post }) {
  return (
    <>
      <Head>
        <title>{post.seo.title.en}</title>
        <meta name="description" content={post.seo.description.en} />
      </Head>

      <article>
        <h1>{post.content.title.en}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content.body.en }} />
      </article>
    </>
  );
}
```

**Benefits:**
- ✅ Good SEO - HTML rendered on server
- ✅ Always fresh content
- ✅ Works with dynamic data

**Drawbacks:**
- ⚠️ Slower than SSG (server processing on each request)
- ⚠️ Higher server costs

#### 3. Hybrid Approach (Recommended)

Combine SSG and SSR based on content type:

```javascript
// next.config.js
module.exports = {
  // SSG for static pages
  async getStaticProps() { /* ... */ },

  // SSR for dynamic collections
  async getServerSideProps() { /* ... */ }
};
```

**Strategy:**
- **SSG** for: Homepage, About, Contact, static pages
- **SSR** for: Blog posts, products, user-generated content
- **ISR** (Incremental Static Regeneration) for: Frequently updated content

### ❌ What NOT to Do

**Don't use client-side only rendering:**

```javascript
// ❌ BAD - No SEO
function App() {
  const [page, setPage] = useState(null);

  useEffect(() => {
    fetch('https://api.lesscms.com/v1/workspace/project/pages/homepage', {
      headers: { 'x-api-key': API_KEY }
    })
      .then(r => r.json())
      .then(setPage);
  }, []);

  if (!page) return <div>Loading...</div>;

  return <div>{page.content.title.en}</div>;
}
```

**Why this fails SEO:**
- Search engine crawlers see "Loading..." instead of content
- No meta tags in initial HTML
- Slow time-to-first-byte
- Poor Core Web Vitals

## SEO Best Practices

### 1. Use Proper Meta Tags

```javascript
import Head from 'next/head';

function Page({ page }) {
  const { seo } = page;

  return (
    <>
      <Head>
        {/* Title */}
        <title>{seo.title.en}</title>

        {/* Description */}
        <meta name="description" content={seo.description.en} />

        {/* Keywords */}
        <meta name="keywords" content={seo.keywords.join(', ')} />

        {/* Open Graph */}
        <meta property="og:type" content="website" />
        <meta property="og:title" content={seo.title.en} />
        <meta property="og:description" content={seo.description.en} />
        <meta property="og:image" content={seo.og_image} />
        <meta property="og:url" content={`https://example.com${page.custom_route}`} />

        {/* Twitter Card */}
        <meta name="twitter:card" content="summary_large_image" />
        <meta name="twitter:title" content={seo.title.en} />
        <meta name="twitter:description" content={seo.description.en} />
        <meta name="twitter:image" content={seo.og_image} />

        {/* Canonical */}
        <link rel="canonical" href={seo.canonical_url || `https://example.com${page.custom_route}`} />
      </Head>

      {/* Page content */}
    </>
  );
}
```

### 2. Implement Structured Data

```javascript
function BlogPost({ post }) {
  const structuredData = {
    "@context": "https://schema.org",
    "@type": "BlogPosting",
    "headline": post.content.title.en,
    "description": post.content.excerpt.en,
    "image": post.content.featured_image.url,
    "datePublished": post.content.published_date,
    "dateModified": post.metadata.updated_at,
    "author": {
      "@type": "Person",
      "name": post.content.author
    }
  };

  return (
    <>
      <Head>
        <script
          type="application/ld+json"
          dangerouslySetInnerHTML={{ __html: JSON.stringify(structuredData) }}
        />
      </Head>

      {/* Content */}
    </>
  );
}
```

### 3. Generate XML Sitemap

```javascript
// pages/sitemap.xml.js
export async function getServerSideProps({ res }) {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/sitemap.xml',
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const sitemap = await response.text();

  res.setHeader('Content-Type', 'text/xml');
  res.write(sitemap);
  res.end();

  return { props: {} };
}

export default function Sitemap() {
  return null;
}
```

### 4. Handle Multilingual SEO

```javascript
function Page({ page, locale }) {
  return (
    <Head>
      {/* Hreflang tags */}
      <link rel="alternate" hrefLang="en" href={`https://example.com/en${page.custom_route}`} />
      <link rel="alternate" hrefLang="pl" href={`https://example.com/pl${page.custom_route}`} />
      <link rel="alternate" hrefLang="x-default" href={`https://example.com${page.custom_route}`} />

      {/* Language-specific meta */}
      <meta property="og:locale" content={locale} />
    </Head>
  );
}
```

### 5. Optimize Images

```javascript
import Image from 'next/image';

function Hero({ page }) {
  const heroImage = page.content.hero_image;

  return (
    <Image
      src={heroImage.url}
      alt={heroImage.alt}
      width={1200}
      height={630}
      priority // Load above the fold images eagerly
      placeholder="blur"
      blurDataURL={heroImage.thumbnail}
    />
  );
}
```

## Framework-Specific Examples

### Next.js (Recommended)

```bash
npx create-next-app@latest my-website
```

**Features:**
- ✅ SSG + SSR + ISR support
- ✅ Automatic image optimization
- ✅ Built-in SEO components
- ✅ Great performance

### Nuxt.js (Vue)

```bash
npx nuxi init my-website
```

```javascript
// pages/[slug].vue
<script setup>
const route = useRoute();

const { data: page } = await useFetch(
  `https://api.lesscms.com/v1/workspace/project/pages/${route.params.slug}`,
  {
    headers: { 'x-api-key': useRuntimeConfig().public.apiKey }
  }
);

useHead({
  title: page.value.seo.title.en,
  meta: [
    { name: 'description', content: page.value.seo.description.en }
  ]
});
</script>
```

### Astro

```bash
npm create astro@latest
```

```astro
---
// src/pages/[slug].astro
const { slug } = Astro.params;

const response = await fetch(
  `https://api.lesscms.com/v1/workspace/project/pages/${slug}`,
  {
    headers: { 'x-api-key': import.meta.env.LESSCMS_API_KEY }
  }
);

const page = await response.json();
---

<html>
  <head>
    <title>{page.seo.title.en}</title>
    <meta name="description" content={page.seo.description.en} />
  </head>
  <body>
    <!-- Content -->
  </body>
</html>
```

## Testing SEO

### 1. Google Search Console

Submit your sitemap:
```
https://example.com/sitemap.xml
```

### 2. Rich Results Test

Test structured data:
```
https://search.google.com/test/rich-results
```

### 3. PageSpeed Insights

Test Core Web Vitals:
```
https://pagespeed.web.dev/
```

### 4. Mobile-Friendly Test

```
https://search.google.com/test/mobile-friendly
```

## Monitoring

Track these metrics:

- **Largest Contentful Paint (LCP)** - < 2.5s
- **First Input Delay (FID)** - < 100ms
- **Cumulative Layout Shift (CLS)** - < 0.1
- **Time to First Byte (TTFB)** - < 600ms

## Key Takeaways

1. ✅ **Always use SSR or SSG** with LessCMS API
2. ✅ **Never use client-side only rendering** for public content
3. ✅ **Implement proper meta tags** from CMS SEO fields
4. ✅ **Generate sitemap** using API endpoint
5. ✅ **Use structured data** for rich results
6. ✅ **Optimize Core Web Vitals** for better rankings
7. ✅ **Test with real crawlers** before launch

## Related

- [Best Practices](best-practices.md)
- [Caching Strategy](caching.md)
- [Sitemap API](../reference/sitemap.md)
