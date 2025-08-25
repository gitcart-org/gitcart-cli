# GitCart - Vícejazyčnost

Kompletní příručka pro vytváření vícejazyčných e-shopů s GitCart.

## 🌍 Přehled vícejazyčné podpory

GitCart poskytuje kompletní podporu pro vícejazyčné e-shopy s flexibilními URL strategiemi, automatickým generováním hreflang tagů a efektivní správou obsahu.

### Klíčové vlastnosti

- **Neomezený počet jazyků** - Podpora libovolného množství jazyků
- **Flexibilní URL strategie** - 4 různé způsoby generování URL
- **Automatic hreflang** - SEO optimalizace pro vyhledávače
- **Content isolation** - Každý jazyk má vlastní obsah
- **Language switching** - Automatické přepínání mezi jazyky
- **Locale-aware formatting** - Formátování podle lokálních zvyklostí

## ⚙️ Konfigurace jazyků

### Základní konfigurace v site.json

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

### Parametry jazyka

| Parametr | Typ | Povinný | Popis |
|----------|-----|---------|-------|
| `name` | string | ✅ | Lidsky čitelný název jazyka |
| `default` | boolean | - | Výchozí jazyk (pouze jeden může být true) |
| `short` | string | ✅ | Krátký kód pro URL (cs, en, de) |
| `currency` | string | ✅ | Měna podle ISO 4217 (CZK, EUR, USD) |
| `date_format` | string | - | PHP-style formát data (výchozí: d.m.Y) |
| `number_format` | object | - | Formátování čísel |
| `rtl` | boolean | - | Right-to-left jazyk (výchozí: false) |

### Formátování čísel

```json
{
  "number_format": {
    "decimal_separator": ",",     // Oddělovač desetinných míst
    "thousands_separator": " ",   // Oddělovač tisíců
    "decimals": 2                // Počet desetinných míst
  }
}
```

## 🔗 URL strategie

GitCart podporuje 4 různé URL strategie pro vícejazyčné weby:

### 1. "default_clean" (doporučeno)

Výchozí jazyk bez prefixu, ostatní s krátkým kódem:

```
Česky (výchozí): https://shop.com/produkty/iphone-15/
English:         https://shop.com/en/products/iphone-15/
Deutsch:         https://shop.com/de/produkte/iphone-15/
```

### 2. "always_full"

Všechny jazyky s plným locale kódem:

```
Česky:   https://shop.com/cs-cz/produkty/iphone-15/
English: https://shop.com/en-us/products/iphone-15/
Deutsch: https://shop.com/de-de/produkte/iphone-15/
```

### 3. "always_short"

Všechny jazyky s krátkým kódem:

```
Česky:   https://shop.com/cs/produkty/iphone-15/
English: https://shop.com/en/products/iphone-15/
Deutsch: https://shop.com/de/produkte/iphone-15/
```

### 4. "default_clean_full"

Výchozí bez prefixu, ostatní s plným locale:

```
Česky (výchozí): https://shop.com/produkty/iphone-15/
English:         https://shop.com/en-us/products/iphone-15/
Deutsch:         https://shop.com/de-de/produkte/iphone-15/
```

### url_locale_format

Určuje formát locale kódů v URL:

```json
{
  "url_locale_format": "short"  // cs, en, de
  "url_locale_format": "full"   // cs-cz, en-us, de-de  
}
```

## 📁 Struktura obsahu

### Organizace jazykových verzí

```
project/
├── site.json
├── cs-cz/                      # Český obsah
│   ├── categories/
│   │   └── elektronika/
│   │       ├── .category.md
│   │       ├── .products/
│   │       │   ├── iphone-15.md
│   │       │   └── samsung-galaxy.md
│   │       └── .images/
│   ├── pages/
│   │   ├── kontakt.md
│   │   ├── o-nas.md
│   │   └── doprava-a-platba.md
│   └── blog/
│       ├── .config.md
│       └── 2024/
│           └── jak-vybrat-telefon.md
├── en-us/                      # Anglický obsah
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
└── de-de/                      # Německý obsah
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

### Sdílení obrázků mezi jazyky

```
cs-cz/categories/elektronika/.images/
├── category-banner.jpg          # Sdíleno mezi jazyky
├── iphone-15-front.jpg         
└── iphone-15-back.jpg          

