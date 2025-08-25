# GitCart - Template systém

Kompletní příručka pro práci s templates v GitCart.

## 🎨 Template Engine podpory

GitCart podporuje několik template engines s Liquid jako výchozí volbou:

| Engine | Extension | Popis | Kompatibilita |
|--------|-----------|-------|---------------|
| **Liquid** | `.liquid` | Shopify/Jekyll syntax (výchozí) | ⭐⭐⭐ |
| Nunjucks | `.njk` | Mozilla template engine | ⭐⭐ |  
| Handlebars | `.hbs` | Minimalistický syntax | ⭐⭐ |
| Mustache | `.mustache` | Logic-less templates | ⭐ |

### Konfigurace template engine

```json
{
  "template_engine": "liquid"
}
```

## 📁 Struktura template systému

### Výchozí templates (vestavěné)
```
templates/default/
├── layout/
│   ├── base.liquid          # Základní layout
│   ├── category.liquid      # Kategorie produktů  
│   ├── product.liquid       # Detail produktu
│   ├── page.liquid         # Statické stránky
│   └── blog-post.liquid    # Blog články
├── component/
│   ├── header.liquid        # Hlavička webu
│   ├── footer.liquid        # Patička webu
│   ├── product-card.liquid  # Karta produktu
│   ├── cart-widget.liquid   # Košík widget
│   ├── search-box.liquid    # Vyhledávání
│   └── pagination.liquid    # Stránkování
├── page/
│   ├── home.liquid         # Domovská stránka
│   ├── cart.liquid         # Stránka košíku
│   ├── checkout.liquid     # Checkout proces
│   ├── blog-index.liquid   # Seznam blogů
│   └── 404.liquid         # Chybová stránka
└── assets/
    ├── css/styles.css
    └── js/app.js
```

### Uživatelské templates (override)
```
your-project/
├── templates/               # Přepíše výchozí templates
│   ├── layout/
│   │   └── product.liquid  # Custom produkt layout
│   ├── component/
│   │   └── header.liquid   # Custom hlavička
│   └── assets/
│       └── css/custom.css  # Dodatečné styly
```

## 🏗️ Template layouts

### base.liquid - Základní layout

```liquid
<!DOCTYPE html>
<html lang="{{ page.locale }}">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- SEO Meta Tags -->
  <title>{{ page.seo_title | default: page.title }}{{ site.seo.default_title_suffix }}</title>
  <meta name="description" content="{{ page.seo_description | default: page.description | default: site.description }}">
  <link rel="canonical" href="{{ page.canonical_url }}">
  
  <!-- Open Graph -->
  <meta property="og:title" content="{{ page.title }}">
  <meta property="og:description" content="{{ page.description }}">
  <meta property="og:url" content="{{ page.url }}">
  <meta property="og:image" content="{{ page.og_image | default: site.seo.og_image | asset_url }}">
  <meta property="og:type" content="{% if product %}product{% else %}website{% endif %}">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="{{ page.title }}">
  <meta name="twitter:description" content="{{ page.description }}">
  <meta name="twitter:image" content="{{ page.og_image | default: site.seo.og_image | asset_url }}">
  
  <!-- Hreflang pro vícejazyčnost -->
  {% for lang_code, language in site.languages %}
    {% if page.translations[lang_code] %}
      <link rel="alternate" hreflang="{{ lang_code }}" href="{{ page.translations[lang_code].url }}">
    {% endif %}
  {% endfor %}
  
  <!-- Plugin zones -->
  {% zone 'head-meta' %}
  
  <!-- Styles -->
  <link rel="stylesheet" href="{{ 'css/styles.css' | asset_url }}">
  {% for style in page.additional_styles %}
    <link rel="stylesheet" href="{{ style | asset_url }}">
  {% endfor %}
</head>
<body class="{{ page.body_class }}">
  {% zone 'body-start' %}
  
  <!-- Header -->
  {% include 'component/header' %}
  
  <!-- Main Content -->
  <main id="main" role="main">
    {{ content }}
  </main>
  
  <!-- Footer -->
  {% include 'component/footer' %}
  
  <!-- Plugin zones -->
  {% zone 'before-footer' %}
  {% zone 'body-end' %}
  
  <!-- Scripts -->
  <script src="{{ 'js/gitcart.js' | asset_url }}"></script>
  {% for script in page.additional_scripts %}
    <script src="{{ script | asset_url }}"></script>
  {% endfor %}
</body>
</html>
```

