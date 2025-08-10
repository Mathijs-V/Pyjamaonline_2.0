# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Shopify Dawn theme - a performance-focused, web-native theme that serves as Shopify's reference implementation for Online Store 2.0. It follows HTML-first, JavaScript-only-as-needed principles with server-side rendering via Liquid.

## Brand & Design Philosophy

When developing for this theme, adhere to the following principles to maintain the brand identity of a modern Dutch fashion brand. This is the creative direction that informs all technical implementation.

* **Aesthetic: Clean & Minimalist.** The brand's identity is built on a sophisticated, minimalist aesthetic inspired by modern Dutch design.
    * **Action:** All new components must align with the existing typography and color schemes defined in `settings_data.json`. Avoid introducing new, inconsistent styles.

* **Content: Visual-First.** As a fashion brand, high-quality product imagery and video are the most important content.
    * **Action:** Always ensure performance is maintained for our large visual assets by strictly following Dawn's native lazy loading and responsive image practices.

* **Approach: Mobile-First & Fluid.** The shopping experience must be seamless on mobile.
    * **Action:** All new features must be designed and built for mobile first, then scaled up to desktop.
    * **Action:** Rely on Dawn's fluid design principles. Let the content dictate layout changes rather than adding arbitrary breakpoints for specific devices.

* **Language: Default is Dutch.** The primary audience is Dutch-speaking.

* **Shopify Admin first approach** If at any point during the development, it's better to change something in the shopify admin instead of in the code, tell me.  

## Key Commands

### Development
- `shopify theme serve` - Launch development server and preview theme locally
- `shopify theme push` - Push theme changes to Shopify store
- `shopify theme pull` - Pull latest theme from Shopify store
- `shopify theme check` - Run Theme Check linter to validate Liquid, JSON, and assets
- `shopify theme list` - List all themes on the connected store
- `shopify theme publish` - Publish theme to make it live

### Testing & Validation
- `shopify theme check` - Validate theme code for errors and best practices
- `shopify theme check --auto-correct` - Auto-fix correctable issues

## Architecture & Structure

### Core Files
- **layout/theme.liquid** - Main layout wrapper for all non-password pages
- **layout/password.liquid** - Layout for password-protected store pages
- **config/settings_schema.json** - Defines theme customization settings schema
- **config/settings_data.json** - Stores theme customization values

### Template System
Templates in `templates/` define page structures using JSON format, referencing sections:
- Each template specifies which sections to include and their order
- Sections are self-contained components with schema definitions
- Dynamic sections can be added/removed/reordered via theme editor

### Section Architecture
Sections in `sections/` are modular components with:
- Liquid markup for rendering
- Schema definition at bottom defining settings, blocks, and presets
- Can include CSS/JS via `{% stylesheet %}` and `{% javascript %}` tags
- Header/footer sections use group files (header-group.json, footer-group.json)

### Asset Management
- CSS files follow component-based naming: `component-[name].css`
- JavaScript modules use vanilla JS with no frameworks
- Icons are SVG files prefixed with `icon-`
- All assets served from `assets/` directory

### Liquid Rendering Flow
1. Request hits template (e.g., product.json)
2. Template specifies sections to render
3. Each section renders with its settings from settings_data.json
4. Snippets provide reusable components
5. Layout wraps final output

## Development Principles

### Web-Native Approach
- No JavaScript frameworks or build tools
- Progressive enhancement over polyfills
- Server-side rendering with Liquid
- Minimal client-side DOM manipulation

### Performance Targets
- Zero Cumulative Layout Shift
- No render-blocking JavaScript
- No long tasks
- JavaScript only when necessary

### Code Style
- Use semantic HTML
- Follow existing component patterns
- Maintain consistency with Dawn's web-native principles
- No external dependencies or libraries

## Localization
- Translations in `locales/` directory
- Buyer-facing: `{{language}}.json`
- Merchant-facing (admin): `{{language}}.schema.json`
- Use `{{ 'key' | t }}` filter for translations

## GitHub Actions
CI pipeline runs on push:
- Lighthouse performance testing
- Theme Check validation
- Both must pass for PR approval

## Working with Sections
When modifying sections:
1. Check schema at bottom for available settings
2. Use `{{ section.settings.setting_name }}` to access values
3. For blocks: `{% for block in section.blocks %}`
4. Update both markup and schema together

## Common Patterns
- Product cards use `snippets/card-product.liquid`
- Cart functionality split between drawer and page implementations
- Media galleries use `snippets/product-media-gallery.liquid`
- Forms use server-side validation with Liquid

## Important Notes
- Always test theme changes with `shopify theme serve` before pushing
- Run `shopify theme check` before committing
- Follow Dawn's web-native principles - no frameworks or build steps
- Server-side logic stays in Liquid, client-side only for progressive enhancement