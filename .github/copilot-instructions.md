---
applyTo: "**/*.{js,css,liquid,json,md}**"
---

# Shopify Dawn Theme Development Guide

## Architecture Overview

This is a Shopify Dawn theme (v15.4.0) with a component-based architecture using Liquid templating, custom web components, and CSS custom properties.

## Core Patterns

### Liquid Templating

- **Sections**: Page components in `/sections/` with JSON schema for settings
- **Snippets**: Reusable components in `/snippets/` with documented parameters
- **Templates**: Page layouts in `/templates/` defining section composition

### Component Communication

```javascript
// Pub/Sub pattern for cross-component events
subscribe(PUB_SUB_EVENTS.optionValueSelectionChange, handler);

// Custom events for component lifecycle
this.dispatchEvent(new CustomEvent("product-info:loaded", { bubbles: true }));
```

### Asset Management

```liquid
<!-- Always use asset_url filter -->
{{ 'component-price.css' | asset_url | stylesheet_tag }}

<!-- Conditional loading -->
{% if section.settings.image_zoom == 'hover' %}
  <script src="{{ 'magnify.js' | asset_url }}" defer="defer"></script>
{% endif %}
```

### CSS Architecture

```css
/* CSS custom properties for theming */
:root {
  --color-background: #ffffff;
  --product-card-border-radius: 12px;
}

/* Component-specific variables */
.product-card-wrapper {
  --border-radius: var(--product-card-corner-radius);
}
```

## Key Conventions

### Section IDs & Data Attributes

```liquid
<!-- Qualified section IDs -->
id="ProductInfo-{{ section.id }}"

<!-- Data attributes for JS hooks -->
data-section="{{ section.id }}"
data-product-id="{{ product.id }}"
```

### Variant Handling

```liquid
<!-- Use selected_or_first_available_variant -->
{%- assign variant = product.selected_or_first_available_variant -%}
{{ variant.price | money }}
```

### Settings Access

```liquid
<!-- Section settings -->
{% if section.settings.image_zoom == 'hover' %}

<!-- Global settings -->
{% if settings.animations_reveal_on_scroll %}
```

## Development Workflow

### File Organization

- `/assets/` - CSS, JS, images (processed by Shopify)
- `/sections/` - Page sections with schema
- `/snippets/` - Reusable components
- `/templates/` - Page templates (auto-generated JSON)
- `/config/` - Theme settings schema
- `/locales/` - Translation files

### Common Patterns

- Use `{% render %}` for snippets with parameters
- Include CSS conditionally based on section settings
- Load JS with `defer="defer"` for performance
- Use `{{ block.shopify_attributes }}` for dynamic attributes

### Metafields

```json
// Defined in .shopify/metafields.json
{
  "key": "custom_field",
  "namespace": "my_app",
  "type": { "name": "single_line_text_field" }
}
```

Access in Liquid: `{{ product.metafields.my_app.custom_field }}`

## Performance Considerations

- Use `media="print" onload="this.media='all'"` for CSS
- Defer non-critical JavaScript
- Lazy load images with `loading="lazy"`
- Minimize DOM manipulation in JavaScript

## Debugging

- Use `{% if request.design_mode %}` for theme editor specific code
- Check browser console for JavaScript errors
- Use `{{ product | json }}` for debugging Liquid objects
- Metafields debug: `{% if request.design_mode or request.params.debug_meta == '1' %}`
