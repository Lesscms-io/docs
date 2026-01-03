# Running API Documentation

This directory contains the LessCMS Public API documentation built with Docsify.

## Quick Start

### Option 1: Using Python (Simplest)

```bash
cd docs/api
python3 -m http.server 3001
```

Open browser: http://localhost:3001

### Option 2: Using Node.js (docsify-cli)

```bash
# Install docsify-cli globally
npm install -g docsify-cli

# Run the docs
cd docs/api
docsify serve .
```

Open browser: http://localhost:3000

### Option 3: Using PHP

```bash
cd docs/api
php -S localhost:3001
```

Open browser: http://localhost:3001

## Documentation Structure

```
docs/api/
├── index.html           # Docsify configuration + styling
├── README.md            # Home page (Introduction)
├── getting-started.md   # Quick start guide
├── _sidebar.md          # Navigation menu
├── reference/           # API Reference
│   ├── README.md        # Overview
│   ├── collections.md   # Collections API
│   ├── pages.md         # Pages API
│   ├── menus.md         # Menus API
│   ├── blocks.md        # Blocks API
│   ├── elements.md      # Elements API
│   ├── routes.md        # Routes API
│   ├── redirects.md     # Redirects API
│   ├── sitemap.md       # Sitemap API
│   ├── robots-txt.md    # Robots.txt API
│   └── config.md        # Config API
└── guides/              # Guides
    ├── authentication.md
    ├── errors.md
    ├── caching.md
    ├── best-practices.md
    └── versioning.md
```

## Editing Documentation

All documentation is written in **Markdown (.md files)**. To edit:

1. Edit any `.md` file
2. Refresh the browser - changes appear instantly (no build step!)

## Features

- ✨ **Zero build** - Pure static site, runs in browser
- 🔍 **Full-text search** - Search all docs
- 📱 **Mobile responsive** - Works on all devices
- 🎨 **SendGrid-inspired design** - Clean, modern UI
- 📋 **Code copy buttons** - One-click code copying
- 🌐 **Syntax highlighting** - For Bash, JavaScript, JSON, PHP, Python

## Deployment

### Deploy to Netlify/Vercel

Just point to the `docs/api` directory - no build configuration needed!

### Deploy to GitHub Pages

```bash
# Push to gh-pages branch
git subtree push --prefix docs/api origin gh-pages
```

### Deploy to your server

Copy the entire `docs/api/` directory to your web server. That's it!

## Customization

### Change Theme Colors

Edit `index.html` and modify CSS variables:

```css
:root {
  --theme-color: #1A82E2;       /* Primary color */
  --theme-color-dark: #0D6EFD;  /* Primary dark */
}
```

### Add/Remove Pages

1. Create/delete `.md` file
2. Update `_sidebar.md` navigation

### Change Sidebar

Edit `_sidebar.md`:

```markdown
* **Section Name**
  * [Page Name](path/to/page.md)
```

## Tips

- **Images**: Place in `docs/api/images/` and reference: `![Alt](images/example.png)`
- **Internal links**: Use relative paths: `[Link](../guides/authentication.md)`
- **Code blocks**: Use triple backticks with language: ` ```javascript `

## Need Help?

- [Docsify Documentation](https://docsify.js.org)
- [Markdown Guide](https://www.markdownguide.org)