### product.liquid - Produkt layout

```liquid
---
layout: base
---

{% assign product = page.product %}

<div class="product-detail">
  <!-- Breadcrumb -->
  <nav class="breadcrumb" aria-label="Breadcrumb">
    <ol>
      <li><a href="{{ '/' | url }}">{{ 'Domů' | t }}</a></li>
      <li><a href="{{ product.category.url }}">{{ product.category.title }}</a></li>
      <li aria-current="page">{{ product.title }}</li>
    </ol>
  </nav>
  
  <div class="product-layout">
    <!-- Product Images -->
    <div class="product-images">
      {% if product.images.size > 0 %}
        <div class="product-gallery">
          <div class="main-image">
            <img src="{{ product.images[0] | asset_url: 'large' }}" 
                 alt="{{ product.images[0].alt | default: product.title }}"
                 id="product-main-image">
          </div>
          
          {% if product.images.size > 1 %}
            <div class="thumbnail-images">
              {% for image in product.images %}
                <button class="thumbnail{% if forloop.first %} active{% endif %}"
                        onclick="changeMainImage('{{ image | asset_url: 'large' }}', '{{ image.alt | default: product.title }}')">
                  <img src="{{ image | asset_url: 'small' }}" 
                       alt="{{ image.alt | default: product.title }}">
                </button>
              {% endfor %}
            </div>
          {% endif %}
        </div>
      {% else %}
        <div class="product-no-image">
          <div class="placeholder">{{ 'Bez obrázku' | t }}</div>
        </div>
      {% endif %}
      
      {% zone 'product-detail-after-images' %}
    </div>
    
    <!-- Product Info -->
    <div class="product-info">
      <header class="product-header">
        <h1>{{ product.title }}</h1>
        {% if product.sku %}
          <div class="product-sku">{{ 'SKU' | t }}: {{ product.sku }}</div>
        {% endif %}
      </header>
      
      <!-- Price -->
      <div class="product-price">
        {% if product.sale_price and product.sale_price < product.price %}
          <span class="price-sale">{{ product.sale_price | money: product.currency }}</span>
          <span class="price-original">{{ product.price | money: product.currency }}</span>
          <span class="price-save">
            {{ 'Ušetříte' | t }} {{ product.price | minus: product.sale_price | money: product.currency }}
          </span>
        {% else %}
          <span class="price-current">{{ product.price | money: product.currency }}</span>
        {% endif %}
      </div>
      
      <!-- Stock Status -->
      <div class="product-stock">
        {% if product.stock > 0 %}
          <span class="in-stock">✓ {{ 'Skladem' | t }} ({{ product.stock }} {{ 'ks' | t }})</span>
        {% else %}
          <span class="out-of-stock">{{ 'Vyprodáno' | t }}</span>
        {% endif %}
      </div>
      
      <!-- Add to Cart -->
      {% if product.stock > 0 %}
        <form class="add-to-cart-form" onsubmit="return addToCart(event)">
          <input type="hidden" name="sku" value="{{ product.sku }}">
          <input type="hidden" name="title" value="{{ product.title }}">
          <input type="hidden" name="price" value="{{ product.sale_price | default: product.price }}">
          <input type="hidden" name="image" value="{{ product.images[0] | asset_url }}">
          
          <div class="quantity-input">
            <label for="quantity">{{ 'Množství' | t }}:</label>
            <input type="number" id="quantity" name="quantity" 
                   value="1" min="1" max="{{ product.stock }}">
          </div>
          
          <button type="submit" class="btn btn-primary btn-add-cart">
            <span class="btn-text">{{ 'Přidat do košíku' | t }}</span>
            <span class="btn-loading" style="display:none;">{{ 'Přidávám...' | t }}</span>
          </button>
        </form>
      {% endif %}
      
      <!-- Product Tags -->
      {% if product.tags.size > 0 %}
        <div class="product-tags">
          <strong>{{ 'Štítky' | t }}:</strong>
          {% for tag in product.tags %}
            <span class="tag">{{ tag }}</span>
          {% endfor %}
        </div>
      {% endif %}
    </div>
  </div>
  
  <!-- Product Description -->
  <div class="product-description">
    {{ product.content }}
  </div>
  
  <!-- Specifications -->
  {% if product.specifications %}
    <div class="product-specifications">
      <h2>{{ 'Technické specifikace' | t }}</h2>
      <dl class="specs-list">
        {% for spec in product.specifications %}
          <dt>{{ spec[0] | capitalize }}</dt>
          <dd>{{ spec[1] }}</dd>
        {% endfor %}
      </dl>
    </div>
  {% endif %}
  
  <!-- Related Products -->
  {% assign related_products = product.category.products | where: 'featured', true | sample: 4 %}
  {% if related_products.size > 0 %}
    <section class="related-products">
      <h2>{{ 'Související produkty' | t }}</h2>
      <div class="products-grid">
        {% for related in related_products %}
          {% unless related.sku == product.sku %}
            {% include 'component/product-card', product: related %}
          {% endunless %}
        {% endfor %}
      </div>
    </section>
  {% endif %}
  
  {% zone 'product-detail-end' %}
</div>

<!-- JSON-LD Structured Data -->
<script type="application/ld+json">
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "{{ product.title | json }}",
  "description": "{{ product.description | default: product.excerpt | strip_html | json }}",
  "sku": "{{ product.sku | json }}",
  "image": [
    {% for image in product.images %}
      "{{ image | asset_url: 'large' }}"{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ],
  "offers": {
    "@type": "Offer",
    "url": "{{ product.url | absolute_url | json }}",
    "priceCurrency": "{{ product.currency | json }}",
    "price": "{{ product.sale_price | default: product.price | json }}",
    "availability": "{% if product.stock > 0 %}https://schema.org/InStock{% else %}https://schema.org/OutOfStock{% endif %}",
    "seller": {
      "@type": "Organization",
      "name": "{{ site.name | json }}"
    }
  }
}
</script>
```

