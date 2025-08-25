# GitCart - SEO optimalizace

Kompletní příručka pro SEO optimalizaci GitCart e-shopů.

## 🎯 SEO vlastnosti GitCart

GitCart je navržen s důrazem na SEO od základu:

- **Statické HTML soubory** - Rychlé načítání, snadná indexace
- **Automatické sitemapy** - XML sitemap s hreflang podporou
- **Structured data** - JSON-LD schema pro produkty a organizaci
- **Meta tagy** - OpenGraph, Twitter Cards, canonical URLs
- **Clean URLs** - SEO-friendly URL struktura
- **Performance** - Optimalizované CSS/JS, lazy loading
- **Mobile-first** - Responzivní design, Core Web Vitals

## 🗺️ Sitemap generování

### Automatické XML sitemap

GitCart automaticky generuje kompletní sitemap včetně:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:image="http://www.google.com/schemas/sitemap-image/1.1"
        xmlns:xhtml="http://www.w3.org/1999/xhtml">
        
  <!-- Homepage -->
  <url>
    <loc>https://myshop.com/</loc>
    <lastmod>2024-01-15T10:30:00Z</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  
  <!-- Category pages -->
  <url>
    <loc>https://myshop.com/elektronika/</loc>
    <lastmod>2024-01-15T08:00:00Z</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
    
    <!-- Image sitemap -->
    <image:image>
      <image:loc>https://myshop.com/assets/images/elektronika-banner.jpg</image:loc>
      <image:title>Elektronika - nejnovější zařízení</image:title>
    </image:image>
  </url>
  
  <!-- Product pages -->
  <url>
    <loc>https://myshop.com/elektronika/iphone-15/</loc>
    <lastmod>2024-01-15T07:45:00Z</lastmod>
    <changefreq>daily</changefreq>
    <priority>0.9</priority>
    
    <!-- Product images -->
    <image:image>
      <image:loc>https://myshop.com/assets/images/iphone-15-front.jpg</image:loc>
      <image:title>iPhone 15 - přední strana</image:title>
    </image:image>
    <image:image>
      <image:loc>https://myshop.com/assets/images/iphone-15-back.jpg</image:loc>
      <image:title>iPhone 15 - zadní strana</image:title>
    </image:image>
  </url>
  
  <!-- Static pages -->
  <url>
    <loc>https://myshop.com/kontakt/</loc>
    <lastmod>2024-01-10T12:00:00Z</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.5</priority>
  </url>
  
  <!-- Blog posts -->
  <url>
    <loc>https://myshop.com/blog/jak-vybrat-telefon/</loc>
    <lastmod>2024-01-14T15:30:00Z</lastmod>
    <changefreq>never</changefreq>
    <priority>0.7</priority>
  </url>
  
</urlset>
```

### Vícejazyčné sitemap s hreflang

```xml
<!-- Pro vícejazyčné weby -->
<url>
  <loc>https://myshop.com/elektronika/iphone-15/</loc>
  <lastmod>2024-01-15T07:45:00Z</lastmod>
  
  <!-- Hreflang alternativy -->
  <xhtml:link rel="alternate" hreflang="cs-cz" 
              href="https://myshop.com/elektronika/iphone-15/"/>
  <xhtml:link rel="alternate" hreflang="en-us" 
              href="https://myshop.com/en/electronics/iphone-15/"/>
  <xhtml:link rel="alternate" hreflang="de-de" 
              href="https://myshop.com/de/elektronik/iphone-15/"/>
  <xhtml:link rel="alternate" hreflang="x-default" 
              href="https://myshop.com/elektronika/iphone-15/"/>
</url>
```

### Robots.txt

```txt
# Automaticky generovaný robots.txt
User-agent: *
Allow: /

# Sitemap
Sitemap: https://myshop.com/sitemap.xml

