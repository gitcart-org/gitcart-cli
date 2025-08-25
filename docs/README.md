# GitCart - Dokumentace

<div align="center">

**Univerzální generátor statických e-shopů z markdown souborů**

[🌍 English](#english) | [🇨🇿 Čeština](#čeština)

</div>

---

## 🇨🇿 Čeština

### 📚 Kompletní dokumentace

#### 📖 Návody pro uživatele
- [**📖 Uživatelská příručka**](cs-cz/user-guide.md) - Kompletní průvodce používáním GitCart od A do Z
- [**⚙️ Konfigurace**](cs-cz/configuration.md) - Detailní referenční příručka pro konfiguraci projektu
- [**🌍 Vícejazyčnost**](cs-cz/multilingual.md) - Vytváření a správa vícejazyčných e-shopů
- [**📊 SEO optimalizace**](cs-cz/seo.md) - SEO optimalizace, výkon a structured data

#### 🔧 Pro vývojáře
- [**🎨 Template systém**](cs-cz/templates.md) - Tvorba vlastních templates a témat pomocí Liquid
- [**💄 Tailwind CSS integrace**](cs-cz/tailwind-integration.md) - Integrace a customizace Tailwind CSS
- [**🔌 Plugin systém**](cs-cz/plugins.md) - Vývoj rozšíření a pluginů pro GitCart
- [**💻 Frontend API**](cs-cz/frontend-api.md) - JavaScript GitCart.js API reference
- [**🔧 Plugin API**](cs-cz/plugin-api.md) - Komplexní API reference pro vývoj pluginů
- [**🎯 Plugin Zones**](cs-cz/plugin-zones.md) - Přehled všech rendering zones pro pluginy

#### 📚 Reference
- [**🛠️ CLI Reference**](cs-cz/cli-reference.md) - Kompletní přehled všech CLI příkazů a možností

### 🚀 Rychlý start

```bash
# Instalace
npm install -g gitcart-cli

# Vytvoření nového e-shopu
gitcart init my-eshop

# Spuštění dev serveru
cd my-eshop
gitcart dev --watch
```

Váš e-shop bude dostupný na `http://localhost:3000`

### 🎯 Co je GitCart?

GitCart je moderní, lightweight generátor statických e-commerce webů, který vytváří rychlé a SEO-optimalizované e-shopy z jednoduchých markdown souborů a JSON konfigurace. Ideální pro malé až střední e-shopy, které nepotřebují složité CMS.

**Klíčové vlastnosti:**
- 📁 Jednoduchá struktura v markdown
- 🌍 Vícejazyčnost s flexibilními URL strategiemi
- 🎨 Liquid templates s kompletním plugin systémem
- 🔍 Automatická SEO optimalizace
- 🛒 Plné e-commerce funkce (košík, checkout, filtry)
- ⚡ Statické soubory pro maximální rychlost

---

## 🌍 English

### 📚 Complete Documentation

#### 📖 User Guides
- [**📖 User Guide**](en-us/user-guide.md) - Complete GitCart usage guide from A to Z
- [**⚙️ Configuration**](en-us/configuration.md) - Detailed configuration reference manual
- [**🌍 Multilingual**](en-us/multilingual.md) - Creating and managing multilingual e-shops
- [**📊 SEO Optimization**](en-us/seo.md) - SEO optimization, performance and structured data

#### 🔧 For Developers
- [**🎨 Template System**](en-us/templates.md) - Creating custom templates and themes using Liquid
- [**💄 Tailwind CSS Integration**](en-us/tailwind-integration.md) - Tailwind CSS integration and customization
- [**🔌 Plugin System**](en-us/plugins.md) - Developing extensions and plugins for GitCart
- [**💻 Frontend API**](en-us/frontend-api.md) - JavaScript GitCart.js API reference
- [**🔧 Plugin API**](en-us/plugin-api.md) - Comprehensive API reference for plugin development
- [**🎯 Plugin Zones**](en-us/plugin-zones.md) - Overview of all rendering zones for plugins

#### 📚 Reference
- [**🛠️ CLI Reference**](en-us/cli-reference.md) - Complete overview of all CLI commands and options

### 🚀 Quick Start

```bash
# Installation
npm install -g gitcart-cli

# Create new e-shop
gitcart init my-eshop

# Start dev server
cd my-eshop
gitcart dev --watch
```

Your e-shop will be available at `http://localhost:3000`

### 🎯 What is GitCart?

GitCart is a modern, lightweight static e-commerce site generator that creates fast and SEO-optimized e-shops from simple markdown files and JSON configuration. Perfect for small to medium e-shops that don't need complex CMS.

**Key Features:**
- 📁 Simple markdown-based structure
- 🌍 Multilingual support with flexible URL strategies
- 🎨 Liquid templates with complete plugin system
- 🔍 Automatic SEO optimization
- 🛒 Full e-commerce features (cart, checkout, filters)
- ⚡ Static files for maximum performance

---

## 📂 Project Structure Overview

```
my-eshop/
├── site.json                 # Main configuration
├── cs-cz/                   # Language content (Czech)
│   ├── categories/          # Product categories
│   │   └── elektronika/
│   │       ├── .category.md # Category metadata
│   │       ├── .products/   # Products in category
│   │       └── .images/     # Category images
│   ├── pages/              # Static pages
│   └── blog/               # Blog (optional)
├── en-us/                   # Language content (English)
├── templates/              # Custom templates (optional)
├── plugins/               # Plugins (optional)
└── dist/                 # Generated static files
```

## 🔧 Essential CLI Commands

```bash
# Project management
gitcart init <name>           # Create new project
gitcart build                 # Build static files
gitcart dev [--port 3000]     # Dev server with hot reload

# Content management
gitcart product add           # Add new product
gitcart category add          # Add new category
gitcart page add              # Add static page
gitcart blog post add         # Add blog post

# Plugin management
gitcart plugin list           # List installed plugins
gitcart plugin create <name>  # Create custom plugin
```

## 🌍 Multilingual Support

GitCart provides complete multilingual support with flexible URL strategies:

```json
{
  "languages": {
    "cs-cz": { "name": "Čeština", "default": true, "short": "cs", "currency": "CZK" },
    "en-us": { "name": "English", "short": "en", "currency": "USD" },
    "de-de": { "name": "Deutsch", "short": "de", "currency": "EUR" }
  },
  "url_strategy": "default_clean"
}
```

## 🔌 Plugin System

Extend GitCart with powerful JavaScript plugins:

```javascript
GitCart.plugin('my-plugin', {
  events: {
    'gitcart:product:viewed': function(product) {
      console.log('Product viewed:', product.title);
    }
  },
  render: {
    'product-detail-after-images': function(context) {
      return '<div>Custom plugin content</div>';
    }
  }
});
```

## 📊 Key Features

### ✨ E-commerce Features
- 🛒 Shopping cart with localStorage persistence
- 💳 Checkout process with customizable steps
- 🔍 Client-side search with Fuse.js
- 📱 Mobile-first responsive design
- 🎨 Customizable Liquid templates

### 🚀 Performance & SEO
- ⚡ Static HTML files (sub-second loading)
- 📊 Automatic sitemap.xml with hreflang
- 🏷️ Meta tags and structured data (JSON-LD)
- 🖼️ Image optimization (WebP, responsive)
- 📦 Minimized CSS/JS bundles

### 🔧 Developer Experience
- 🔥 Hot reload development server
- 📦 Plugin ecosystem
- 🎯 Zone-based content injection
- 🌍 Multilingual content management
- ⚙️ Comprehensive configuration options

## 🚀 Deployment

Deploy to any static hosting provider:

```bash
# Build for production
gitcart build

# Deploy dist/ folder to:
# - Netlify: drag & drop
# - Vercel: vercel --prod
# - GitHub Pages: push to gh-pages branch
# - Any static hosting provider
```

## 📞 Support & Community

- 📖 [Documentation](https://docs.gitcart.org)
- 🐛 [GitHub Issues](https://github.com/gitcart-org/gitcart-cli/issues)
- 💬 [Discussions](https://github.com/gitcart-org/gitcart-cli/discussions)

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

<div align="center">

**Made with ❤️ by the GitCart Team**

[⭐ Star us on GitHub](https://github.com/gitcart-org/gitcart-cli) | [🐛 Report Bug](https://github.com/gitcart-org/gitcart-cli/issues) | [💡 Request Feature](https://github.com/gitcart-org/gitcart-cli/issues)

</div>