## 🧩 Komponenty

### header.liquid

```liquid
<header class="site-header" role="banner">
  <div class="header-container">
    <!-- Logo -->
    <div class="header-logo">
      <a href="{{ '/' | url }}" class="logo-link">
        {% if site.logo %}
          <img src="{{ site.logo | asset_url }}" alt="{{ site.name }}" class="logo-image">
        {% else %}
          <span class="logo-text">{{ site.name }}</span>
        {% endif %}
      </a>
    </div>
    
    <!-- Mobile Menu Toggle -->
    <button class="mobile-menu-toggle" onclick="toggleMobileMenu()" aria-label="Toggle navigation">
      <span class="hamburger"></span>
    </button>
    
    <!-- Navigation -->
    <nav class="main-navigation" role="navigation">
      <ul class="nav-menu">
        <!-- Categories -->
        {% for category in site.categories %}
          {% if category.featured %}
            <li class="nav-item{% if category.children.size > 0 %} has-dropdown{% endif %}">
              <a href="{{ category.url }}" class="nav-link">
                {{ category.title }}
              </a>
              
              {% if category.children.size > 0 %}
                <ul class="dropdown-menu">
                  {% for child in category.children %}
                    <li>
                      <a href="{{ child.url }}">{{ child.title }}</a>
                    </li>
                  {% endfor %}
                </ul>
              {% endif %}
            </li>
          {% endif %}
        {% endfor %}
        
        <!-- Custom Navigation -->
        {% for link in navigation.header %}
          <li class="nav-item">
            <a href="{{ link.url }}" class="nav-link">
              {% if link.icon %}<i class="icon-{{ link.icon }}"></i>{% endif %}
              {{ link.title }}
            </a>
          </li>
        {% endfor %}
      </ul>
    </nav>
    
    <!-- Header Actions -->
    <div class="header-actions">
      <!-- Language Switcher -->
      {% if site.languages.size > 1 %}
        <div class="language-switcher">
          <button class="lang-toggle" onclick="toggleLangMenu()">
            {{ current_language.short | upcase }}
          </button>
          <ul class="lang-menu">
            {% for lang_code, language in site.languages %}
              {% unless lang_code == current_language.code %}
                <li>
                  <a href="{{ page.translations[lang_code].url | default: '/' }}">
                    {{ language.name }}
                  </a>
                </li>
              {% endunless %}
            {% endfor %}
          </ul>
        </div>
      {% endif %}
      
      <!-- Search -->
      {% include 'component/search-box' %}
      
      <!-- Cart Widget -->
      {% include 'component/cart-widget' %}
    </div>
  </div>
</header>
```

