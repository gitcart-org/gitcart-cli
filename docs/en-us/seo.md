# GitCart - SEO Optimization

Complete guide for SEO optimization of GitCart e-shops.

## 🎯 GitCart SEO Features

GitCart is designed with SEO in mind from the ground up:

- **Static HTML files** - Fast loading, easy indexing
- **Automatic sitemaps** - XML sitemap with hreflang support
- **Structured data** - JSON-LD schema for products and organization
- **Meta tags** - OpenGraph, Twitter Cards, canonical URLs
- **Clean URLs** - SEO-friendly URL structure
- **Performance** - Optimized CSS/JS, lazy loading
- **Mobile-first** - Responsive design, Core Web Vitals

## 🗺️ Sitemap Generation

### Automatic XML Sitemap

GitCart automatically generates complete sitemap including:

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
    <loc>https://myshop.com/electronics/</loc>
    <lastmod>2024-01-15T08:00:00Z</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
    
    <!-- Image sitemap -->
    <image:image>
      <image:loc>https://myshop.com/assets/images/electronics-banner.jpg</image:loc>
      <image:title>Electronics - Latest devices</image:title>
    </image:image>
  </url>
  
  <!-- Product pages -->
  <url>
    <loc>https://myshop.com/electronics/iphone-15/</loc>
    <lastmod>2024-01-15T07:45:00Z</lastmod>
    <changefreq>daily</changefreq>
    <priority>0.9</priority>
    
    <!-- Product images -->
    <image:image>
      <image:loc>https://myshop.com/assets/images/iphone-15-front.jpg</image:loc>
      <image:title>iPhone 15 - Front view</image:title>
    </image:image>
    <image:image>
      <image:loc>https://myshop.com/assets/images/iphone-15-back.jpg</image:loc>
      <image:title>iPhone 15 - Back view</image:title>
    </image:image>
  </url>
  
  <!-- Static pages -->
  <url>
    <loc>https://myshop.com/contact/</loc>
    <lastmod>2024-01-10T12:00:00Z</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.5</priority>
  </url>
  
  <!-- Blog posts -->
  <url>
    <loc>https://myshop.com/blog/how-to-choose-phone/</loc>
    <lastmod>2024-01-14T15:30:00Z</lastmod>
    <changefreq>never</changefreq>
    <priority>0.7</priority>
  </url>
  
</urlset>
```

### Multilingual Sitemap with Hreflang

```xml
<!-- For multilingual websites -->
<url>
  <loc>https://myshop.com/electronics/iphone-15/</loc>
  <lastmod>2024-01-15T07:45:00Z</lastmod>
  
  <!-- Hreflang alternatives -->
  <xhtml:link rel="alternate" hreflang="cs-cz" 
              href="https://myshop.com/cs/electronics/iphone-15/"/>
  <xhtml:link rel="alternate" hreflang="en-us" 
              href="https://myshop.com/electronics/iphone-15/"/>
  <xhtml:link rel="alternate" hreflang="de-de" 
              href="https://myshop.com/de/electronics/iphone-15/"/>
  <xhtml:link rel="alternate" hreflang="x-default" 
              href="https://myshop.com/electronics/iphone-15/"/>
</url>
```

### Robots.txt

```txt
# Automatically generated robots.txt
User-agent: *
Allow: /

# Sitemap
Sitemap: https://myshop.com/sitemap.xml

