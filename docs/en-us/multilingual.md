# GitCart - Multilingual Support

Complete guide for creating multilingual e-shops with GitCart.

## 🌍 Multilingual Support Overview

GitCart provides complete support for multilingual e-shops with flexible URL strategies, automatic hreflang tag generation, and efficient content management.

### Key Features

- **Unlimited languages** - Support for any number of languages
- **Flexible URL strategies** - 4 different URL generation approaches
- **Automatic hreflang** - SEO optimization for search engines
- **Content isolation** - Each language has its own content
- **Language switching** - Automatic switching between languages
- **Locale-aware formatting** - Formatting according to local conventions

## ⚙️ Language Configuration

### Basic Configuration in site.json

```json
{
  "name": "Multilingual Shop",
  "url": "https://shop.com",
  
  "languages": {
    "cs-cz": {
      "name": "Čeština",
      "default": true,
      "short": "cs",
      "currency": "CZK",
      "date_format": "d.m.Y",
      "number_format": {
        "decimal_separator": ",",
        "thousands_separator": " "
      },
      "rtl": false
    },
    "en-us": {
      "name": "English",
      "short": "en",
      "currency": "USD", 
      "date_format": "m/d/Y",
      "number_format": {
        "decimal_separator": ".",
        "thousands_separator": ","
      },
      "rtl": false
    },
    "ar-sa": {
      "name": "العربية",
      "short": "ar",
      "currency": "SAR",
      "date_format": "d/m/Y",
      "number_format": {
        "decimal_separator": ".",
        "thousands_separator": ","
      },
      "rtl": true
    }
  },
  
  "url_strategy": "default_clean",
  "url_locale_format": "short"
}
```

### Language Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | ✅ | Human-readable language name |
| `default` | boolean | - | Default language (only one can be true) |
| `short` | string | ✅ | Short code for URLs (cs, en, de) |
| `currency` | string | ✅ | Currency according to ISO 4217 (CZK, EUR, USD) |
| `date_format` | string | - | PHP-style date format (default: d.m.Y) |
| `number_format` | object | - | Number formatting |
| `rtl` | boolean | - | Right-to-left language (default: false) |

### Number Formatting

```json
{
  "number_format": {
    "decimal_separator": ",",     // Decimal separator
    "thousands_separator": " ",   // Thousands separator
    "decimals": 2                // Number of decimal places
  }
}
```

## 🔗 URL Strategies

GitCart supports 4 different URL strategies for multilingual websites:

### 1. "default_clean" (recommended)

Default language without prefix, others with short code:

```
Czech (default): https://shop.com/products/iphone-15/
English:         https://shop.com/en/products/iphone-15/
German:          https://shop.com/de/products/iphone-15/
```

### 2. "always_full"

All languages with full locale code:

```
Czech:   https://shop.com/cs-cz/products/iphone-15/
English: https://shop.com/en-us/products/iphone-15/
German:  https://shop.com/de-de/products/iphone-15/
```

### 3. "always_short"

All languages with short code:

```
Czech:   https://shop.com/cs/products/iphone-15/
English: https://shop.com/en/products/iphone-15/
German:  https://shop.com/de/products/iphone-15/
```

### 4. "default_clean_full"

Default without prefix, others with full locale:

```
Czech (default): https://shop.com/products/iphone-15/
English:         https://shop.com/en-us/products/iphone-15/
German:          https://shop.com/de-de/products/iphone-15/
```

### url_locale_format

Determines the format of locale codes in URLs:

```json
{
  "url_locale_format": "short"  // cs, en, de
  "url_locale_format": "full"   // cs-cz, en-us, de-de  
}
```

## 📁 Content Structure

### Language Version Organization

```
project/
├── site.json
├── cs-cz/                      # Czech content
│   ├── categories/
│   │   └── electronics/
│   │       ├── .category.md
│   │       ├── .products/
│   │       │   ├── iphone-15.md
│   │       │   └── samsung-galaxy.md
│   │       └── .images/
│   ├── pages/
│   │   ├── contact.md
│   │   ├── about-us.md
│   │   └── shipping-payment.md
│   └── blog/
│       ├── .config.md
│       └── 2024/
│           └── how-to-choose-phone.md
├── en-us/                      # English content
│   ├── categories/
│   │   └── electronics/
│   │       ├── .category.md
│   │       ├── .products/
│   │       │   ├── iphone-15.md
│   │       │   └── samsung-galaxy.md
│   │       └── .images/
│   ├── pages/
│   │   ├── contact.md
│   │   ├── about-us.md
│   │   └── shipping-payment.md
│   └── blog/
│       ├── .config.md
│       └── 2024/
│           └── how-to-choose-phone.md
└── de-de/                      # German content
    ├── categories/
    │   └── elektronik/
    │       ├── .category.md
    │       ├── .products/
    │       └── .images/
    └── pages/
        ├── kontakt.md
        ├── ueber-uns.md
        └── versand-zahlung.md
```