### product-card.liquid

```liquid
<article class="product-card">
  <div class="product-card-image">
    <a href="{{ product.url }}" class="image-link">
      {% if product.images.size > 0 %}
        <img src="{{ product.images[0] | asset_url: 'medium' }}" 
             alt="{{ product.images[0].alt | default: product.title }}"
             loading="lazy">
      {% else %}
        <div class="no-image-placeholder">
          <span>{{ 'Bez obrázku' | t }}</span>
        </div>
      {% endif %}
      
      {% if product.sale_price and product.sale_price < product.price %}
        <span class="product-badge sale-badge">{{ 'SLEVA' | t }}</span>
      {% endif %}
      
      {% if product.featured %}
        <span class="product-badge featured-badge">{{ 'DOPORUČUJEME' | t }}</span>
      {% endif %}
      
      {% if product.stock == 0 %}
        <span class="product-badge stock-badge">{{ 'VYPRODÁNO' | t }}</span>
      {% endif %}
    </a>
    
    <!-- Quick Action Buttons -->
    <div class="product-actions">
      {% if product.stock > 0 %}
        <button class="btn-quick-add" 
                onclick="quickAddToCart('{{ product.sku }}', '{{ product.title | escape }}', {{ product.sale_price | default: product.price }})"
                title="{{ 'Rychlé přidání do košíku' | t }}">
          <i class="icon-cart-plus"></i>
        </button>
      {% endif %}
      
      <button class="btn-quick-view" 
              onclick="openQuickView('{{ product.sku }}')"
              title="{{ 'Rychlý náhled' | t }}">
        <i class="icon-eye"></i>
      </button>
    </div>
  </div>
  
  <div class="product-card-content">
    <header class="product-header">
      <h3 class="product-title">
        <a href="{{ product.url }}">{{ product.title }}</a>
      </h3>
      
      {% if product.sku %}
        <div class="product-sku">{{ product.sku }}</div>
      {% endif %}
    </header>
    
    <div class="product-price">
      {% if product.sale_price and product.sale_price < product.price %}
        <span class="price-sale">{{ product.sale_price | money: product.currency }}</span>
        <span class="price-original">{{ product.price | money: product.currency }}</span>
      {% else %}
        <span class="price-current">{{ product.price | money: product.currency }}</span>
      {% endif %}
    </div>
    
    {% if product.excerpt %}
      <div class="product-excerpt">
        {{ product.excerpt | strip_html | truncate: 100 }}
      </div>
    {% endif %}
    
    {% if product.tags.size > 0 %}
      <div class="product-tags">
        {% for tag in product.tags limit: 3 %}
          <span class="tag">{{ tag }}</span>
        {% endfor %}
      </div>
    {% endif %}
    
    <footer class="product-footer">
      {% if product.stock > 0 %}
        <button class="btn btn-primary btn-add-cart"
                onclick="addToCart(event, '{{ product.sku }}', '{{ product.title | escape }}', {{ product.sale_price | default: product.price }})">
          {{ 'Přidat do košíku' | t }}
        </button>
      {% else %}
        <button class="btn btn-secondary" disabled>
          {{ 'Vyprodáno' | t }}
        </button>
      {% endif %}
      
      <a href="{{ product.url }}" class="btn btn-outline">
        {{ 'Detail' | t }}
      </a>
    </footer>
  </div>
</article>
```

