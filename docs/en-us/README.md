# GitCart - Documentation (English)

<div align="center">

**Universal static e-shop generator from markdown files**

</div>

---

## 📚 Complete Documentation

### User Guides
- [📖 User Guide](user-guide.md) - Complete guide to using GitCart
- [⚙️ Configuration](configuration.md) - Detailed configuration reference guide
- [🌍 Multilingual Support](multilingual.md) - Creating multilingual e-shops
- [📊 SEO Optimization](seo.md) - SEO optimization and performance

### For Developers
- [🎨 Template System](templates.md) - Creating custom templates and themes
- [📚 Taiwind CSS integration](tailwind-integration.md) - Tailwind CSS integration and customization
- [🔌 Plugin System](plugins.md) - Extension and plugin development
- [💻 Frontend API](frontend-api.md) - JavaScript API for frontend functionality
- [🔧 Plugin API](plugin-api.md) - API reference for plugin development

### Reference
- [🛠️ CLI Commands](cli-reference.md) - Complete overview of CLI commands and options

---

## 🎯 What is GitCart?

GitCart is a modern, lightweight static e-commerce website generator that creates fast and SEO-optimized e-shops from simple markdown files and JSON configuration. Perfect for small to medium e-shops that don't need complex CMS but want professional and fast solutions.

### ✨ Key Features

- 📁 **Simple structure** - Products and categories in markdown files
- 🌍 **Multilingual support** - Full support for multiple languages and locales
- 🎨 **Flexible templating** - Liquid templates with customization options
- 🔍 **SEO optimized** - Automatic sitemaps, meta tags, structured data
- 🛒 **E-commerce features** - Cart, checkout, product filters
- 🔌 **Plugin system** - Extensibility through JavaScript plugins
- 📱 **Responsive design** - Mobile-first approach with Tailwind CSS
- ⚡ **Fast results** - Static files for maximum speed
- 🚀 **Easy deployment** - GitHub/GitLab Pages, Netlify, Vercel ready

## 🚀 Quick Start

### Installation

#### Recommended: Global Installation
```bash
npm install -g gitcart-cli
```

#### Alternative: Local Installation with npx
```bash
npm install gitcart-cli
# Then use with npx:
npx gitcart init my-eshop
```

### Creating a New Project

```bash
# With global installation
gitcart init my-eshop

# With local installation
npx gitcart init my-eshop

# With default template
gitcart init my-eshop --template default

# With minimal template
gitcart init my-eshop --template minimal

# Navigate to project folder
cd my-eshop
```

### Basic Configuration

Edit `site.json`:

```json
{
  "name": "My E-shop",
  "url": "https://myshop.com",
  "languages": {
    "en-us": {
      "name": "English", 
      "default": true, 
      "short": "en", 
      "currency": "USD"
    }
  },
  "variables": {
    "contact": {
      "email": "info@myshop.com",
      "phone": "+1 555 123 4567"
    }
  }
}
```

### Adding Your First Product

```bash
# Interactive product creation
gitcart product add

# Or manually create file en-us/categories/electronics/.products/iphone-15.md
```

```markdown
---
title: iPhone 15
price: 999
sku: IPHONE15-128
images: 
  - iphone-front.jpg
  - iphone-back.jpg
tags: [smartphone, apple]
featured: true
---

# iPhone 15

Latest iPhone with USB-C connector and advanced features.

## Specifications
- 128GB storage
- A17 Bionic chip
- 48MP camera
```

### Build and Preview

```bash
# Build pages
gitcart build

# Start dev server with live reload
gitcart dev --watch
```

Your e-shop is ready at `http://localhost:3000`!

## 📂 Project Structure

```
my-eshop/
├── site.json                 # Main configuration
├── en-us/                   # Language content
│   ├── categories/          # Product categories
│   │   └── electronics/
│   │       ├── .category.md # Category metadata
│   │       ├── .products/   # Products in category
│   │       └── .images/     # Category images
│   ├── pages/              # Static pages
│   └── blog/               # Blog (optional)
├── templates/              # Custom templates (optional)
├── plugins/               # Plugins (optional)
└── dist/                 # Generated static files
```

## 🔧 CLI Commands

### Project Management
```bash
gitcart init <name>          # New project
gitcart build                # Build static files
gitcart dev [--port 3000]    # Dev server with hot reload
```

### Content Management
```bash
gitcart product add          # Add new product
gitcart category add         # Add new category
gitcart page add             # Add static page
gitcart blog post add        # Add blog article
```

### Plugin Management
```bash
gitcart plugin list          # Show installed plugins
gitcart plugin add <name>    # Install plugin
gitcart plugin create <name> # Create custom plugin
```

## 🎨 Templates and Themes

GitCart uses Liquid template engine (from Shopify/Jekyll) for maximum flexibility.

### Available Templates
- `default` - Complete e-shop template with cart
- `minimal` - Minimalist design