### Sharing Images Between Languages

```
cs-cz/categories/electronics/.images/
├── category-banner.jpg          # Shared between languages
├── iphone-15-front.jpg         
└── iphone-15-back.jpg          

# English version can use same images
en-us/categories/electronics/.images/
├── category-banner-en.jpg       # Or localized versions
├── iphone-15-front.jpg         # Symlink or copy
└── iphone-15-back.jpg
```

## 🗂️ Creating Multilingual Content

### Products in Different Languages

#### Czech Product (cs-cz/categories/electronics/.products/iphone-15.md)

```markdown
---
title: iPhone 15
price: 25999
sale_price: 23999
sku: IPHONE15-128-BLUE
stock: 15
currency: CZK
variant: blue-128gb
featured: true
tags: 
  - smartphone
  - apple
  - 5g
images: 
  - iphone-15-blue-front.jpg
  - iphone-15-blue-back.jpg
specifications:
  display: "6.1\" Super Retina XDR"
  processor: "A17 Bionic chip"
  storage: "128GB storage"
  camera: "48MP Main + 12MP Ultra Wide"
seo_title: "iPhone 15 128GB Blue - best price"
seo_description: "iPhone 15 with A17 Bionic chip and USB-C. Free shipping over 1000 CZK!"
---

# iPhone 15 - A New Era of Innovation

iPhone 15 represents a major leap forward in smartphone technology. With revolutionary USB-C connector, powerful A17 Bionic chip, and advanced camera system.

## ⭐ Key Features

- **A17 Bionic chip** for incredible performance
- **48MP camera** with advanced features
- **USB-C connector** for universal connectivity
- **Up to 20 hours** of battery life
```

#### English Product (en-us/categories/electronics/.products/iphone-15.md)

```markdown
---
title: iPhone 15
price: 999
sale_price: 899
sku: IPHONE15-128-BLUE
stock: 15
currency: USD
variant: blue-128gb
featured: true
tags: 
  - smartphone
  - apple
  - 5g
images: 
  - iphone-15-blue-front.jpg
  - iphone-15-blue-back.jpg
specifications:
  display: "6.1\" Super Retina XDR"
  processor: "A17 Bionic chip"
  storage: "128GB storage"
  camera: "48MP Main + 12MP Ultra Wide"
seo_title: "iPhone 15 128GB Blue - Best Price"
seo_description: "iPhone 15 with A17 Bionic chip and USB-C. Free shipping over $50!"
---

# iPhone 15 - A New Era of Innovation

The iPhone 15 represents a major leap forward in smartphone technology. With the revolutionary USB-C connector, powerful A17 Bionic chip, and advanced camera system.

## ⭐ Key Features

- **A17 Bionic chip** for incredible performance
- **48MP camera** with advanced features
- **USB-C connector** for universal connectivity
- **Up to 20 hours** of battery life
```

### Categories in Different Languages

#### Czech Category (cs-cz/categories/electronics/.category.md)

```markdown
---
title: Electronics
description: Latest electronic devices at great prices
sort_order: 1
featured: true
banner_image: electronics-banner.jpg
seo_title: "Electronics - phones, computers & more | Our Shop"
seo_description: "Wide selection of electronics. iPhone, Samsung, laptops and accessories. Free shipping over 1000 CZK."
---

# Electronics

Discover the latest electronic devices at the best prices. From smartphones to laptops to smartwatches.

## 📱 What You'll Find

- **Mobile phones** - iPhone, Samsung, Xiaomi
- **Computers** - Laptops, PC builds, tablets
- **Audio** - Headphones, speakers, home theater
- **Smart devices** - Smartwatches, fitness trackers
```

#### English Category (en-us/categories/electronics/.category.md)

```markdown
---
title: Electronics
description: Latest electronic devices at great prices
sort_order: 1
featured: true
banner_image: electronics-banner.jpg
seo_title: "Electronics - Phones, Computers & More | Our Shop"
seo_description: "Wide selection of electronics. iPhone, Samsung, laptops and accessories. Free shipping over $50."
---

# Electronics

Discover the latest electronic devices at the best prices. From smartphones to laptops to smartwatches.

## 📱 What You'll Find

- **Mobile phones** - iPhone, Samsung, Xiaomi
- **Computers** - Laptops, PC builds, tablets
- **Audio** - Headphones, speakers, home theater
- **Smart devices** - Smartwatches, fitness trackers
```

