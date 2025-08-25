# Tailwind CSS v4 Integration in GitCart

This document describes the Tailwind CSS v4 integration implemented in GitCart for modern, utility-first styling.

## Overview

GitCart now includes built-in support for Tailwind CSS v4 with the following features:

- **Automatic detection** of Tailwind CSS files
- **Intelligent caching** for faster rebuilds
- **Production optimization** with Lightning CSS
- **Watch mode support** for development
- **E-commerce specific utilities** and components
- **Dark mode support** with proper theming
- **RTL language support** for Arabic/Hebrew
- **Plugin system compatibility**

## File Structure

```
project/
├── tailwind.config.js          # Tailwind configuration
├── templates/
│   └── default/
│       └── assets/
│           └── css/
│               └── style.css    # Main Tailwind CSS file
└── dist/
    └── assets/
        └── css/
            └── style.css        # Compiled output
```

## Configuration

### tailwind.config.js

The configuration is optimized for GitCart's e-commerce use case:

```javascript
export default {
  content: [
    './templates/**/*.{liquid,njk,hbs,mustache,html,js}',
    './src/**/*.js',
    './plugins/**/*.js',
    './dist/**/*.html'
  ],
  darkMode: 'class',
  theme: {
    extend: {
      // E-commerce specific extensions
      aspectRatio: {
        'product': '4 / 3',
        'category': '16 / 9'
      },
      colors: {
        primary: { /* ... */ },
        success: { /* ... */ },
        error: { /* ... */ }
      }
    }
  }
}
```

### Dependencies

The following packages are required:

```json
{
  "dependencies": {
    "@tailwindcss/cli": "^4.1.12",
    "lightningcss": "^1.30.1"
  }
}
```

## Template CSS Structure

### Main CSS File (style.css)

```css
/* Import Tailwind CSS v4 */
@import "tailwindcss";

/* Custom utilities for GitCart e-commerce */
@layer utilities {
  .aspect-product {
    aspect-ratio: 4 / 3;
  }
  
  .line-clamp-2 {
    overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
  }
}

/* E-commerce components */
@layer components {
  .products-grid {
    @apply grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6;
  }
  
  .btn-primary {
    @apply bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded-lg transition-colors;
  }
  
  .product-card {
    @apply bg-white dark:bg-gray-800 rounded-lg shadow-md hover:shadow-xl transition-all;
  }
}
```

## Build Process

### Automatic Detection

The AssetProcessor automatically detects Tailwind CSS files by checking for:
- `@import "tailwindcss"` 
- `@tailwind` directives

### Build Pipeline

1. **Template Rendering** - Generate HTML from Liquid templates
2. **Content Scanning** - Scan templates for Tailwind classes
3. **CSS Compilation** - Build CSS with Tailwind CLI
4. **Optimization** - Optimize with Lightning CSS (production only)
5. **Caching** - Cache results for faster rebuilds

### CLI Usage

```bash
# Build with Tailwind CSS
gitcart build

# Development with watch mode
gitcart dev --watch

# Production build with minification
gitcart build --minify
```

## Features

### Dark Mode Support

All templates include dark mode variants:

```html
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  <h1 class="text-blue-600 dark:text-blue-400">Welcome</h1>
</div>
```

### E-commerce Components

Pre-built components for e-commerce:

```html
<!-- Product Grid -->
<div class="products-grid">
  <!-- Product Card -->
  <div class="product-card">
    <div class="aspect-product overflow-hidden">
      <img class="w-full h-full object-cover group-hover:scale-105 transition-transform">
    </div>
    <div class="p-4">
      <h3 class="font-semibold mb-2">Product Name</h3>
      <div class="price-regular">$99.99</div>
      <button class="btn-primary w-full mt-3">Add to Cart</button>
    </div>
  </div>
</div>
```

### Responsive Design

Mobile-first approach with responsive utilities:

```html
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
  <div class="p-4 lg:p-6">
    <h2 class="text-lg lg:text-xl font-semibold">Title</h2>
  </div>
</div>
```

### RTL Language Support

Built-in RTL support for Arabic/Hebrew:

```html
<body class="{% if current_language_data.rtl %}rtl{% endif %}">
  <div class="text-left rtl:text-right">
    <p class="ml-4 rtl:ml-0 rtl:mr-4">Content</p>
  </div>
</body>
```

## Performance

### Caching System

- **File-based caching** prevents unnecessary rebuilds
- **Content-aware hashing** detects changes accurately
- **Production optimization** with Lightning CSS

### Bundle Size Optimization

- **Purging** removes unused CSS classes
- **Minification** reduces file size
- **Modern CSS** targets current browsers only

## Development Workflow

### Watch Mode

Tailwind CSS automatically rebuilds when templates change:

```bash
gitcart dev --watch
# Starts development server with Tailwind watch mode
```

### Hot Reload

Changes to templates trigger:
1. Template re-rendering
2. Tailwind CSS rebuild  
3. Browser reload

### Debugging

Use verbose mode for detailed output:

```bash
gitcart build --verbose
# Shows Tailwind compilation details
```

## Plugin Integration

### CSS Injection

Plugins can add custom CSS:

```javascript
GitCart.plugin('my-plugin', {
  styles: './custom-styles.css',
  tailwindClasses: ['bg-custom-500', 'text-plugin-primary']
});
```

### Dynamic Classes

The safelist protects dynamically generated classes:

```javascript
// In tailwind.config.js
safelist: [
  'badge-sale',
  'order-pending',
  { pattern: /^(bg|text)-primary-(50|100|500|900)$/ }
]
```

## Troubleshooting

### Common Issues

1. **Build fails**: Check if `@tailwindcss/cli` is installed
2. **Classes not appearing**: Verify content paths in config
3. **Slow builds**: Clear cache with `assetProcessor.clearTailwindCache()`

### Testing

Use the built-in test script:

```bash
node test-tailwind-build.js
```

This validates:
- Configuration files
- Dependencies
- Build process
- Output generation
- Cache functionality

## Migration from Custom CSS

### Before (Custom CSS)
```css
.product-card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
```

### After (Tailwind CSS)
```html
<div class="bg-white dark:bg-gray-800 rounded-lg shadow-md hover:shadow-xl hover:-translate-y-1 transition-all">
  <!-- Content -->
</div>
```

## Best Practices

### 1. Use Semantic Components
```css
@layer components {
  .shop-button {
    @apply bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded-lg transition-colors;
  }
}
```

### 2. Maintain Dark Mode Consistency
```html
<!-- Always provide dark mode variants -->
<div class="bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100">
```

### 3. Use Responsive Design
```html
<!-- Mobile-first responsive design -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3">
```

### 4. Leverage E-commerce Utilities
```html
<!-- Use predefined aspect ratios -->
<div class="aspect-product">
  <img class="w-full h-full object-cover">
</div>
```

## Conclusion

The Tailwind CSS v4 integration provides a modern, maintainable, and performant styling solution for GitCart e-commerce sites. The automatic build process, intelligent caching, and e-commerce-specific utilities make it easy to create beautiful, responsive online stores.