# Block admin and dev files
Disallow: /admin/
Disallow: /*.json$
Disallow: /dev/
Disallow: /test/

# Allow crawling assets
Allow: /assets/css/
Allow: /assets/js/
Allow: /assets/images/

# Crawl delay for large e-shops
Crawl-delay: 1
```

## 🏷️ Meta Tags and Structured Data

### Automatic Meta Tags

GitCart automatically generates complete meta tags:

```html
<!-- For product -->
<head>
  <!-- Basic meta -->
  <title>iPhone 15 128GB Blue - Best Price | My E-shop</title>
  <meta name="description" content="iPhone 15 with A17 Bionic chip and USB-C connector. Free shipping over $50. In stock - 15 units.">
  <link rel="canonical" href="https://myshop.com/electronics/iphone-15/">
  
  <!-- OpenGraph -->
  <meta property="og:type" content="product">
  <meta property="og:title" content="iPhone 15 128GB Blue">
  <meta property="og:description" content="iPhone 15 with A17 Bionic chip and USB-C connector at a great price.">
  <meta property="og:url" content="https://myshop.com/electronics/iphone-15/">
  <meta property="og:image" content="https://myshop.com/assets/images/iphone-15-front.jpg">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:site_name" content="My E-shop">
  <meta property="og:locale" content="en_US">
  
  <!-- Product specific OpenGraph -->
  <meta property="product:price:amount" content="999">
  <meta property="product:price:currency" content="USD">
  <meta property="product:availability" content="in stock">
  <meta property="product:condition" content="new">
  <meta property="product:retailer_item_id" content="IPHONE15-128-BLUE">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="iPhone 15 128GB Blue">
  <meta name="twitter:description" content="iPhone 15 with A17 Bionic chip and USB-C connector at a great price.">
  <meta name="twitter:image" content="https://myshop.com/assets/images/iphone-15-front.jpg">
  <meta name="twitter:site" content="@myshop">
  
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

#### Product Schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "iPhone 15 128GB Blue",
  "description": "The iPhone 15 represents a major leap forward in smartphone technology. With revolutionary USB-C connector, powerful A17 Bionic chip, and advanced camera system.",
  "sku": "IPHONE15-128-BLUE",
  "mpn": "IPHONE15-128-BLUE",
  "brand": {
    "@type": "Brand", 
    "name": "Apple"
  },
  "category": "Electronics > Mobile Phones",
  "image": [
    "https://myshop.com/assets/images/iphone-15-front.jpg",
    "https://myshop.com/assets/images/iphone-15-back.jpg",
    "https://myshop.com/assets/images/iphone-15-gallery-1.jpg"
  ],
  "offers": {
    "@type": "Offer",
    "url": "https://myshop.com/electronics/iphone-15/",
    "priceCurrency": "USD",
    "price": "899",
    "priceValidUntil": "2024-12-31",
    "availability": "https://schema.org/InStock",
    "itemCondition": "https://schema.org/NewCondition",
    "seller": {
      "@type": "Organization",
      "name": "My E-shop"
    },
    "hasMerchantReturnPolicy": {
      "@type": "MerchantReturnPolicy",
      "returnPolicyCategory": "https://schema.org/MerchantReturnFiniteReturnWindow",
      "merchantReturnDays": 30
    },
    "shippingDetails": {
      "@type": "OfferShippingDetails",
      "shippingRate": {
        "@type": "MonetaryAmount",
        "value": "0",
        "currency": "USD"
      },
      "deliveryTime": {
        "@type": "ShippingDeliveryTime",
        "businessDays": {
          "@type": "OpeningHoursSpecification",
          "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]
        },
        "cutoffTime": "15:00",
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
    "ratingValue": "4.8",
    "reviewCount": "127"
  },
  "review": [
    {
      "@type": "Review",
      "reviewRating": {
        "@type": "Rating",
        "ratingValue": "5",
        "bestRating": "5"
      },
      "author": {
        "@type": "Person",
        "name": "John D."
      },
      "reviewBody": "Amazing phone with great battery life and camera quality!"
    }
  ]
}
</script>
```

#### Organization Schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "My E-shop",
  "url": "https://myshop.com",
  "logo": "https://myshop.com/assets/images/logo.png",
  "description": "Your trusted online electronics store",
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+1-555-123-4567",
    "contactType": "Customer Service",
    "availableLanguage": ["English"],
    "areaServed": "US"
  },
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main Street",
    "addressLocality": "New York",
    "addressRegion": "NY",
    "postalCode": "10001",
    "addressCountry": "US"
  },
  "sameAs": [
    "https://facebook.com/myshop",
    "https://twitter.com/myshop",
    "https://instagram.com/myshop"
  ]
}
</script>
```

## 🏪 E-commerce SEO Best Practices

### URL Structure

```
✅ Good URLs:
https://myshop.com/electronics/iphone-15/
https://myshop.com/electronics/?page=2
https://myshop.com/blog/how-to-choose-phone/
https://myshop.com/contact/

❌ Bad URLs:
https://myshop.com/product.php?id=123
https://myshop.com/cat/1/subcat/5/
https://myshop.com/page.html?p=2&c=electronics
```

### Title Tag Optimization

```html
<!-- Product pages -->
<title>iPhone 15 128GB Blue - Best Price | My E-shop</title>

<!-- Category pages -->
<title>Electronics - Phones, Computers & More | My E-shop</title>

<!-- Blog posts -->
<title>How to Choose the Right Smartphone in 2024</title>

<!-- Static pages -->
<title>Contact Us - Customer Support | My E-shop</title>
```

### Meta Description Best Practices

```html
<!-- Product (max 160 characters) -->
<meta name="description" content="iPhone 15 with A17 Bionic chip and USB-C. Free shipping over $50. 30-day returns. In stock - order now!">

<!-- Category -->
<meta name="description" content="Wide selection of electronics: smartphones, laptops, tablets, accessories. Best prices, free shipping over $50.">

<!-- Blog post -->
<meta name="description" content="Complete 2024 guide to choosing the perfect smartphone. Compare features, prices, and find the best phone for your needs.">
```

## 🚀 Performance SEO

### Core Web Vitals Optimization

GitCart is optimized for Google's Core Web Vitals:

#### Largest Contentful Paint (LCP)
- **Optimized images** - WebP format, responsive sizes
- **Critical CSS inlining** - Above-the-fold styles
- **CDN integration** - Fast asset delivery
- **Lazy loading** - Images load when needed

#### First Input Delay (FID)
- **Minimal JavaScript** - Only essential JS loaded initially
- **Async loading** - Non-critical scripts deferred
- **Service Worker** - Caching for faster interactions

#### Cumulative Layout Shift (CLS)
- **Reserved space** - Image and ad dimensions set
- **Web fonts** - font-display: swap
- **Stable layouts** - No content jumping

### Performance Configuration

```json
{
  "build": {
    "minify_css": true,
    "minify_js": true,
    "optimize_images": true,
    "webp_conversion": true,
    "lazy_loading": true,
    "critical_css": true,
    "service_worker": true,
    "cache_busting": true
  }
}
```

## 🔍 Search Features

### Internal Search SEO

```html
<!-- Search results page -->
<title>Search Results for "iPhone" - My E-shop</title>
<meta name="description" content="Found 15 products for 'iPhone' - Browse smartphones, accessories and more. Best prices guaranteed.">
<link rel="canonical" href="https://myshop.com/search/?q=iphone">

<!-- No results page -->
<title>No Results for "xyzabc" - My E-shop</title>
<meta name="description" content="No products found for 'xyzabc'. Browse our electronics, clothing, and home categories for great deals.">
<meta name="robots" content="noindex, follow">
```

### Search Console Integration

```javascript
// Track internal searches
GitCart.on('gitcart:search:query', function(data) {
  // Google Analytics 4
  gtag('event', 'search', {
    search_term: data.query,
    results_count: data.results.length
  });
  
  // Search Console API (optional)
  if (data.results.length === 0) {
    // Track zero-result searches for content improvement
    console.log('Zero results for:', data.query);
  }
});
```

## 📊 SEO Monitoring & Analytics

### Essential SEO Metrics

```javascript
// Track SEO-relevant events
GitCart.on('gitcart:page:loaded', function(data) {
  // Page performance
  const navigation = performance.getEntriesByType('navigation')[0];
  const lcp = performance.getEntriesByType('largest-contentful-paint')[0];
  
  gtag('event', 'page_timing', {
    page_load_time: Math.round(navigation.loadEventEnd - navigation.fetchStart),
    lcp_time: lcp ? Math.round(lcp.startTime) : null,
    page_type: data.page
  });
});

// Track product page engagement
GitCart.on('gitcart:product:viewed', function(data) {
  gtag('event', 'view_item', {
    currency: data.currency,
    value: data.price,
    items: [{
      item_id: data.sku,
      item_name: data.title,
      item_category: data.category,
      price: data.price
    }]
  });
});
```

### SEO Audit Configuration

```json
{
  "seo": {
    "audit": {
      "enabled": true,
      "check_title_length": true,
      "check_meta_description": true,
      "check_h1_tags": true,
      "check_alt_attributes": true,
      "check_canonical_urls": true,
      "check_broken_links": true,
      "generate_report": true
    }
  }
}
```

## 🛠️ Advanced SEO Configuration

### Custom SEO Settings per Product

```markdown
---
title: iPhone 15 128GB Blue
price: 999
sku: IPHONE15-128-BLUE
# Custom SEO
seo_title: "iPhone 15 128GB Blue - Best Price & Free Shipping"
seo_description: "Get the new iPhone 15 with A17 Bionic chip, USB-C, and 48MP camera. Free shipping over $50, 30-day returns. Order now!"
canonical_url: "https://myshop.com/electronics/iphone-15/"
robots: "index, follow"
priority: 0.9
changefreq: "daily"
# OpenGraph overrides
og_title: "iPhone 15 - The Most Advanced iPhone Yet"
og_description: "Revolutionary USB-C, powerful A17 Bionic, pro camera system."
og_image: "iphone-15-og.jpg"
---
```

### Category SEO Configuration

```markdown
---
title: Electronics
description: Latest electronic devices at great prices
# SEO settings
seo_title: "Electronics - Phones, Computers, Tablets | My E-shop"
seo_description: "Huge selection of electronics: smartphones, laptops, tablets, smart home devices. Best prices, expert reviews, free shipping over $50."
featured_products: 12
sort_order: 1
priority: 0.8
changefreq: "weekly"
# Schema markup
schema_type: "CollectionPage"
breadcrumb_name: "Electronics"
---
```

## 🌍 International SEO

### Hreflang Implementation

```html
<!-- Automatic hreflang generation -->
<link rel="alternate" hreflang="en-us" href="https://myshop.com/electronics/iphone-15/">
<link rel="alternate" hreflang="cs-cz" href="https://myshop.com/cs/electronics/iphone-15/">
<link rel="alternate" hreflang="de-de" href="https://myshop.com/de/electronics/iphone-15/">
<link rel="alternate" hreflang="x-default" href="https://myshop.com/electronics/iphone-15/">
```

### Currency and Pricing SEO

```javascript
// Structured data per locale
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "iPhone 15",
  "offers": {
    "@type": "Offer",
    "priceCurrency": "USD",
    "price": "999",
    "eligibleRegion": {
      "@type": "GeoShape",
      "addressCountry": "US"
    }
  }
}
```

## 🎯 SEO Testing & Validation

### Build-time SEO Checks

```bash
# Run SEO audit during build
gitcart build --seo-audit

✅ SEO Audit Results:
  📄 Pages analyzed: 156
  ✅ Title tags: All pages have titles (avg: 42 chars)
  ✅ Meta descriptions: 154/156 pages (avg: 138 chars)
  ⚠️  H1 tags: 2 pages missing H1
  ✅ Image alt attributes: 98% coverage
  ✅ Canonical URLs: All pages canonical
  ⚠️  Sitemap: 2 broken internal links found

🚀 Performance:
  ✅ LCP: 1.2s (Good)
  ✅ FID: 45ms (Good)
  ✅ CLS: 0.05 (Good)
```

### SEO Plugin Integration

```javascript
// SEO monitoring plugin
(function() {
  const CONFIG = {
    name: 'seo-monitor',
    track_performance: true,
    track_searches: true,
    report_errors: true
  };

  const SEOMonitor = {
    init() {
      this.trackPageLoad();
      this.monitorCoreWebVitals();
      this.trackSearchBehavior();
    },

    trackPageLoad() {
      // Track SEO-relevant page metrics
      const observer = new PerformanceObserver((list) => {
        for (const entry of list.getEntries()) {
          if (entry.entryType === 'largest-contentful-paint') {
            gtag('event', 'web_vitals', {
              name: 'LCP',
              value: Math.round(entry.startTime),
              event_category: 'Web Vitals'
            });
          }
        }
      });
      observer.observe({entryTypes: ['largest-contentful-paint']});
    }
  };

  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('seo-monitor', {
      config: CONFIG,
      events: {
        'gitcart:page:loaded': SEOMonitor.trackPageLoad.bind(SEOMonitor)
      },
      init: SEOMonitor.init.bind(SEOMonitor)
    });
  }
})();
```

## 📈 SEO Checklist

### On-Page SEO Checklist

- ✅ **Title tags** - Unique, descriptive, 50-60 characters
- ✅ **Meta descriptions** - Compelling, 150-160 characters
- ✅ **H1 tags** - One per page, includes target keyword
- ✅ **URL structure** - Clean, descriptive, keyword-rich
- ✅ **Image alt text** - Descriptive alternative text
- ✅ **Canonical URLs** - Prevent duplicate content
- ✅ **Schema markup** - Product, Organization, BreadcrumbList
- ✅ **Internal linking** - Logical site structure

### Technical SEO Checklist

- ✅ **Sitemap.xml** - Complete, up-to-date
- ✅ **Robots.txt** - Proper crawl instructions
- ✅ **Page speed** - Core Web Vitals optimized
- ✅ **Mobile-friendly** - Responsive design
- ✅ **HTTPS** - Secure connection
- ✅ **Structured data** - Error-free markup
- ✅ **Hreflang** - For multilingual sites
- ✅ **404 handling** - Custom 404 pages

### Content SEO Checklist

- ✅ **Unique content** - Original product descriptions
- ✅ **Keyword optimization** - Natural keyword usage
- ✅ **Content length** - Sufficient detail for products
- ✅ **User intent** - Content matches search intent
- ✅ **Product specifications** - Detailed technical info
- ✅ **Customer reviews** - User-generated content
- ✅ **FAQ sections** - Answer common questions
- ✅ **Blog content** - Helpful, informative articles