### Static Pages

#### Contact in Czech (cs-cz/pages/contact.md)

```markdown
---
title: Contact
description: Contact information for our e-shop
nav_footer: true
nav_order: 1
icon: phone
template: contact
---

# Contact Us

Have a question? We're here to help!

## 📞 Phone Support

**Customer Service:** +420 123 456 789
- Mon-Fri: 8:00 AM - 5:00 PM
- Weekends: Closed

## ✉️ Email

**General inquiries:** info@our-shop.cz
**Orders:** orders@our-shop.cz
**Returns:** returns@our-shop.cz

## 🏢 Store Location

Our store in downtown Prague:

**Address:**
Wenceslas Square 1
110 00 Prague 1

**Store Hours:**
- Monday - Friday: 9:00 AM - 7:00 PM
- Saturday: 9:00 AM - 3:00 PM
- Sunday: Closed
```

#### Contact in English (en-us/pages/contact.md)

```markdown
---
title: Contact
description: Contact information for our e-shop
nav_footer: true
nav_order: 1
icon: phone
template: contact
---

# Contact Us

Have a question? We're here to help!

## 📞 Phone Support

**Customer Service:** +1 555 123 4567
- Mon-Fri: 8:00 AM - 5:00 PM EST
- Weekends: Closed

## ✉️ Email

**General inquiries:** info@our-shop.com
**Orders:** orders@our-shop.com
**Returns:** returns@our-shop.com

## 🏢 Store Location

Visit our flagship store in downtown:

**Address:**
123 Main Street
New York, NY 10001

**Store Hours:**
- Monday - Friday: 9:00 AM - 7:00 PM
- Saturday: 9:00 AM - 3:00 PM
- Sunday: Closed
```

## 🔧 Templates for Multilingual Support

### Language Switcher Component

```liquid
<!-- templates/component/language-switcher.liquid -->
{% if site.languages.size > 1 %}
  <div class="language-switcher">
    <button class="lang-current" onclick="toggleLangMenu()" aria-label="Change language">
      <span class="flag flag-{{ current_language.short }}"></span>
      {{ current_language.short | upcase }}
      <svg class="chevron" width="12" height="12" viewBox="0 0 12 12">
        <path d="M2 4l4 4 4-4" stroke="currentColor" stroke-width="2" fill="none"/>
      </svg>
    </button>
    
    <ul class="lang-menu" style="display: none;">
      {% for lang_code, language in site.languages %}
        {% unless lang_code == current_language.code %}
          <li>
            <a href="{{ page.translations[lang_code].url | default: '/' | prepend: language.base_url }}"
               class="lang-option"
               hreflang="{{ lang_code }}">
              <span class="flag flag-{{ language.short }}"></span>
              {{ language.name }}
            </a>
          </li>
        {% endunless %}
      {% endfor %}
    </ul>
  </div>
{% endif %}
```

### RTL Support in Base Layout

```liquid
<!-- templates/layout/base.liquid -->
<html lang="{{ current_language.code }}" 
      {% if current_language.rtl %}dir="rtl"{% endif %}>
<head>
  <!-- Standard meta tags -->
  <title>{{ page.seo_title | default: page.title }}{% if site.seo.title_suffix %}{{ site.seo.title_suffix }}{% endif %}</title>
  <meta name="description" content="{{ page.seo_description | default: page.description }}">
  
  <!-- Hreflang links -->
  {% for lang_code, language in site.languages %}
    {% if page.translations[lang_code] %}
      <link rel="alternate" 
            hreflang="{{ lang_code }}" 
            href="{{ page.translations[lang_code].url | absolute_url }}">
    {% endif %}
  {% endfor %}
  
  <!-- x-default hreflang -->
  {% assign default_lang = site.languages | where: 'default', true | first %}
  {% if default_lang and page.translations[default_lang[0]] %}
    <link rel="alternate" 
          hreflang="x-default" 
          href="{{ page.translations[default_lang[0]].url | absolute_url }}">
  {% endif %}
  
  <!-- RTL specific styles -->
  {% if current_language.rtl %}
    <link rel="stylesheet" href="{{ 'css/rtl.css' | asset_url }}">
  {% endif %}
</head>
<body class="{% if current_language.rtl %}rtl{% endif %} lang-{{ current_language.short }}">
  <!-- Content -->
</body>
</html>
```

### Localized Formatting

