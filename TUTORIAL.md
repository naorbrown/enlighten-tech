# Website Development Tutorial: Architecture & Vocabulary

This tutorial explains what was built for the Enlighten Technologies website and the key concepts involved.

---

## Project Overview

You now have a **static single-page website** hosted for free on **GitHub Pages**. Here's how it all fits together.

---

## Key Vocabulary

### Frontend Basics

| Term | Meaning |
|------|---------|
| **HTML** | HyperText Markup Language - the structure/content of your page (headings, paragraphs, forms) |
| **CSS** | Cascading Style Sheets - the styling (colors, fonts, layout, spacing) |
| **Static Site** | A website made of fixed files (HTML/CSS/JS) with no server-side processing |
| **Single-Page** | All content on one HTML file, sections accessed by scrolling or anchor links |

### CSS Concepts Used

| Concept | What It Does | Where Used |
|---------|--------------|------------|
| **CSS Variables** | Reusable values defined once, used everywhere | `:root { --accent: #e8a84c; }` |
| **Flexbox** | One-dimensional layout (row OR column) | Navigation bar, contact info |
| **CSS Grid** | Two-dimensional layout (rows AND columns) | Companies grid, contact form |
| **Position: Fixed** | Element stays in place when scrolling | Sticky navigation bar |
| **Backdrop Filter** | Applies effects (like blur) behind an element | Nav bar transparency |
| **Media Queries** | Apply different styles at different screen sizes | Mobile responsiveness |

### Git & GitHub

| Term | Meaning |
|------|---------|
| **Git** | Version control system - tracks changes to your code |
| **Repository (Repo)** | A project folder tracked by Git |
| **Commit** | A saved snapshot of your changes |
| **Push** | Upload your local commits to GitHub |
| **Pull** | Download changes from GitHub to your computer |
| **GitHub Pages** | Free hosting service for static websites |

---

## File Structure

```
Naor Personal Website/
└── index.html      # The entire website (HTML + embedded CSS)
```

For a simple site like this, everything is in one file. Larger projects typically separate:
- `index.html` - structure
- `styles.css` - styling
- `script.js` - interactivity

---

## How the Website Works

### 1. Color Scheme (CSS Variables)

```css
:root {
    --bg-primary: #0f0f0e;      /* Dark background */
    --text-primary: #f5f0e8;    /* Warm white text */
    --accent: #e8a84c;          /* Amber/gold accent */
}
```

Change these values once, and every element using them updates automatically.

### 2. Sticky Navigation

```css
nav {
    position: fixed;    /* Stays in place */
    top: 0;            /* At the very top */
    backdrop-filter: blur(10px);  /* Blurred background */
}
```

The `scroll-margin-top: 80px` on sections prevents content from hiding behind the nav when you click anchor links.

### 3. Responsive Design

```css
@media (max-width: 768px) {
    /* Styles that only apply on screens narrower than 768px */
    .companies-grid {
        grid-template-columns: 1fr 1fr;  /* 2 columns instead of 3 */
    }
}
```

### 4. Contact Form

The form uses `action="https://formspree.io/f/your-form-id"` as a placeholder. To make it functional:

1. Go to [formspree.io](https://formspree.io)
2. Create a free account
3. Create a new form
4. Replace `your-form-id` with your actual form ID
5. Form submissions will be emailed to you

---

## Deployment Pipeline

```
Your Computer                    GitHub                      Live Website
┌─────────────┐                ┌─────────────┐              ┌─────────────┐
│ index.html  │  ──git push──> │  Repository │  ──builds──> │ GitHub Pages│
│ (edit here) │                │             │              │  (public)   │
└─────────────┘                └─────────────┘              └─────────────┘
```

### Commands You'll Use

```bash
# Check what's changed
git status

# Stage all changes
git add .

# Save changes with a message
git commit -m "Updated hero text"

# Upload to GitHub (triggers rebuild)
git push

# Download any remote changes first
git pull
```

---

## Common Tasks

### Change Text/Copy
1. Open `index.html` in any text editor
2. Find the text you want to change
3. Edit it
4. Save the file
5. Run: `git add . && git commit -m "Updated text" && git push`

### Change Colors
1. Find the `:root` section at the top of the `<style>` block
2. Modify the hex color codes
3. Save and push

### Add a New Section
1. Copy an existing section's HTML structure
2. Give it a unique `id` (e.g., `id="testimonials"`)
3. Add a link in the nav: `<a href="#testimonials">Testimonials</a>`
4. Add matching CSS styles

---

## Next Steps

1. **Domain**: Purchase `enlightentech.io` (~$35/year) and point it to GitHub Pages
2. **Form**: Set up Formspree to receive contact submissions
3. **Email**: Set up professional email forwarding (e.g., hello@enlightentech.io → naorbrown@gmail.com)
4. **Analytics**: Add Google Analytics or Plausible to track visitors

---

## Useful Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [CSS Tricks - Flexbox Guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [CSS Tricks - Grid Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)
- [Formspree](https://formspree.io) - Simple form backend
- [Cloudflare Email Routing](https://developers.cloudflare.com/email-routing/) - Free email forwarding

---

*Generated for Enlighten Technologies website project*