# Blokování admin a dev souborů
Disallow: /admin/
Disallow: /*.json$
Disallow: /dev/
Disallow: /test/

# Povolení crawlování assestů
Allow: /assets/css/
Allow: /assets/js/
Allow: /assets/images/

# Crawl delay pro velké e-shopy
Crawl-delay: 1
```

## 🏷️ Meta tagy a structured data

### Automatické meta tagy

GitCart automaticky generuje kompletní meta tagy:

```html
<!-- Pro produkt -->
<head>
  <!-- Basic meta -->
  <title>iPhone 15 128GB Modrý - nejlepší cena | Můj E-shop</title>
  <meta name="description" content="iPhone 15 s A17 Bionic čipem a USB-C konektorem. Doprava zdarma nad 1000 Kč. Skladem 15 ks.">
  <link rel="canonical" href="https://myshop.com/elektronika/iphone-15/">
  
  <!-- OpenGraph -->
  <meta property="og:type" content="product">
  <meta property="og:title" content="iPhone 15 128GB Modrý">
  <meta property="og:description" content="iPhone 15 s A17 Bionic čipem a USB-C konektorem za skvělou cenu.">
  <meta property="og:url" content="https://myshop.com/elektronika/iphone-15/">
  <meta property="og:image" content="https://myshop.com/assets/images/iphone-15-front.jpg">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:site_name" content="Můj E-shop">
  <meta property="og:locale" content="cs_CZ">
  
  <!-- Product specific OpenGraph -->
  <meta property="product:price:amount" content="25999">
  <meta property="product:price:currency" content="CZK">
  <meta property="product:availability" content="in stock">
  <meta property="product:condition" content="new">
  <meta property="product:retailer_item_id" content="IPHONE15-128-BLUE">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="iPhone 15 128GB Modrý">
  <meta name="twitter:description" content="iPhone 15 s A17 Bionic čipem a USB-C konektorem za skvělou cenu.">
  <meta name="twitter:image" content="https://myshop.com/assets/images/iphone-15-front.jpg">
  <meta name="twitter:site" content="@mujeshop">
  
  <!-- Mobile -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  
  <!-- Performance hints -->
  <link rel="dns-prefetch" href="//fonts.googleapis.com">
  <link rel="preconnect" href="https://images.myshop.com">
  <link rel="preload" href="/assets/css/styles.css" as="style">
</head>
```

### JSON-LD Structured Data

#### Product schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "iPhone 15 128GB Modrý",
  "description": "iPhone 15 představuje významný pokrok ve světě smartphonů. S revolučním USB-C konektorem, výkonným A17 Bionic čipem a pokročilým kamerovým systémem.",
  "sku": "IPHONE15-128-BLUE",
  "mpn": "IPHONE15-128-BLUE",
  "brand": {
    "@type": "Brand", 
    "name": "Apple"
  },
  "category": "Elektronika > Mobilní telefony > Smartphony",
  "image": [
    "https://myshop.com/assets/images/iphone-15-front.jpg",
    "https://myshop.com/assets/images/iphone-15-back.jpg",
    "https://myshop.com/assets/images/iphone-15-gallery-1.jpg"
  ],
  "offers": {
    "@type": "Offer",
    "url": "https://myshop.com/elektronika/iphone-15/",
    "priceCurrency": "CZK",
    "price": "23999",
    "priceValidUntil": "2024-12-31",
    "availability": "https://schema.org/InStock",
    "itemCondition": "https://schema.org/NewCondition",
    "seller": {
      "@type": "Organization",
      "name": "Můj E-shop"
    },
    "shippingDetails": {
      "@type": "OfferShippingDetails",
      "shippingRate": {
        "@type": "MonetaryAmount",
        "currency": "CZK",
        "value": "99"
      },
      "deliveryTime": {
        "@type": "ShippingDeliveryTime",
        "businessDays": {
          "@type": "OpeningHoursSpecification",
          "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]
        },
        "cutoffTime": "16:00",
        "handlingTime": {
          "@type": "QuantitativeValue",
          "minValue": 1,
          "maxValue": 2,
          "unitCode": "DAY"
        },
        "transitTime": {
          "@type": "QuantitativeValue", 
          "minValue": 1,
          "maxValue": 3,
          "unitCode": "DAY"
        }
      }
    }
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.5",
    "reviewCount": "127",
    "bestRating": "5",
    "worstRating": "1"
  },
  "review": [
    {
      "@type": "Review",
      "author": {
        "@type": "Person",
        "name": "Jan Novák"
      },
      "reviewRating": {
        "@type": "Rating",
        "ratingValue": "5",
        "bestRating": "5"
      },
      "reviewBody": "Skvělý telefon, rychlá doprava, doporučuji!"
    }
  ]
}
</script>
```

#### Organization schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Můj E-shop",
  "url": "https://myshop.com",
  "logo": "https://myshop.com/assets/images/logo.png",
  "description": "Kvalitní elektronika za skvělé ceny. Rychlá doprava, 2 roky záruky.",
  
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+420-123-456-789",
    "contactType": "customer service",
    "availableLanguage": ["Czech", "English"],
    "areaServed": "CZ"
  },
  
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Václavské náměstí 1",
    "addressLocality": "Praha",
    "postalCode": "110 00",
    "addressCountry": "CZ"
  },
  
  "sameAs": [
    "https://facebook.com/mujeshop",
    "https://instagram.com/mujeshop",
    "https://twitter.com/mujeshop"
  ],
  
  "openingHours": [
    "Mo-Fr 09:00-18:00",
    "Sa 09:00-15:00"
  ]
}
</script>
```

#### Breadcrumb schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Domů",
      "item": "https://myshop.com/"
    },
    {
      "@type": "ListItem", 
      "position": 2,
      "name": "Elektronika",
      "item": "https://myshop.com/elektronika/"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "iPhone 15 128GB Modrý",
      "item": "https://myshop.com/elektronika/iphone-15/"
    }
  ]
}
</script>
```

## 🚀 Performance optimalizace

### Core Web Vitals

GitCart optimalizuje pro Google Core Web Vitals:

#### Largest Contentful Paint (LCP)

```html
<!-- Preload critical resources -->
<link rel="preload" href="/assets/css/styles.css" as="style">
<link rel="preload" href="/assets/fonts/inter-var.woff2" as="font" type="font/woff2" crossorigin>

<!-- Critical CSS inline -->
<style>
  /* Above-the-fold kritické styly */
  .header { 
    background: #fff;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  .hero {
    min-height: 400px;
    display: flex;
    align-items: center;
  }