# Anglická verze může použít stejné obrázky
en-us/categories/electronics/.images/
├── category-banner-en.jpg       # Nebo lokalizované verze
├── iphone-15-front.jpg         # Symlink nebo kopie
└── iphone-15-back.jpg
```

## 🗂️ Vytváření vícejazyčného obsahu

### Produkty v různých jazycích

#### Český produkt (cs-cz/categories/elektronika/.products/iphone-15.md)

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
  display: "6,1\" Super Retina XDR"
  processor: "A17 Bionic čip"
  storage: "128GB úložiště"
  camera: "48MP hlavní + 12MP ultra široká"
seo_title: "iPhone 15 128GB Modrý - nejlepší cena"
seo_description: "iPhone 15 s A17 Bionic čipem a USB-C. Doprava zdarma nad 1000 Kč!"
---

# iPhone 15 - Nová éra inovací

iPhone 15 představuje významný pokrok ve světě smartphonů. S revolučním USB-C konektorem, výkonným A17 Bionic čipem a pokročilým kamerovým systémem.

## ⭐ Klíčové vlastnosti

- **A17 Bionic čip** pro neuvěřitelný výkon
- **48MP kamera** s pokročilými funkcemi
- **USB-C konektor** pro univerzální připojení
- **Až 20 hodin** výdrže baterie
```

#### Anglický produkt (en-us/categories/electronics/.products/iphone-15.md)

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

### Kategorie v různých jazycích

#### Česká kategorie (cs-cz/categories/elektronika/.category.md)

```markdown
---
title: Elektronika
description: Nejnovější elektronické zařízení za skvělé ceny
sort_order: 1
featured: true
banner_image: elektronika-banner.jpg
seo_title: "Elektronika - mobily, počítače a více | Náš E-shop"
seo_description: "Široký výběr elektroniky. iPhone, Samsung, notebooky a příslušenství. Doprava zdarma nad 1000 Kč."
---

# Elektronika

Objevte nejnovější elektronické zařízení za nejlepší ceny. Od smartphonů přes notebooky až po chytré hodinky.

## 📱 Co u nás najdete

- **Mobilní telefony** - iPhone, Samsung, Xiaomi
- **Počítače** - Notebooky, PC sestavy, tablety
- **Audio** - Sluchátka, reproduktory, domácí kina
- **Smart zařízení** - Chytré hodinky, fitness náramky
```

#### Anglická kategorie (en-us/categories/electronics/.category.md)

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

### Statické stránky

#### Kontakt česky (cs-cz/pages/kontakt.md)

```markdown
---
title: Kontakt
description: Kontaktní údaje našeho e-shopu
nav_footer: true
nav_order: 1
icon: phone
template: contact
---

# Kontaktujte nás

Máte otázku? Rádi vám pomůžeme!

## 📞 Telefonický kontakt

**Zákaznická podpora:** +420 123 456 789
- Po-Pá: 8:00 - 17:00
- Víkend: Zavřeno

## ✉️ Email

**Obecné dotazy:** info@nas-eshop.cz
**Objednávky:** objednavky@nas-eshop.cz
**Reklamace:** reklamace@nas-eshop.cz

## 🏢 Provozovna

Naše prodejna v centru Prahy:

**Adresa:**
Václavské náměstí 1
110 00 Praha 1

**Otevírací doba:**
- Pondělí - Pátek: 9:00 - 19:00
- Sobota: 9:00 - 15:00
- Neděle: Zavřeno
```

#### Kontakt anglicky (en-us/pages/contact.md)

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

## 🔧 Templates pro vícejazyčnost

### Language switcher komponenta

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

### RTL podpora v base layout

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

### Lokalizované formátování

```liquid
<!-- Formátování ceny podle locale -->
<span class="price">
  {{ product.price | money: current_language.currency, current_language.number_format }}
</span>

<!-- Formátování data -->
<time datetime="{{ post.date | date: '%Y-%m-%d' }}">
  {{ post.date | date: current_language.date_format }}
</time>

<!-- Pluralizace podle jazyka -->
{% case current_language.short %}
  {% when 'cs' %}
    {{ product.stock | pluralize: 'kus', 'kusy', 'kusů' }}
  {% when 'en' %}
    {{ product.stock | pluralize: 'piece', 'pieces' }}
  {% when 'de' %}
    {{ product.stock | pluralize: 'Stück', 'Stück' }}
{% endcase %}
```

## 🌐 SEO pro vícejazyčné stránky

### Automatické hreflang tagy

GitCart automaticky generuje hreflang tagy pro všechny jazyky:

```html
<!-- Pro českou verze /produkty/iphone-15/ -->
<link rel="alternate" hreflang="cs-cz" href="https://shop.com/produkty/iphone-15/">
<link rel="alternate" hreflang="en-us" href="https://shop.com/en/products/iphone-15/">
<link rel="alternate" hreflang="de-de" href="https://shop.com/de/produkte/iphone-15/">
<link rel="alternate" hreflang="x-default" href="https://shop.com/produkty/iphone-15/">
```

