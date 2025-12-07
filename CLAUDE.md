# Ghost Theme Development Guide

A custom Ghost theme for a personal blog and monthly newsletter, built on the [Ghost Starter](https://github.com/TryGhost/Starter) foundation.

## Project Overview

This theme transforms the minimal Starter theme into a clean, premium-feeling personal blog. The design philosophy prioritizes content over decoration—text is king.

### Design Principles

- **Content-focused and minimal** - No visual clutter, let the writing shine
- **Premium and polished** - Quality feel without corporate stiffness
- **Casual and personal** - Approachable, not stuffy

### Design Reference

The [Attila theme](https://github.com/zutrinken/attila) serves as inspiration for:
- Clean typography with generous whitespace
- Light/dark mode toggle (system preference default)
- Reading progress indicator on posts
- Subtle, elegant animations

## Development Commands

```bash
npm run dev      # Start development with watch mode
npm run build    # Production build
npm run test     # Run gscan validation
npm run zip      # Build and package theme for upload
```

## Architecture

### Build System

- **Rollup** for JavaScript bundling
- **PostCSS** for CSS processing with imports and preset-env
- Output to `assets/built/`

### File Structure

```
├── default.hbs          # Base template (header, footer, dark mode toggle)
├── index.hbs            # Homepage post listing
├── post.hbs             # Individual post template
├── page.hbs             # Static page template
├── tag.hbs              # Tag archive
├── author.hbs           # Author archive
├── error.hbs            # Error pages
├── members/             # Membership pages (signin, signup, account)
├── partials/            # Reusable components
│   ├── card.hbs         # Post card for listings
│   └── icons/           # SVG icons
├── assets/
│   ├── css/
│   │   ├── index.css    # Main CSS entry point
│   │   ├── vars.css     # CSS custom properties
│   │   ├── components/  # Buttons, forms, global styles
│   │   └── ghost/       # Ghost-specific components
│   ├── js/
│   │   └── index.js     # Main JS entry point
│   └── built/           # Compiled assets (git-ignored in production)
└── package.json         # Theme config and dependencies
```

## Implementation Specifications

### Typography

| Element | Specification |
|---------|--------------|
| Body font | System font stack: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, sans-serif` |
| Headings | Inter or system fonts |
| Line height | 1.6-1.8 for body text |
| Content width | 680-720px max-width |
| Code font | `"Fira Code", "JetBrains Mono", "SF Mono", Consolas, monospace` |

### Color Palette

Keep it simple—essentially monochrome with one accent color.

**Light Mode:**
- Background: off-white (`#fafafa` or similar)
- Text: dark gray (`#1a1a1a` or similar)

**Dark Mode:**
- Background: dark gray (`#1a1a1a` or similar)
- Text: light gray (`#e5e5e5` or similar)

**Accent:** Pull from Ghost's accent color setting, or use a muted blue

### Layout: Homepage (`index.hbs`)

- Simple header: site title + minimal nav (Home, About, Subscribe)
- Post list showing: title, date, excerpt (2-3 lines max)
- **No feature images** on homepage listing—just clean text
- Simple pagination at bottom
- Footer with copyright and social links

### Layout: Post Page (`post.hbs`)

- Clean article header: title, date, reading time
- **No feature image banner**—keep it text-focused
- Wide, readable content area (680-720px)
- Reading progress bar at top (thin, subtle)
- Optional minimal author byline at bottom
- Newsletter signup CTA at end of posts
- Previous/Next post navigation

### Layout: Page Template (`page.hbs`)

- Same clean styling as posts
- No date or reading time display

## Features to Implement

### Dark Mode

- Toggle button in header (sun/moon icon)
- Respect `prefers-color-scheme` media query
- Store preference in `localStorage`
- Smooth CSS transitions between modes
- Apply class to `<html>` element (e.g., `data-theme="dark"`)

### Reading Progress Bar

- Thin bar at top of viewport on post pages
- Shows scroll progress through article
- Subtle color (accent or muted)
- Only visible on `post.hbs`

### Code Blocks

- Syntax highlighting (One Dark theme or similar)
- Proper monospace font
- Optional: line numbers
- Copy-to-clipboard button
- Horizontal scroll for long lines

### Newsletter Integration

- Ghost's native membership/subscription support
- Subscribe CTA locations:
  - Footer (all pages)
  - End of posts
  - Dedicated `/subscribe` page
- Prominent but not obnoxious styling

### Mobile Responsiveness

- Mobile-first CSS approach
- Hamburger menu for mobile navigation
- Touch-friendly tap targets (min 44x44px)
- Readable on all screen sizes
- Test at 320px, 768px, 1024px, 1440px breakpoints

## What to Avoid

- Hero images or large image banners
- Complex grid layouts
- Sidebar clutter
- Social share button spam
- Gimmicky animations
- Over-designed elements
- Feature images on listings or post headers

## Implementation Order

1. **Typography & Base Styles**
   - Set up CSS custom properties in `vars.css`
   - Implement font stacks and typographic scale
   - Establish spacing and rhythm

2. **Layout Structure**
   - Update `default.hbs` with clean header/footer
   - Simplify navigation
   - Set content max-width

3. **Homepage** (`index.hbs`)
   - Text-only post listing
   - Remove feature image display
   - Clean pagination

4. **Post Template** (`post.hbs`)
   - Clean article header
   - Newsletter CTA
   - Post navigation

5. **Dark Mode**
   - CSS custom properties for colors
   - Toggle button and icon
   - localStorage persistence
   - System preference detection

6. **Reading Progress**
   - JavaScript scroll handler
   - Progress bar styling
   - Post-page only display

7. **Code Blocks**
   - Syntax highlighting setup
   - Copy button functionality
   - Styling refinements

8. **Polish & Testing**
   - Mobile responsiveness
   - Cross-browser testing
   - Ghost validation (`npm run test`)

## Ghost Theme Requirements

- Valid `package.json` with Ghost engine version
- Required templates: `index.hbs`, `post.hbs`, `default.hbs`
- Use Ghost helpers: `{{#foreach}}`, `{{content}}`, `{{excerpt}}`, etc.
- Support Ghost features: navigation, search, members
- Pass `gscan` validation

## Useful Ghost Helpers

```handlebars
{{@site.title}}              {{!-- Site title --}}
{{@site.description}}        {{!-- Site description --}}
{{@site.logo}}               {{!-- Site logo URL --}}
{{@site.accent_color}}       {{!-- Theme accent color --}}
{{navigation}}               {{!-- Primary navigation --}}
{{navigation type="secondary"}} {{!-- Footer navigation --}}
{{#foreach posts}}           {{!-- Loop through posts --}}
{{excerpt words="30"}}       {{!-- Post excerpt --}}
{{content}}                  {{!-- Full post content --}}
{{date format="MMMM D, YYYY"}} {{!-- Formatted date --}}
{{reading_time}}             {{!-- Estimated reading time --}}
{{#if @member}}              {{!-- Check if user is logged in --}}
{{search}}                   {{!-- Search button --}}
```

## Testing

```bash
# Validate theme structure
npm run test

# Build for production
npm run build

# Create uploadable zip
npm run zip
```

## Resources

- [Ghost Theme Documentation](https://ghost.org/docs/themes/)
- [Ghost Handlebars Helpers](https://ghost.org/docs/themes/helpers/)
- [Ghost Starter Theme](https://github.com/TryGhost/Starter)
- [Attila Theme (reference)](https://github.com/zutrinken/attila)