```liquid
<!-- Price formatting by locale -->
<span class="price">
  {{ product.price | money: current_language.currency, current_language.number_format }}
</span>

<!-- Date formatting -->
<time datetime="{{ post.date | date: '%Y-%m-%d' }}">
  {{ post.date | date: current_language.date_format }}
</time>

<!-- Pluralization by language -->
{% case current_language.short %}
  {% when 'cs' %}
    {{ product.stock | pluralize: 'piece', 'pieces', 'pieces' }}
  {% when 'en' %}
    {{ product.stock | pluralize: 'piece', 'pieces' }}
  {% when 'de' %}
    {{ product.stock | pluralize: 'Stück', 'Stück' }}
{% endcase %}
```

## 🌐 SEO for Multilingual Pages

### Automatic Hreflang Tags

GitCart automatically generates hreflang tags for all languages:

```html
<!-- For Czech version /products/iphone-15/ -->
<link rel="alternate" hreflang="cs-cz" href="https://shop.com/products/iphone-15/">
<link rel="alternate" hreflang="en-us" href="https://shop.com/en/products/iphone-15/">
<link rel="alternate" hreflang="de-de" href="https://shop.com/de/products/iphone-15/">
<link rel="alternate" hreflang="x-default" href="https://shop.com/products/iphone-15/">
```

### Sitemap with Multilingual Support

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xhtml="http://www.w3.org/1999/xhtml">
  
  <!-- Czech product -->
  <url>
    <loc>https://shop.com/products/iphone-15/</loc>
    <lastmod>2024-01-15</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
    
    <xhtml:link rel="alternate" hreflang="cs-cz" 
                href="https://shop.com/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="en-us" 
                href="https://shop.com/en/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="de-de" 
                href="https://shop.com/de/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="x-default" 
                href="https://shop.com/products/iphone-15/"/>
  </url>
  
  <!-- English product -->
  <url>
    <loc>https://shop.com/en/products/iphone-15/</loc>
    <lastmod>2024-01-15</lastmod>
    
    <xhtml:link rel="alternate" hreflang="cs-cz" 
                href="https://shop.com/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="en-us" 
                href="https://shop.com/en/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="de-de" 
                href="https://shop.com/de/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="x-default" 
                href="https://shop.com/products/iphone-15/"/>
  </url>
  
</urlset>
```

### Structured Data for Multilingual Products

```json
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "iPhone 15",
  "description": "iPhone 15 with A17 Bionic chip and USB-C",
  "sku": "IPHONE15-128-BLUE",
  "offers": {
    "@type": "Offer",
    "url": "https://shop.com/en/products/iphone-15/",
    "priceCurrency": "USD",
    "price": "899",
    "availability": "https://schema.org/InStock",
    "seller": {
      "@type": "Organization",
      "name": "Our Shop"
    }
  },
  "alternativeHeadline": [
    {
      "@language": "cs-cz",
      "@value": "iPhone 15 - Nová éra inovací"
    },
    {
      "@language": "en-us", 
      "@value": "iPhone 15 - A New Era of Innovation"
    }
  ]
}
```

## 🛠️ CLI for Multilingual Content

### Creating Content in Multiple Languages

```bash
# Create product in all languages
gitcart product add --multilingual
? Product name (cs-cz): iPhone 15
? Product name (en-us): iPhone 15  
? Product name (de-de): iPhone 15
? SKU: IPHONE15-128
? Base price: 25999
? Currency mapping:
  cs-cz: 25999 CZK
  en-us: 999 USD
  de-de: 899 EUR

# Create category in multiple languages
gitcart category add --multilingual
? Category name (cs-cz): Elektronika
? Category name (en-us): Electronics
? Category name (de-de): Elektronik
? URL slug (cs-cz): elektronika
? URL slug (en-us): electronics  
? URL slug (de-de): elektronik

# Create page in multiple languages
gitcart page add --multilingual
? Page name (cs-cz): Kontakt
? Page name (en-us): Contact
? Page name (de-de): Kontakt
```

### Content Synchronization

```bash
# Check for missing translations
gitcart multilingual check
❌ Missing translations found:
  Product 'samsung-galaxy' missing in: en-us, de-de
  Page 'shipping-payment' missing in: en-us
  Category 'electronics' missing translation in: de-de

# Create templates for missing translations
gitcart multilingual scaffold --missing
✅ Created translation templates:
  en-us/categories/electronics/.products/samsung-galaxy.md
  en-us/pages/shipping-payment.md
  de-de/categories/electronics/.category.md