### Sitemap s vícejazyčností

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xhtml="http://www.w3.org/1999/xhtml">
  
  <!-- Český produkt -->
  <url>
    <loc>https://shop.com/produkty/iphone-15/</loc>
    <lastmod>2024-01-15</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
    
    <xhtml:link rel="alternate" hreflang="cs-cz" 
                href="https://shop.com/produkty/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="en-us" 
                href="https://shop.com/en/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="de-de" 
                href="https://shop.com/de/produkte/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="x-default" 
                href="https://shop.com/produkty/iphone-15/"/>
  </url>
  
  <!-- Anglický produkt -->
  <url>
    <loc>https://shop.com/en/products/iphone-15/</loc>
    <lastmod>2024-01-15</lastmod>
    
    <xhtml:link rel="alternate" hreflang="cs-cz" 
                href="https://shop.com/produkty/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="en-us" 
                href="https://shop.com/en/products/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="de-de" 
                href="https://shop.com/de/produkte/iphone-15/"/>
    <xhtml:link rel="alternate" hreflang="x-default" 
                href="https://shop.com/produkty/iphone-15/"/>
  </url>
  
</urlset>
```

### Structured data pro vícejazyčné produkty

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

## 🛠️ CLI pro vícejazyčný obsah

### Vytváření obsahu ve více jazycích

```bash
# Vytvoření produktu ve všech jazycích
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

# Vytvoření kategorie ve více jazycích
gitcart category add --multilingual
? Category name (cs-cz): Elektronika
? Category name (en-us): Electronics
? Category name (de-de): Elektronik
? URL slug (cs-cz): elektronika
? URL slug (en-us): electronics  
? URL slug (de-de): elektronik

# Vytvoření stránky ve více jazycích
gitcart page add --multilingual
? Page name (cs-cz): Kontakt
? Page name (en-us): Contact
? Page name (de-de): Kontakt
```

### Synchronizace obsahu

```bash
# Kontrola chybějících překladů
gitcart multilingual check
❌ Missing translations found:
  Product 'samsung-galaxy' missing in: en-us, de-de
  Page 'doprava-a-platba' missing in: en-us
  Category 'elektronika' missing translation in: de-de

# Vytvoření šablon pro chybějící překlady
gitcart multilingual scaffold --missing
✅ Created translation templates:
  en-us/categories/electronics/.products/samsung-galaxy.md
  en-us/pages/shipping-payment.md
  de-de/categories/elektronik/.category.md
```

### Bulk operace

```bash
# Hromadné aktualizace cen podle měny
gitcart multilingual update-prices --exchange-rates api
✅ Updated prices:
  cs-cz: 25999 CZK
  en-us: 999 USD (rate: 26.0)
  de-de: 899 EUR (rate: 28.9)

# Hromadná kontrola SEO metadat
gitcart multilingual seo-check
⚠️ SEO issues found:
  cs-cz/produkty/iphone-15: Missing meta description
  en-us/products/iphone-15: Title too short (< 30 chars)
```

## 🎯 Nejlepší postupy

### 1. Konzistentní SKU napříč jazyky

```markdown
<!-- cs-cz -->
---
sku: IPHONE15-128-BLUE
title: iPhone 15 128GB Modrý
price: 25999
currency: CZK
---

<!-- en-us -->
---
sku: IPHONE15-128-BLUE  # Stejné SKU
title: iPhone 15 128GB Blue
price: 999
currency: USD
---
```

### 2. Lokalizace cen a měn

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

### 3. Sdílení obrázků

```bash
# Hardlinks pro sdílení obrázků
ln cs-cz/categories/elektronika/.images/iphone-15.jpg \
   en-us/categories/electronics/.images/iphone-15.jpg
   
# Nebo symbolické linky
ln -s ../../cs-cz/categories/elektronika/.images/iphone-15.jpg \
      en-us/categories/electronics/.images/iphone-15.jpg
```

### 4. Variables pro vícejazyčnost

Pro vícejazyčné projekty můžete definovat variables per-language:

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
        "address": "Václavské náměstí 1, Praha"
      },
      "navigation": {
        "header": [
          {"page": "o-nas"},
          {"page": "kontakt"}
        ],
        "footer": [
          {"page": "doprava-a-platba"},
          {"page": "reklamace"}
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

**Poznámka:** Klíč `.global` obsahuje proměnné sdílené mezi všemi jazyky (např. sociální sítě, základní info o firmě). Language-specific klíče pak obsahují lokalizované proměnné.

### 5. RTL jazyky

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

## 🚀 Deployment vícejazyčných shopů

### Subdomain strategie

```nginx
# nginx konfigurace pro subdomény
server {
    server_name cs.shop.com;
    root /var/www/shop/dist/cs-cz;
}

server {
    server_name en.shop.com;  
    root /var/www/shop/dist/en-us;
}
```

### Subfolder strategie  

```nginx
# nginx pro subfolder
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

### CDN konfigurace

```javascript
// CloudFlare Workers pro automatické přesměrování
export default {
  async fetch(request) {
    const acceptLanguage = request.headers.get('Accept-Language');
    const url = new URL(request.url);
    
    // Detekce jazyka z Accept-Language
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