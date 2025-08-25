# GitCart - Dokumentace (Čeština)

<div align="center">

![GitCart Logo](https://img.shields.io/badge/GitCart-v0.1.2-blue?style=for-the-badge)
![Node.js](https://img.shields.io/badge/Node.js-18+-green?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

**Univerzální generátor statických e-shopů z markdown souborů**

</div>

---

## 📚 Kompletní dokumentace

### Návody pro uživatele
- [📖 Uživatelská příručka](user-guide.md) - Kompletní průvodce používáním GitCart
- [⚙️ Konfigurace](configuration.md) - Detailní referenční příručka pro konfiguraci
- [🌍 Vícejazyčnost](multilingual.md) - Vytváření vícejazyčných e-shopů
- [📊 SEO optimalizace](seo.md) - SEO optimalizace a výkon

### Pro vývojáře
- [🎨 Template systém](templates.md) - Tvorba vlastních templates a témat
- [📚 Taiwind CSS integrace](tailwind-integration.md) - Integrace Tailwind CSS pro vlastní styly
- [🔌 Plugin systém](plugins.md) - Vývoj rozšíření a pluginů
- [💻 Frontend API](frontend-api.md) - JavaScript API pro frontend funkcionalitu
- [🔧 Plugin API](plugin-api.md) - API reference pro vývoj pluginů

### Reference
- [🛠️ CLI příkazy](cli-reference.md) - Kompletní přehled CLI příkazů a možností

---

## 🎯 Co je GitCart?

GitCart je moderní, lightweight generátor statických e-commerce webů, který vytváří rychlé a SEO-optimalizované e-shopy z jednoduchých markdown souborů a JSON konfigurace. Ideální pro malé až střední e-shopy, které nepotřebují složité CMS, ale chtějí profesionální a rychlé řešení.

### ✨ Klíčové vlastnosti

- 📁 **Jednoduchá struktura** - Produkty a kategorie v markdown souborech
- 🌍 **Vícejazyčnost** - Plná podpora více jazyků a locale
- 🎨 **Flexibilní templating** - Liquid templates s možností customizace
- 🔍 **SEO optimalizované** - Automatické sitemapy, meta tagy, structured data
- 🛒 **E-commerce funkce** - Košík, checkout, produktové filtry
- 🔌 **Plugin systém** - Rozšiřitelnost pomocí JavaScriptových pluginů
- 📱 **Responzivní design** - Mobile-first přístup s Tailwind CSS
- ⚡ **Rychlé výsledky** - Statické soubory pro maximální rychlost
- 🚀 **Easy deployment** - GitHub/GitLab Pages, Netlify, Vercel ready

## 🚀 Rychlý start

### Instalace

```bash
# Globální instalace CLI
npm install -g gitcart-cli

# Nebo lokální instalace
npm install gitcart-cli
```

### Vytvoření nového projektu

```bash
# Interaktivní vytvoření projektu
gitcart init my-eshop

# S předvoleným template
gitcart init my-eshop --template default

# S minimalistickým template
gitcart init my-eshop --template minimal

# Přejít do složky projektu
cd my-eshop
```

### Základní konfigurace

Editujte `site.json`:

```json
{
  "name": "Můj E-shop",
  "url": "https://myshop.com",
  "languages": {
    "cs-cz": {
      "name": "Čeština", 
      "default": true, 
      "short": "cs", 
      "currency": "CZK"
    }
  },
  "variables": {
    "contact": {
      "email": "info@myshop.com",
      "phone": "+420 123 456 789"
    }
  }
}
```

### Přidání prvního produktu

```bash
# Interaktivní vytvoření produktu
gitcart product add

# Nebo ručně vytvořte soubor cs-cz/categories/elektronika/.products/iphone-15.md
```

```markdown
---
title: iPhone 15
price: 25999
sku: IPHONE15-128
images: 
  - iphone-front.jpg
  - iphone-back.jpg
tags: [smartphone, apple]
featured: true
---

# iPhone 15

Nejnovější iPhone s USB-C konektorem a pokročilými funkcemi.

## Specifikace
- 128GB úložiště
- A17 Bionic čip
- 48MP fotoaparát
```

### Build a preview

```bash
# Vybuildit stránky
gitcart build

# Spustit dev server s live reload
gitcart dev --watch
```

Váš e-shop je připraven na `http://localhost:3000`!

## 📂 Struktura projektu

```
my-eshop/
├── site.json                 # Hlavní konfigurace
├── cs-cz/                   # Jazykové obsahy
│   ├── categories/          # Kategorie produktů
│   │   └── elektronika/
│   │       ├── .category.md # Metadata kategorie
│   │       ├── .products/   # Produkty v kategorii
│   │       └── .images/     # Obrázky kategorie
│   ├── pages/              # Statické stránky
│   └── blog/               # Blog (volitelné)
├── templates/              # Custom templates (volitelné)
├── plugins/               # Pluginy (volitelné)
└── dist/                 # Výsledné statické soubory
```

## 🔧 CLI příkazy

### Správa projektu
```bash
gitcart init <název>          # Nový projekt
gitcart build                 # Vybuildit statické soubory
gitcart dev [--port 3000]     # Dev server s hot reload
```

### Správa obsahu
```bash
gitcart product add           # Přidat nový produkt
gitcart category add          # Přidat novou kategorii  
gitcart page add              # Přidat statickou stránku
gitcart blog post add         # Přidat blog článek
```

### Správa pluginů
```bash
gitcart plugin list           # Zobrazit nainstalované pluginy
gitcart plugin add <název>    # Nainstalovat plugin
gitcart plugin create <název> # Vytvořit vlastní plugin
```

## 🎨 Templates a témata

GitCart používá Liquid template engine (ze Shopify/Jekyll) pro maximální flexibilitu.

### Dostupné templates
- `default` - Kompletní e-shop template s košíkem
- `minimal` - Minimalistický design

### Customizace template
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
Pluginy mohou vkládat obsah do předdefinovaných zón:
- `head` - SEO meta tagy, tracking kódy
- `product-detail-after-images` - Doplňkový obsah u produktů
- `order-success-info` - Informace po dokončení objednávky

## 🔍 SEO a výkon

### Automatické SEO optimalizace
- ✅ Sitemap.xml s hreflang podporou
- ✅ Robots.txt s proper direktivami
- ✅ Meta tagy (title, description, og, twitter)
- ✅ JSON-LD structured data (Product, Organization, BlogPosting)
- ✅ Canonical URLs pro multilingual support
- ✅ Automatická optimalizace obrázků

### Výkon
- ⚡ Statické HTML soubory (sub-second loading)
- 📦 Minimalizované CSS/JS bundles
- 🖼️ Optimalizované obrázky (WebP, responsive)
- 🔍 Client-side search s Fuse.js (12kb)
- 💾 Efektivní caching strategie

## 🛒 E-commerce funkce

### Frontend GitCart.js API
```javascript
// Přidat produkt do košíku
GitCart.cart.add('IPHONE15', 1, {
  variant: 'blue-128gb'
});

// Spustit checkout proces
GitCart.checkout.start();

// Vyhledat produkty
GitCart.search.query('iphone', {
  category: 'elektronika',
  price: { min: 10000, max: 30000 }
});

// Poslouchat události
GitCart.on('gitcart:cart:item:added', function(data) {
  console.log('Přidáno do košíku:', data);
});
```

### Podporované platební brány
- Stripe (plugin)
- PayPal (plugin)
- Platba na dobírku (plugin)

### Dodací možnosti
- Zásilkovna (plugin)
- Osobní odběr (plugin)

## 🔌 Plugin systém

GitCart má výkonný plugin systém pro rozšíření funkcionality.

### Vzorové pluginy
- 📊 **[Google Analytics](examples/plugins/google-analytics.js)** - E-commerce tracking
- 📧 **[Email Notifications](examples/plugins/email-notifications.js)** - Automatické emaily pro objednávky
- 💬 **[Chat Widget](examples/plugins/chat-widget.js)** - Live chat podpora (Tawk.to, Intercom)
- 📱 **[Social Share](examples/plugins/social-share.js)** - Sdílení na sociálních sítích
- ⭐ **[Product Reviews](examples/plugins/product-reviews.js)** - Hodnocení a recenze produktů

### Vytvoření vlastního pluginu
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
          console.log('Produkt zobrazen:', product.title);
        }
      },
      render: {
        'product-detail-after-images': function(context) {
          return '<div>Plugin obsah</div>';
        }
      }
    });
  }
})();
```

## 🌍 Vícejazyčnost

Kompletní podpora více jazyků s flexibilními URL strategiami.

### Konfigurace jazyků
```json
{
  "languages": {
    "cs-cz": { 
      "name": "Čeština", 
      "default": true, 
      "short": "cs", 
      "currency": "CZK" 
    },
    "en-us": { 
      "name": "English", 
      "short": "en", 
      "currency": "USD" 
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

### URL strategie
- `default_clean`: `/produkt/` (výchozí), `/en/product/` (ostatní)
- `always_full`: `/cs-cz/produkt/`, `/en-us/product/`
- `always_short`: `/cs/produkt/`, `/en/product/`

### Struktura obsahu
```
cs-cz/categories/elektronika/...
en-us/categories/electronics/...
de-de/categories/elektronik/...
```

## 📊 Analytics a tracking

### Integrace s externími službami
- Google Analytics 4 (plugin)
- Facebook Pixel (plugin)

## 🚀 Deployment

### Statické hosting platformy
```bash
# Vybuildit pro produkci
gitcart build

# Nahrání na hosting (manuálně)
# dist/ složku nahrajte na váš hosting provider:
# - Netlify: drag & drop dist/ do Netlify
# - Vercel: vercel --prod
# - GitHub Pages: push dist/ do gh-pages branch
```

### Automatická CI/CD
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
# Spustit všechny testy
npm test

# Spustit specifické testy
npm test -- build.test.js

# Watch mode pro vývoj
npm run test:watch

# Coverage report
npm run test:coverage
```

## 📞 Podpora

### Community podpora
- 📖 [Dokumentace](https://docs.gitcart.org)
- 🐛 [GitHub Issues](https://github.com/gitcart-org/gitcart-cli/issues)

## 📄 Licence

MIT License - viz [LICENSE](../LICENSE) soubor pro detaily.