</style>

<!-- Async načtení nekritického CSS -->
<link rel="preload" href="/assets/css/styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

#### First Input Delay (FID)

```html
<!-- Lazy loading JavaScriptu -->
<script defer src="/assets/js/gitcart.js"></script>

<!-- Code splitting -->
<script>
  // Load cart functionality only when needed
  function loadCartModule() {
    return import('/assets/js/modules/cart.js');
  }
  
  // Load search only on interaction
  document.querySelector('.search-toggle').addEventListener('click', () => {
    import('/assets/js/modules/search.js');
  });
</script>
```

#### Cumulative Layout Shift (CLS)

```html
<!-- Rezervace místa pro obrázky -->
<img src="product.jpg" 
     width="400" 
     height="300"
     style="aspect-ratio: 4/3;"
     loading="lazy"
     alt="Produkt">

<!-- Stabilní layout pro ads -->
<div class="ad-slot" style="min-height: 250px;">
  <!-- Reklama se načte asynchronně -->
</div>
```

### Image optimalizace

```html
<!-- Responsive images -->
<picture>
  <source media="(min-width: 768px)" 
          srcset="product-large.webp 1200w, product-medium.webp 800w" 
          type="image/webp">
  <source media="(min-width: 768px)" 
          srcset="product-large.jpg 1200w, product-medium.jpg 800w">
  <source srcset="product-small.webp 400w" type="image/webp">
  <img src="product-small.jpg" 
       alt="iPhone 15" 
       loading="lazy"
       width="400" 
       height="300">
</picture>

<!-- Lazy loading s intersection observer -->
<img class="lazy-image" 
     data-src="product-large.jpg"
     data-srcset="product-large.jpg 1200w, product-medium.jpg 800w"
     src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 400 300'%3E%3C/svg%3E"
     alt="iPhone 15">
```

### CSS optimalizace

```css
/* Critical CSS - inline v <head> */
.header, .navigation, .hero {
  /* Above-the-fold styles */
}

/* Non-critical CSS - async loaded */
@media print {
  /* Print styles */
}

.modal, .dropdown, .tooltip {
  /* Interactive elements */
}

/* Font loading optimalization */
@font-face {
  font-family: 'Inter';
  src: url('/assets/fonts/inter-var.woff2') format('woff2-variations');
  font-display: swap;
  font-weight: 100 900;
}
```

## 🔍 Technical SEO

### URL struktura

GitCart vytváří SEO-friendly URL:

```
✅ Dobré URL:
https://myshop.com/elektronika/
https://myshop.com/elektronika/iphone-15/
https://myshop.com/blog/jak-vybrat-telefon/
https://myshop.com/kontakt/

❌ Špatné URL:
https://myshop.com/category.php?id=123
https://myshop.com/product.php?sku=IPHONE15
https://myshop.com/blog/post/123456
```