## 🎯 Rendering zones

Zones umožňují pluginům vkládat obsah do templates.

### Dostupné zones

| Zone | Umístění | Účel |
|------|----------|------|
| `head-meta` | `<head>` | SEO meta tagy, tracking kódy |
| `body-start` | Za `<body>` | Analytics, tracking pixely |
| `product-detail-after-images` | Po obrázcích produktu | Reviews, dodatečný obsah |
| `product-detail-end` | Konec produktu | Related content |
| `shipping-selection` | Checkout shipping | Custom shipping methods |
| `payment-selection` | Checkout payment | Payment gateways |
| `order-success-info` | Thank you page | Tracking, additional info |
| `before-footer` | Před footrem | Newsletter, CTA |
| `body-end` | Před `</body>` | Chat widgets, scripts |

### Použití v templates

```liquid
<!-- Vložit zone -->
{% zone 'product-detail-after-images' %}

<!-- Zone s kontextem -->
{% zone 'product-detail-after-images', product: product, category: category %}

<!-- Zone s fallback -->
{% zone 'custom-zone' %}
  <div>Výchozí obsah když žádný plugin neposkytuje obsah</div>
{% endzone %}
```

### Plugin rendering

```javascript
GitCart.plugin('my-plugin', {
  render: {
    'product-detail-after-images': function(context) {
      if (!context.product) return '';
      
      return `
        <div class="plugin-content">
          <h3>Dodatečné informace</h3>
          <p>Produkt: ${context.product.title}</p>
        </div>
      `;
    }
  }
});
```

## 🎨 CSS a styling

### Tailwind CSS integrace

```liquid
<!-- templates/layout/base.liquid -->
<link rel="stylesheet" href="{{ 'css/styles.css' | asset_url }}">
```

```css
/* templates/assets/css/styles.css */
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';

/* Custom komponenty */
.product-card {
  @apply bg-white shadow-md rounded-lg overflow-hidden transition-shadow hover:shadow-lg;
}

.btn {
  @apply px-4 py-2 rounded font-medium transition-colors focus:outline-none focus:ring-2;
}

.btn-primary {
  @apply bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500;
}

.btn-secondary {
  @apply bg-gray-600 text-white hover:bg-gray-700 focus:ring-gray-500;
}

/* Dark mode podpora */
@media (prefers-color-scheme: dark) {
  .product-card {
    @apply bg-gray-800 text-white;
  }
}
```

## 🔧 Custom filters

GitCart poskytuje vlastní Liquid filtry:

### Asset filtry

```liquid
<!-- Generování URL pro asset -->
{{ 'css/styles.css' | asset_url }}
<!-- /assets/css/styles.css -->

<!-- Asset s version hash -->
{{ 'js/app.js' | asset_url: 'versioned' }}
<!-- /assets/js/app.js?v=abc123 -->

<!-- Optimalizovaný obrázek -->
{{ 'product.jpg' | asset_url: 'large' }}
<!-- /assets/images/product-large.jpg -->
```

### URL filtry pro stránky a sekce

```liquid
<!-- Generování URL ze jména stránky -->
{{ 'kontakt' | page_url: current_language }}
<!-- /kontakt/ (nebo /en/contact/ pro vícejazyčné) -->

{{ 'o-nas' | page_url }}
<!-- Použije current_language automaticky -->

<!-- Generování URL pro sekce (blog a kategorie) -->
{{ 'blog' | section_url: current_language }}
<!-- /blog/ -->

{{ 'elektronika' | section_url: current_language }}
<!-- /elektronika/ (nebo /en/electronics/) -->

<!-- Legacy: Přímé URL řetězce (stále podporováno) -->
{{ '/kontakt' | url: current_language }}
<!-- /kontakt -->
```

