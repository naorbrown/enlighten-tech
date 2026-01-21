# Enlighten Tech Website

A minimal, single-page static website for a technical consulting practice. Built with pure HTML and CSS—no frameworks, no build tools, no JavaScript dependencies.

**Live site**: [naorbrown.github.io/enlighten-tech](https://naorbrown.github.io/enlighten-tech/)

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        index.html                           │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  <head>                                               │  │
│  │  ├── Meta tags (SEO, Open Graph, Twitter Card)        │  │
│  │  ├── Security headers                                 │  │
│  │  └── <style> (all CSS embedded)                       │  │
│  └───────────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  <body>                                               │  │
│  │  ├── nav (fixed position)                             │  │
│  │  ├── section.hero (with inline SVG graphic)           │  │
│  │  ├── section#about                                    │  │
│  │  ├── section#services                                 │  │
│  │  ├── section#approach                                 │  │
│  │  ├── section#contact (form → Formspree)               │  │
│  │  ├── footer                                           │  │
│  │  └── <script type="application/ld+json"> (SEO)        │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Why a Single File?

For a small marketing site, a single `index.html` with embedded CSS is optimal:

1. **Zero build complexity** — No webpack, no npm, no node_modules
2. **One HTTP request** — The entire site loads in a single round-trip
3. **Easy deployment** — Push to GitHub, done
4. **Easy maintenance** — Everything is in one place

When to split into separate files:
- CSS exceeds ~500 lines
- Multiple pages share styles
- Team collaboration requires separation of concerns

---

## CSS Architecture

### Design Tokens via CSS Custom Properties

All colors, spacing, and theming are defined once in `:root`:

```css
:root {
    --bg-primary: #101214;
    --bg-secondary: #1a1d20;
    --text-primary: #e8eaed;
    --text-secondary: #9aa0a6;
    --accent: #7eb8da;
    --accent-hover: #9ccaeb;
    --border: #2d3134;
}
```

**Why this matters:**
- Change the accent color once, it updates everywhere
- Dark/light theme switching becomes trivial (swap variable values)
- Consistent design language across components

### Layout Strategy

We use **CSS Grid** for 2D layouts and **Flexbox** for 1D alignment:

| Component | Layout | Why |
|-----------|--------|-----|
| Navigation | Flexbox | Single row, space-between alignment |
| Hero | Flexbox | Text + graphic side by side |
| Services | Grid (3 columns) | Equal-width cards |
| Approach | Grid (2 columns) | 2x2 layout |
| Form | Flexbox (column) | Stacked inputs |

```css
/* Grid for multi-column layouts */
.services-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 14px;
}

/* Flexbox for alignment */
.hero-content {
    display: flex;
    align-items: center;
    gap: 8px;
}
```

### Responsive Design

Mobile-first breakpoints using `@media` queries:

```css
/* Base styles: mobile */
.services-grid {
    grid-template-columns: 1fr;  /* Stack on mobile */
}

/* Tablet and up */
@media (min-width: 600px) {
    .services-grid {
        grid-template-columns: repeat(3, 1fr);
    }
}
```

**Key responsive techniques used:**
- `max-width` on container prevents content from stretching too wide
- `padding` in percentages or small fixed values for breathing room
- Grid columns collapse to single column on mobile
- Navigation links hide on mobile, CTA button remains

### Fixed Navigation with Backdrop Blur

```css
nav {
    position: fixed;
    top: 0;
    background-color: rgba(16, 18, 20, 0.95);
    backdrop-filter: blur(10px);
}
```

The `backdrop-filter: blur()` creates a frosted glass effect. Combined with slight transparency, content scrolling behind the nav is visible but blurred.

**Gotcha:** When using fixed nav, content hides behind it. Solution:

```css
section {
    scroll-margin-top: 65px;  /* Height of nav */
}
```

This ensures anchor links (`#about`, `#services`) scroll to the right position.

---

## SVG Graphics

The knowledge graph graphic is **inline SVG**, not an external file:

```html
<svg class="hero-graphic" viewBox="0 0 80 80" fill="none">
    <!-- Circles, lines, etc. -->
</svg>
```

### Why Inline SVG?

| Approach | Pros | Cons |
|----------|------|------|
| Inline SVG | Styleable with CSS, no extra request, crisp at any size | Larger HTML file |
| External `.svg` | Cacheable, cleaner HTML | Extra HTTP request, harder to style |
| PNG/JPG | Universal support | Blurry when scaled, larger file size |

For a single decorative graphic, inline SVG wins.

### SVG Structure Explained

```svg
<!-- Concentric rings (background decoration) -->
<circle cx="40" cy="40" r="38" stroke="#5a9cbf" stroke-opacity="0.2"/>

<!-- Lines connecting nodes (knowledge graph edges) -->
<line x1="40" y1="40" x2="40" y2="8" stroke="#7eb8da"/>

<!-- Nodes (knowledge graph vertices) -->
<circle cx="40" cy="8" r="3" fill="#101214" stroke="#7eb8da"/>
```

The graphic uses:
- **Concentric circles** for a subtle glow effect
- **Lines from center** to outer nodes (hub-and-spoke pattern)
- **Hexagonal node arrangement** for symmetry
- **Layered opacity** for depth (outer elements more transparent)

---

## SEO & Meta Tags

### Essential Meta Tags

```html
<!-- Basic SEO -->
<meta name="description" content="...">
<meta name="keywords" content="...">
<meta name="robots" content="index, follow">
<link rel="canonical" href="https://...">
```

### Open Graph (Facebook, LinkedIn)

```html
<meta property="og:type" content="website">
<meta property="og:title" content="Enlighten Tech">
<meta property="og:description" content="...">
<meta property="og:url" content="https://...">
```

When someone shares your link on social media, these tags control the preview card.

### Twitter Card

```html
<meta name="twitter:card" content="summary">
<meta name="twitter:title" content="...">
```

### Structured Data (JSON-LD)

```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "ProfessionalService",
    "name": "Enlighten Tech",
    "description": "...",
    "serviceType": ["AI Product Development", ...]
}
</script>
```

This helps Google understand your business for rich search results.

---

## Security Headers

```html
<meta http-equiv="X-Content-Type-Options" content="nosniff">
<meta name="referrer" content="strict-origin-when-cross-origin">
```

| Header | Purpose |
|--------|---------|
| `X-Content-Type-Options: nosniff` | Prevents MIME-type sniffing attacks |
| `referrer: strict-origin-when-cross-origin` | Limits referrer info sent to other domains |

**Note:** For production, configure these as HTTP headers on your server/CDN for stronger enforcement. Meta tags are a fallback.

---

## Form Handling with Formspree

The contact form uses [Formspree](https://formspree.io) as a backend:

```html
<form action="https://formspree.io/f/your-form-id" method="POST">
    <input type="email" name="email" required>
    <textarea name="message" required></textarea>
    <button type="submit">Send</button>
</form>
```

### How It Works

1. User submits form
2. Browser POSTs data to Formspree
3. Formspree validates and emails you
4. User sees Formspree's thank-you page (or configure redirect)

### Setup Steps

1. Create account at [formspree.io](https://formspree.io)
2. Create new form, get your form ID
3. Replace `your-form-id` in the action URL
4. Done—submissions go to your email

**Why Formspree over custom backend?**
- No server to maintain
- Built-in spam filtering
- Free tier handles most use cases
- GDPR compliant

---

## Deployment: GitHub Pages

### How It Works

```
Local Machine              GitHub                    Live Site
┌──────────────┐          ┌──────────────┐          ┌──────────────┐
│ index.html   │  push    │ Repository   │  build   │ GitHub Pages │
│ README.md    │ ──────>  │              │ ──────>  │              │
└──────────────┘          └──────────────┘          └──────────────┘
```

GitHub Pages automatically serves static files from your repository. No CI/CD configuration needed for simple HTML sites.

### Commands

```bash
# Check what changed
git status

# Stage changes
git add .

# Commit with message
git commit -m "Update hero text"

# Push to GitHub (triggers deploy)
git push
```

### Enable GitHub Pages

1. Go to repo Settings → Pages
2. Source: Deploy from branch
3. Branch: `main`, folder: `/ (root)`
4. Save

Site will be live at `https://username.github.io/repo-name/`

---

## File Size & Performance

Current `index.html`: ~15KB (uncompressed)

| Metric | Value | Why It's Good |
|--------|-------|---------------|
| HTML size | ~15KB | Single request, instant load |
| External requests | 0 | No fonts, no analytics, no CDN dependencies |
| JavaScript | 0KB | Pure HTML/CSS |
| Time to interactive | <100ms | Nothing to parse/execute |

### Performance Best Practices Used

1. **No external fonts** — System font stack (`-apple-system, BlinkMacSystemFont, ...`)
2. **No JavaScript** — CSS handles all interactivity (hover states, smooth scroll)
3. **Inline critical CSS** — No render-blocking stylesheet requests
4. **SVG over images** — Vector graphics scale perfectly, small file size

---

## Common Tasks

### Change Colors

Edit the `:root` variables at the top of the `<style>` block:

```css
:root {
    --accent: #7eb8da;      /* Change this */
    --accent-hover: #9ccaeb; /* And this */
}
```

### Add a New Section

1. Add HTML structure:
```html
<section id="testimonials">
    <div class="container">
        <p class="section-label">Testimonials</p>
        <!-- Content here -->
    </div>
</section>
```

2. Add nav link:
```html
<a href="#testimonials">Testimonials</a>
```

3. Add CSS as needed for layout.

### Update SEO

1. Change `<title>` tag
2. Update `meta name="description"`
3. Update Open Graph tags (`og:title`, `og:description`)
4. Update JSON-LD structured data

---

## Next Steps

- [ ] **Custom domain** — Purchase domain, configure DNS to point to GitHub Pages
- [ ] **Form setup** — Create Formspree account, replace placeholder form ID
- [ ] **Email forwarding** — Set up `hello@yourdomain.com` → personal email
- [ ] **Analytics** — Add Plausible or Fathom for privacy-friendly tracking

---

## Resources

- [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
- [CSS Grid Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)
- [Flexbox Guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [Formspree](https://formspree.io)
- [Schema.org Structured Data](https://schema.org/ProfessionalService)
- [Open Graph Protocol](https://ogp.me/)

---

*Built with zero dependencies. Ship fast, stay simple.*