### Canonical URLs

```html
<!-- Jednoznačné canonical URL -->
<link rel="canonical" href="https://myshop.com/elektronika/iphone-15/">

<!-- Pro paginované stránky -->
<link rel="canonical" href="https://myshop.com/elektronika/">
<link rel="prev" href="https://myshop.com/elektronika/">
<link rel="next" href="https://myshop.com/elektronika/2/">

<!-- Pro filtered stránky -->
<link rel="canonical" href="https://myshop.com/elektronika/">
```

### HTTP headers

```http
# Performance headers
Cache-Control: public, max-age=31536000, immutable
Content-Encoding: gzip, br
Vary: Accept-Encoding

# Security headers  
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin

# SEO headers
Link: <https://myshop.com/elektronika/iphone-15/>; rel="canonical"
```

## 📊 SEO monitoring a analytics

### Google Search Console integrace

```html
<!-- Search Console verification -->
<meta name="google-site-verification" content="your-verification-code">

<!-- Enhanced ecommerce events -->
<script>
gtag('event', 'page_view', {
  page_title: 'iPhone 15 128GB Modrý',
  page_location: 'https://myshop.com/elektronika/iphone-15/',
  content_group1: 'Elektronika',
  content_group2: 'Produkty'
});

gtag('event', 'view_item', {
  currency: 'CZK',
  value: 23999,
  items: [{
    item_id: 'IPHONE15-128-BLUE',
    item_name: 'iPhone 15 128GB Modrý',
    category: 'Elektronika',
    category2: 'Mobilní telefony',
    price: 23999,
    quantity: 1
  }]
});
</script>
```

### SEO audit nástroje

```bash
# Lighthouse audit
npx lighthouse https://myshop.com --chrome-flags="--headless"

# PageSpeed Insights API
curl "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=https://myshop.com&key=API_KEY"

# Core Web Vitals monitoring
npm install web-vitals
```

### automatické SEO testy

```javascript
// __tests__/seo/meta-tags.test.js
describe('SEO Meta Tags', () => {
  test('should have title under 60 characters', () => {
    const title = page.title;
    expect(title.length).toBeLessThanOrEqual(60);
  });

  test('should have meta description between 150-160 chars', () => {
    const description = page.metaDescription;
    expect(description.length).toBeGreaterThanOrEqual(150);
    expect(description.length).toBeLessThanOrEqual(160);
  });

  test('should have canonical URL', () => {
    const canonical = page.canonicalUrl;
    expect(canonical).toMatch(/^https:\/\/[^\/]+.*[^\/]$/);
  });
});

// __tests__/seo/structured-data.test.js
describe('Structured Data', () => {
  test('product should have valid JSON-LD', () => {
    const jsonLd = page.getJsonLd('Product');
    expect(jsonLd).toHaveProperty('@type', 'Product');
    expect(jsonLd).toHaveProperty('name');
    expect(jsonLd).toHaveProperty('offers');
    expect(jsonLd.offers).toHaveProperty('price');
  });
});
```

## 🎯 E-commerce SEO strategie

### Category SEO

```markdown
<!-- cs-cz/categories/elektronika/.category.md -->
---
title: Elektronika
seo_title: "Elektronika - mobily, počítače, audio | Můj E-shop"
seo_description: "Široký výběr elektroniky za skvělé ceny. iPhone, Samsung, notebooky, tablety a příslušenství. Doprava zdarma nad 1000 Kč. ⭐ 4.8/5 od zákazníků."
featured: true
sort_order: 1
---

# Elektronika - nejnovější technologie za skvělé ceny

Objevte **nejširší výběr elektroniky** v České republice. Od nejnovějších **iPhonů a Samsung** telefonu až po **výkonné notebooky** a **chytré hodinky**.

## 🏆 Proč nakupovat elektroniku u nás?

- ✅ **Nejlepší ceny** - garantujeme nejnižší cenu na trhu
- ✅ **Okamžité skladové zásoby** - 95% produktů skladem
- ✅ **Rychlá doprava** - doručení do 24 hodin po ČR
- ✅ **2 roky záruky** - na všechny produkty
- ✅ **Odborné poradenství** - pomůžeme s výběrem

## 📱 Nejoblíbenější kategorie

### Mobilní telefony
Nejnovější **iPhone 15**, **Samsung Galaxy S24** a další top smartphony s **5G připojením**.

### Notebooky a počítače  
**Gaming notebooky**, **ultrabook pro práci** a **All-in-One PC** pro domácnost.

### Audio technika
**Bezdrátová sluchátka**, **soundbary** a **Hi-Fi systémy** pro dokonalý zvukový zážitek.
```