### Formátovací filtry

```liquid
<!-- Formátování ceny -->
{{ 1999 | money: 'CZK' }}
<!-- 1 999 Kč -->

{{ 29.99 | money: 'USD' }}
<!-- $29.99 -->

<!-- Formátování data -->
{{ '2024-01-15' | date: '%d.%m.%Y' }}
<!-- 15.01.2024 -->

<!-- URL generování -->
{{ '/produkty/iphone' | url }}
<!-- /cs/produkty/iphone (podle URL strategy) -->

<!-- Truncate s HTML -->
{{ product.description | strip_html | truncate: 100 }}
```

### SEO filtry

```liquid
<!-- Escape pro HTML atributy -->
{{ product.title | escape }}

<!-- JSON escape pro structured data -->
{{ product.description | json }}

<!-- Slug generování -->
{{ 'Příliš žluťoučký kůň' | slugify }}
<!-- prilis-zlutoucky-kun -->
```

## 🌍 Překlady (i18n)

### Použití překladů v templates

```liquid
<!-- Základní překlad -->
{{ 'Přidat do košíku' | t }}

<!-- Překlad s parametry -->
{{ 'Zobrazuji %{start} až %{end} z %{total} produktů' | t: start: 1, end: 12, total: 45 }}

<!-- Překlad s fallback -->
{{ 'custom.message' | t | default: 'Výchozí text' }}

<!-- Pluralizace -->
{{ product.stock | pluralize: 'produkt', 'produkty', 'produktů' }}
```

### Translation soubory

```yaml
# locales/cs.yml
cs:
  add_to_cart: "Přidat do košíku"
  out_of_stock: "Vyprodáno"
  price_from: "od %{price}"
  products_count:
    one: "%{count} produkt"
    few: "%{count} produkty"  
    many: "%{count} produktů"
    other: "%{count} produktů"
```

## ⚡ Výkon a optimalizace

### Template caching

```liquid
<!-- Cache blok pro náročné operace -->
{% cache 'expensive-operation', expires_in: 3600 %}
  {% assign related = collections.all | where: 'featured', true | sample: 10 %}
  <!-- Render related products -->
{% endcache %}
```

### Lazy loading

```liquid
<!-- Lazy loading obrázků -->
<img src="{{ product.image | asset_url: 'small' }}" 
     data-src="{{ product.image | asset_url: 'large' }}"
     alt="{{ product.title }}"
     loading="lazy"
     class="lazy-image">
```

### Critical CSS

```liquid
<!-- Inline kritický CSS -->
<style>
  /* Critical above-the-fold CSS */
  .header { /* styles */ }
  .hero { /* styles */ }
</style>

<!-- Async loading nekritického CSS -->
<link rel="preload" href="{{ 'css/styles.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

## 🐛 Debugging templates

### Template debugging

```liquid
<!-- Debug informace -->
{% if settings.debug %}
  <div class="debug-info">
    <h4>Template: {{ template }}</h4>
    <h4>Page: {{ page.template }}</h4>
    <pre>{{ page | json }}</pre>
  </div>
{% endif %}

<!-- Comment pro debugging -->
{% comment %}
  Debug: Product has {{ product.images.size }} images
  Category: {{ product.category.title }}
  Price: {{ product.price }} {{ product.currency }}
{% endcomment %}
```

### Error handling

```liquid
<!-- Safe property access -->
{{ product.images[0].alt | default: product.title | default: 'Obrázek produktu' }}

<!-- Existence check -->
{% if product.specifications and product.specifications.size > 0 %}
  <!-- Render specifications -->
{% endif %}

<!-- Try/rescue pattern -->
{% assign price = product.sale_price | default: product.price | default: 0 %}
```