```

### Bulk Operations

```bash
# Bulk price updates by currency
gitcart multilingual update-prices --exchange-rates api
✅ Updated prices:
  cs-cz: 25999 CZK
  en-us: 999 USD (rate: 26.0)
  de-de: 899 EUR (rate: 28.9)

# Bulk SEO metadata check
gitcart multilingual seo-check
⚠️ SEO issues found:
  cs-cz/products/iphone-15: Missing meta description
  en-us/products/iphone-15: Title too short (< 30 chars)
```

## 🎯 Best Practices

### 1. Consistent SKUs Across Languages

```markdown
<!-- cs-cz -->
---
sku: IPHONE15-128-BLUE
title: iPhone 15 128GB Blue
price: 25999
currency: CZK
---

<!-- en-us -->
---
sku: IPHONE15-128-BLUE  # Same SKU
title: iPhone 15 128GB Blue
price: 999
currency: USD
---
```

### 2. Price and Currency Localization

```json
{
  "languages": {
    "cs-cz": {
      "currency": "CZK",
      "price_suffix": " Kč",
      "price_format": "{amount} {currency}"
    },
    "en-us": {
      "currency": "USD", 
      "price_prefix": "$",
      "price_format": "{currency}{amount}"
    },
    "de-de": {
      "currency": "EUR",
      "price_suffix": " €",
      "price_format": "{amount} {currency}"
    }
  }
}
```

### 3. Image Sharing

```bash
# Hard links for sharing images
ln cs-cz/categories/electronics/.images/iphone-15.jpg \
   en-us/categories/electronics/.images/iphone-15.jpg
   
# Or symbolic links
ln -s ../../cs-cz/categories/electronics/.images/iphone-15.jpg \
      en-us/categories/electronics/.images/iphone-15.jpg
```

### 4. Variables for Multilingual Sites

For multilingual projects you can define variables per-language:

```json
{
  "variables": {
    "global": {
      "company": {
        "name": "My E-shop Ltd.",
        "founded": 2020,
        "vat_id": "CZ12345678"
      },
      "social": {
        "facebook": "https://facebook.com/myshop",
        "instagram": "@myshop"
      }
    },
    "cs-cz": {
      "contact": {
        "email": "info@eshop.cz",
        "phone": "+420 123 456 789",
        "address": "Wenceslas Square 1, Prague"
      },
      "navigation": {
        "header": [
          {"page": "about-us"},
          {"page": "contact"}
        ],
        "footer": [
          {"page": "shipping-payment"},
          {"page": "returns"}
        ]
      }
    },
    "en-us": {
      "contact": {
        "email": "info@eshop.com",
        "phone": "+1 555 123 4567", 
        "address": "123 Main St, New York"
      },
      "navigation": {
        "header": [
          {"page": "about-us"},
          {"page": "contact"}
        ],
        "footer": [
          {"page": "shipping-payment"},
          {"page": "returns"}
        ]
      }
    }
  }
}
```

**Note:** The `.global` key contains variables shared between all languages (e.g. social media, basic company info). Language-specific keys then contain localized variables.

### 5. RTL Languages

```css
/* templates/assets/css/rtl.css */
.rtl {
  direction: rtl;
  text-align: right;
}

.rtl .product-card {
  margin-left: 0;
  margin-right: 1rem;
}

.rtl .breadcrumb {
  direction: rtl;
}

.rtl .breadcrumb::before {
  transform: scaleX(-1);
}
```

## 🚀 Deployment of Multilingual Shops

### Subdomain Strategy

```nginx
# nginx configuration for subdomains
server {
    server_name cs.shop.com;
    root /var/www/shop/dist/cs-cz;
}

server {
    server_name en.shop.com;  
    root /var/www/shop/dist/en-us;
}
```

### Subfolder Strategy  

```nginx
# nginx for subfolders
server {
    server_name shop.com;
    root /var/www/shop/dist;
    
    location /en/ {
        try_files $uri $uri/ /en/index.html;
    }
    
    location /de/ {
        try_files $uri $uri/ /de/index.html;
    }
}
```

### CDN Configuration

```javascript
// CloudFlare Workers for automatic redirects
export default {
  async fetch(request) {
    const acceptLanguage = request.headers.get('Accept-Language');
    const url = new URL(request.url);
    
    // Language detection from Accept-Language
    if (url.pathname === '/' && acceptLanguage) {
      if (acceptLanguage.includes('de')) {
        return Response.redirect(url.origin + '/de/', 302);
      } else if (acceptLanguage.includes('en')) {
        return Response.redirect(url.origin + '/en/', 302);
      }
    }
    
    return fetch(request);
  }
}
```