### Product SEO

```markdown
<!-- cs-cz/categories/elektronika/.products/iphone-15.md -->
---
title: iPhone 15 128GB
seo_title: "iPhone 15 128GB - nejlepší cena 23.999 Kč | Můj E-shop"
seo_description: "iPhone 15 128GB s A17 Bionic čipem, USB-C a 48MP kamerou. ⚡ Skladem 15 ks, doprava zdarma. ⭐ 4.9/5 od zákazníků. Záruka 2 roky."
price: 25999
sale_price: 23999
sku: IPHONE15-128-BLUE
stock: 15
---

# iPhone 15 128GB - revoluce s USB-C konektorem

**iPhone 15** představuje **novou éru smartphonů** s revolučním **USB-C konektorem**, výkonným **A17 Bionic čipem** a pokročilým **48MP kamerovým systémem**.

## ⚡ Klíčové výhody iPhone 15

- **A17 Bionic čip** - nejrychlejší čip v historii iPhone
- **USB-C konektor** - univerzální nabíjení a připojení
- **48MP hlavní kamera** - profesionální fotografie a 4K video
- **Až 20 hodin výdrže** - celodenní použití bez starostí
- **Akční display** - 6.1" Super Retina XDR s True Tone

## 💰 Nejlepší cena na trhu - ušetříte 2.000 Kč

**Původní cena:** ~~25.999 Kč~~
**Naše cena:** **23.999 Kč** 
**Ušetříte:** 2.000 Kč (7,7%)

### 🚚 Doprava a platba
- **Doprava zdarma** při nákupu nad 1.000 Kč
- **Doručení do 24 hodin** po celé ČR
- **Osobní odběr** v Praze zdarma
- **Platba kartou, převodem** nebo na dobírku

## 🔍 Technické specifikace

| Vlastnost | Hodnota |
|-----------|---------|
| **Displej** | 6.1" Super Retina XDR OLED |
| **Procesor** | A17 Bionic (3 nm) |
| **Paměť** | 128GB / 6GB RAM |
| **Kamera** | 48MP + 12MP Ultra Wide |
| **Baterie** | Až 20 hodin videa |
| **Konektor** | USB-C |

## ⭐ Hodnocení zákazníků (4.9/5)

> "Nejlepší telefon, který jsem kdy měl. Rychlá doprava, perfektní balení." 
> **- Jan N., Praha**

> "Konečně USB-C! Skvělá kamera a baterie vydrží celý den."
> **- Marie K., Brno**
```

### Blog SEO