### Template Customization
```liquid
<!-- templates/layouts/product.liquid -->
<div class="product-detail">
  <h1>{{ product.title }}</h1>
  <div class="price">${{ product.price }} {{ product.currency }}</div>
  
  {% if seo %}
    {{ seo.meta_html }}
  {% endif %}
  
  {% zone 'product-detail-after-images' %}
</div>
```

### Rendering Zones
Plugins can inject content into predefined zones:
- `head` - SEO meta tags, tracking codes
- `product-detail-after-images` - Additional product content
- `order-success-info` - Thank you page information

## 🔍 SEO and Performance

### Automatic SEO Optimization
- ✅ Sitemap.xml with hreflang support
- ✅ Robots.txt with proper directives
- ✅ Meta tags (title, description, og, twitter)
- ✅ JSON-LD structured data (Product, Organization, BlogPosting)
- ✅ Canonical URLs for multilingual support
- ✅ Automatic image optimization

### Performance
- ⚡ Static HTML files (sub-second loading)
- 📦 Minified CSS/JS bundles
- 🖼️ Optimized images (WebP, responsive)
- 🔍 Client-side search with Fuse.js (12kb)
- 💾 Efficient caching strategy

## 🛒 E-commerce Features

### Frontend GitCart.js API
```javascript
// Add product to cart
GitCart.cart.add('IPHONE15', 1, {
  variant: 'blue-128gb'
});

// Start checkout process
GitCart.checkout.start();

// Search products
GitCart.search.query('iphone', {
  category: 'electronics',
  price: { min: 500, max: 1500 }
});

// Listen to events
GitCart.on('gitcart:cart:item:added', function(data) {
  console.log('Added to cart:', data);
});
```

### Supported Payment Gateways
- Stripe (plugin)
- PayPal (plugin)
- Cash on delivery (plugin)

### Shipping Options
- Standard shipping (plugin)
- Express delivery (plugin)

## 🔌 Plugin System

GitCart has a powerful plugin system for extending functionality.

### Sample Plugins
- 📊 **[Google Analytics](examples/plugins/google-analytics.js)** - E-commerce tracking
- 📧 **[Email Notifications](examples/plugins/email-notifications.js)** - Automatic order emails
- 💬 **[Chat Widget](examples/plugins/chat-widget.js)** - Live chat support (Tawk.to, Intercom)
- 📱 **[Social Share](examples/plugins/social-share.js)** - Social media sharing
- ⭐ **[Product Reviews](examples/plugins/product-reviews.js)** - Product ratings and reviews

### Creating Custom Plugin
```bash
gitcart plugin create my-plugin
```

```javascript
// plugins/my-plugin.js
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'my-plugin',
    version: '1.0.0'
  };

  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('my-plugin', {
      config: CONFIG,
      events: {
        'gitcart:product:viewed': function(product) {
          console.log('Product viewed:', product.title);
        }
      },
      render: {
        'product-detail-after-images': function(context) {
          return '<div>Plugin content</div>';
        }
      }
    });
  }
})();
```

## 🌍 Multilingual Support

Complete support for multiple languages with flexible URL strategies.

### Language Configuration
```json
{
  "languages": {
    "en-us": { 
      "name": "English", 
      "default": true, 
      "short": "en", 
      "currency": "USD" 
    },
    "es-es": { 
      "name": "Español", 
      "short": "es", 
      "currency": "EUR" 
    },
    "de-de": { 
      "name": "Deutsch", 
      "short": "de", 
      "currency": "EUR" 
    }
  },
  "url_strategy": "default_clean"
}
```

### URL Strategies
- `default_clean`: `/product/` (default), `/es/product/` (others)
- `always_full`: `/en-us/product/`, `/es-es/product/`
- `always_short`: `/en/product/`, `/es/product/`

### Content Structure
```
en-us/categories/electronics/...
es-es/categories/electronica/...
de-de/categories/elektronik/...
```

## 📊 Analytics and Tracking

### Integration with External Services
- Google Analytics 4 (plugin)
- Facebook Pixel (plugin)

## 🚀 Deployment

### Static Hosting Platforms
```bash
# Build for production
gitcart build

# Upload to hosting (manually)
# Upload dist/ folder to your hosting provider:
# - Netlify: drag & drop dist/ to Netlify
# - Vercel: vercel --prod
# - GitHub Pages: push dist/ to gh-pages branch
```

### Automatic CI/CD
```yaml
# .github/workflows/deploy.yml
name: Deploy GitCart
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm install
      - run: gitcart build
      - run: |
          # Deploy dist/ to GitHub Pages
          cd dist
          git init
          git add .
          git commit -m "Deploy"
          git push -f https://github.com/yourusername/yourrepo.git main:gh-pages
```

## 🧪 Testing

```bash
# Run all tests
npm test

# Run specific tests
npm test -- build.test.js

# Watch mode for development
npm run test:watch

# Coverage report
npm run test:coverage
```

## 📞 Support

### Community Support
- 📖 [Documentation](https://docs.gitcart.org)
- 🐛 [GitHub Issues](https://github.com/gitcart-org/gitcart-cli/issues)

## 📄 License

MIT License - see [LICENSE](../LICENSE) file for details.