```markdown
<!-- cs-cz/blog/2024/jak-vybrat-telefon.md -->
---
title: Jak vybrat telefon v roce 2024
seo_title: "Jak vybrat smartphone v roce 2024 - kompletní průvodce | Blog"
seo_description: "Nevíte jak vybrat telefon? ✅ Kompletní průvodce výběrem smartphonu 2024. Procesor, kamera, baterie, cena. ⭐ Odborné rady zdarma."
date: 2024-01-14
category: tipy
author: Jan Novák
---

# Jak vybrat perfektní smartphone v roce 2024 📱

Výběr nového **telefonu** může být složitý. Na trhu je stovky modelů a není jednoduché se zorientovat. V tomto **kompletním průvodci** vám ukážeme, **na co se zaměřit** při výběru nového smartphonu.

## 🎯 Na co se zaměřit při výběru telefonu?

### 1. Operační systém - Android vs iOS

**Android telefony:**
- ✅ Větší výběr modelů a cen  
- ✅ Více možností přizpůsobení
- ✅ Lepší integrace s Google službami

**iPhone (iOS):**
- ✅ Dlouhodobé aktualizace
- ✅ Lepší optimalizace a výkon
- ✅ Prémiové zpracování

### 2. Procesor - srdce vašeho telefonu

**Nejlepší procesory 2024:**
- **Apple A17 Bionic** (iPhone 15) - nejrychlejší
- **Snapdragon 8 Gen 3** (Samsung Galaxy S24) 
- **Google Tensor G3** (Pixel 8)
- **MediaTek Dimensity 9300** - dobrý poměr cena/výkon

### 3. Kamera - pro dokonalé fotografie

**Na co se zaměřit:**
- **Megapixely** nejsou všechno - důležitá je velikost senzoru
- **Optická stabilizace** - ostré fotky i za pohybu  
- **Noční režim** - fotografie i ve tmě
- **Ultra široká kamera** - pro skupinové fotky a krajiny

**Nejlepší kamery 2024:**
1. **iPhone 15 Pro** - 48MP s LiDAR
2. **Samsung Galaxy S24 Ultra** - 200MP zoom
3. **Google Pixel 8 Pro** - AI fotografie

## 📊 Srovnání TOP 5 telefonů 2024

| Model | Cena | Procesor | Kamera | Baterie |
|-------|------|----------|---------|---------|
| iPhone 15 | 23.999 Kč | A17 Bionic | 48MP | 20h |
| Galaxy S24 | 21.999 Kč | Snapdragon 8 Gen 3 | 50MP | 22h |
| Pixel 8 | 18.999 Kč | Tensor G3 | 50MP | 18h |
| OnePlus 12 | 19.999 Kč | Snapdragon 8 Gen 3 | 50MP | 24h |
| Xiaomi 14 | 16.999 Kč | Snapdragon 8 Gen 3 | 50MP | 20h |

## 💡 Naše doporučení podle rozpočtu

### Do 15.000 Kč - nejlepší poměr cena/výkon
- **Xiaomi Redmi Note 13 Pro** - 12.999 Kč
- **Samsung Galaxy A54** - 13.999 Kč
- **Realme GT Neo 5** - 14.999 Kč

### 15.000 - 25.000 Kč - střední třída
- **OnePlus Nord 3** - 16.999 Kč  
- **Google Pixel 7a** - 17.999 Kč
- **iPhone 13** - 19.999 Kč

### Nad 25.000 Kč - prémiové modely
- **iPhone 15 Pro** - 29.999 Kč
- **Samsung Galaxy S24 Ultra** - 34.999 Kč
- **Google Pixel 8 Pro** - 26.999 Kč

## ❓ Často kladené otázky

### Kolik paměti potřebuji?
- **64GB** - pouze pro základní použití
- **128GB** - optimální pro většinu uživatelů  
- **256GB+** - pro power uživatele a fotografie

### Je lepší Android nebo iPhone?
Záleží na vašich potřebách. **iPhone** je lepší pro jednoduchost a dlouhodobé aktualizace. **Android** nabízí více možností a lepší poměr cena/výkon.

## 🎯 Závěr - jak vybrat správně

Při výběru telefonu si **definujte rozpočet** a **klíčové funkce**. Pokud fotíte často, investujte do **kvalitní kamery**. Pokud používáte telefon celodenně, zvolte model s **velkou baterií**.

**Potřebujete poradit s výběrem?** Navštivte naši prodejnu nebo nás kontaktujte na **+420 123 456 789**. Naši odborníci vám pomohou vybrat **ideální telefon** pro vaše potřeby.

---

*Tento článek byl aktualizován 14. ledna 2024. Ceny jsou orientační a mohou se lišit.*
```

## 📈 SEO metriky a KPI

### Klíčové metriky

1. **Organic traffic** - návštěvnost z vyhledávačů
2. **Keyword rankings** - pozice pro cílové klíčová slova
3. **Click-through rate (CTR)** - míra prokliku ze SERPů
4. **Conversion rate** - konverze z organic trafficu
5. **Page load speed** - rychlost načítání stránek
6. **Core Web Vitals** - LCP, FID, CLS
7. **Mobile usability** - použitelnost na mobilu

### SEO reporting

```bash
# Automatické SEO audity
gitcart seo audit
✅ SEO Audit Results:
  - 45 pages analyzed
  - 42 pages have proper titles (93%)
  - 38 pages have meta descriptions (84%)  
  - All images have alt tags
  - Sitemap is valid
  - No broken links found

⚠️ Issues found:
  - 3 pages missing meta descriptions
  - 2 titles over 60 characters
  - Category "elektronika" needs more internal links

# Performance audit
gitcart performance audit
⚡ Performance Results:
  - Average page load: 1.2s
  - Core Web Vitals: ✅ GOOD
  - Mobile-friendly: ✅ YES
  - HTTPS: ✅ YES
```

GitCart poskytuje kompletní SEO základ pro úspěšný e-shop. Kombinace statických souborů, optimalizovaného kódu a správné struktury zajišťuje výborné SEO výsledky od prvního dne.