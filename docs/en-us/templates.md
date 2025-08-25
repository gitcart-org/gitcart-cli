# GitCart - Template System

Complete guide for working with templates in GitCart.

## 🎨 Template Engine Support

GitCart supports several template engines with Liquid as the default choice:

| Engine | Extension | Description | Compatibility |
|--------|-----------|-------------|---------------|
| **Liquid** | `.liquid` | Shopify/Jekyll syntax (default) | ⭐⭐⭐ |
| Nunjucks | `.njk` | Mozilla template engine | ⭐⭐ |  
| Handlebars | `.hbs` | Minimalist syntax | ⭐⭐ |
| Mustache | `.mustache` | Logic-less templates | ⭐ |

### Template Engine Configuration

```json
{
  "template_engine": "liquid"
}
```

## 📁 Template System Structure

### Default Templates (Built-in)
```
templates/default/
├── layout/
│   ├── base.liquid          # Base layout
│   ├── category.liquid      # Product categories  
│   ├── product.liquid       # Product detail
│   ├── page.liquid         # Static pages
│   └── blog-post.liquid    # Blog articles
├── component/
│   ├── header.liquid        # Site header
│   ├── footer.liquid        # Site footer
│   ├── product-card.liquid  # Product card
│   ├── cart-widget.liquid   # Cart widget
│   ├── search-box.liquid    # Search functionality
│   └── pagination.liquid    # Pagination
├── page/
│   ├── home.liquid         # Homepage
│   ├── cart.liquid         # Cart page
│   ├── checkout.liquid     # Checkout process
│   ├── blog-index.liquid   # Blog listing
│   └── 404.liquid         # Error page
└── assets/
    ├── css/styles.css
    └── js/app.js
```

### User Templates (Override)
```
your-project/
├── templates/               # Overrides default templates
│   ├── layout/
│   │   └── product.liquid  # Custom product layout
│   ├── component/
│   │   └── header.liquid   # Custom header
│   └── assets/
│       └── css/custom.css  # Additional styles
```

## 🏗️ Template Layouts

### base.liquid - Base Layout

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
  
  <!-- Hreflang for multilingual -->
  {% for lang_code, language in site.languages %}
    {% if page.translations[lang_code] %}
      <link rel="alternate" hreflang="{{ lang_code }}" href="{{ page.translations[lang_code].url }}">
    {% endif %}
  {% endfor %}
  
  <!-- Stylesheets -->
  <link rel="stylesheet" href="{{ 'css/styles.css' | asset_url }}">
  
  <!-- Plugin zones -->
  {% zone 'zone-head' %}
</head>
<body class="{% if page.custom_class %}{{ page.custom_class }}{% endif %}">
  {% zone 'body-start' %}
  
  <!-- Header -->
  {% include 'component/header.liquid' %}
  
  {% zone 'header-end' %}
  
  <!-- Main Content -->
  <main class="main-content">
    {% zone 'main-start' %}
    {{ content }}
    {% zone 'main-end' %}
  </main>
  
  {% zone 'before-footer' %}
  
  <!-- Footer -->
  {% include 'component/footer.liquid' %}
  
  {% zone 'footer-end' %}
  
  <!-- Scripts -->
  <script src="{{ 'js/gitcart.js' | asset_url }}"></script>
  
  {% zone 'body-end' %}
</body>
</html>
```

### product.liquid - Product Detail Layout

```liquid
---
layout: base
---

<article class="product-detail container mx-auto px-4 py-8">
  <!-- Breadcrumb -->
  <nav class="breadcrumb mb-6">
    <a href="/" class="text-blue-600 hover:underline">{{ 'Home' | t: current_language }}</a>
    <span class="mx-2">/</span>
    <a href="{{ product.category.url }}" class="text-blue-600 hover:underline">{{ product.category.title }}</a>
    <span class="mx-2">/</span>
    <span class="text-gray-600">{{ product.title }}</span>
  </nav>

  <div class="product-layout grid lg:grid-cols-2 gap-12">
    <!-- Product Images -->
    <div class="product-images">
      {% zone 'product-images-start' %}
      
      {% if product.images.size > 0 %}
        <div class="main-image mb-4">
          <img src="{{ product.images[0] | img_url: 'large' }}" 
               alt="{{ product.images[0].alt | default: product.title }}"
               class="w-full rounded-lg">
        </div>
        
        {% if product.images.size > 1 %}
          <div class="image-thumbnails grid grid-cols-4 gap-2">
            {% for image in product.images limit: 8 %}
              <img src="{{ image | img_url: 'thumb' }}" 
                   alt="{{ image.alt | default: product.title }}"
                   class="thumbnail cursor-pointer rounded border-2 hover:border-blue-500">
            {% endfor %}
          </div>
        {% endif %}
      {% else %}
        <div class="no-image bg-gray-200 aspect-square flex items-center justify-center rounded-lg">
          <span class="text-gray-500">{{ 'No image available' | t: current_language }}</span>
        </div>
      {% endif %}
      
      {% zone 'product-images-end' %}
    </div>

    <!-- Product Information -->
    <div class="product-info">
      {% zone 'product-info-start' %}
      
      <h1 class="product-title text-3xl font-bold mb-4">{{ product.title }}</h1>
      
      <!-- SKU -->
      {% if product.sku %}
        <div class="product-sku text-gray-600 text-sm mb-4">
          {{ 'SKU' | t: current_language }}: {{ product.sku }}
        </div>
      {% endif %}

      <!-- Price -->
      <div class="product-price mb-6">
        {% if product.sale_price and product.sale_price < product.price %}
          <div class="flex items-center gap-4">
            <span class="sale-price text-3xl font-bold text-red-600">
              {{ product.sale_price | money: current_language_data.currency }}
            </span>
            <span class="original-price text-xl text-gray-500 line-through">
              {{ product.price | money: current_language_data.currency }}
            </span>
          </div>
          <div class="savings text-green-600 font-medium">
            {{ 'Save' | t: current_language }} {{ product.price | minus: product.sale_price | money: current_language_data.currency }}
          </div>
        {% else %}
          <span class="current-price text-3xl font-bold text-gray-900">
            {{ product.price | money: current_language_data.currency }}
          </span>
        {% endif %}
      </div>

      <!-- Stock Status -->
      <div class="stock-status mb-6">
        {% if product.stock > 0 %}
          <span class="in-stock text-green-600 flex items-center">
            <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
            </svg>
            {{ 'In Stock' | t: current_language }} ({{ product.stock }})
          </span>
        {% else %}
          <span class="out-of-stock text-red-600 flex items-center">
            <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path>
            </svg>
            {{ 'Out of Stock' | t: current_language }}
          </span>
        {% endif %}
      </div>

      {% zone 'product-before-add-to-cart' %}

      <!-- Add to Cart Form -->
      {% if product.stock > 0 %}
        <form class="add-to-cart-form mb-8" data-product-sku="{{ product.sku }}">
          <div class="quantity-selector mb-4">
            <label for="quantity" class="block text-sm font-medium mb-2">
              {{ 'Quantity' | t: current_language }}:
            </label>
            <input type="number" 
                   id="quantity" 
                   name="quantity" 
                   value="1" 
                   min="1" 
                   max="{{ product.stock }}"
                   class="w-24 px-3 py-2 border border-gray-300 rounded-md">
          </div>
          
          <button type="submit" 
                  class="add-to-cart-btn w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg transition-colors">
            {{ 'Add to Cart' | t: current_language }}
          </button>
        </form>
      {% else %}
        <div class="out-of-stock-notice bg-gray-100 p-4 rounded-lg text-center mb-8">
          <span class="text-gray-600">{{ 'This product is currently out of stock' | t: current_language }}</span>
        </div>
      {% endif %}

      {% zone 'product-after-add-to-cart' %}

      <!-- Product Tags -->
      {% if product.tags.size > 0 %}
        <div class="product-tags mb-6">
          <h3 class="text-sm font-medium text-gray-900 mb-2">{{ 'Tags' | t: current_language }}:</h3>
          <div class="flex flex-wrap gap-2">
            {% for tag in product.tags %}
              <span class="tag bg-gray-200 text-gray-700 px-3 py-1 text-sm rounded-full">
                {{ tag }}
              </span>
            {% endfor %}
          </div>
        </div>
      {% endif %}

      {% zone 'product-info-end' %}
    </div>
  </div>

  {% zone 'product-before-description' %}

  <!-- Product Description -->
  <div class="product-description mt-12">
    <h2 class="text-2xl font-bold mb-6">{{ 'Description' | t: current_language }}</h2>
    <div class="prose max-w-none">
      {{ product.content }}
    </div>
  </div>

  {% zone 'product-after-description' %}

  <!-- Product Specifications -->
  {% if product.specifications %}
    <div class="product-specifications mt-12">
      <h2 class="text-2xl font-bold mb-6">{{ 'Specifications' | t: current_language }}</h2>
      <div class="overflow-x-auto">
        <table class="w-full border-collapse border border-gray-300">
          {% for spec in product.specifications %}
            <tr class="border-b border-gray-300">
              <td class="border border-gray-300 px-4 py-3 bg-gray-50 font-medium">
                {{ spec[0] | capitalize }}
              </td>
              <td class="border border-gray-300 px-4 py-3">
                {{ spec[1] }}
              </td>
            </tr>
          {% endfor %}
        </table>
      </div>
    </div>
  {% endif %}

  {% zone 'product-end' %}
</article>

<!-- Add to Cart JavaScript -->
<script>
document.addEventListener('DOMContentLoaded', function() {
  const addToCartForm = document.querySelector('.add-to-cart-form');
  
  if (addToCartForm) {
    addToCartForm.addEventListener('submit', function(e) {
      e.preventDefault();
      
      const sku = this.dataset.productSku;
      const quantity = parseInt(this.querySelector('#quantity').value);
      
      if (typeof GitCart !== 'undefined') {
        GitCart.cart.add(sku, quantity, {
          title: '{{ product.title | escape }}',
          price: {{ product.sale_price | default: product.price }},
          image: '{{ product.images[0] | img_url: 'thumb' }}',
          url: '{{ product.url }}'
        });
        
        // Show success message
        alert('{{ "Product added to cart!" | t: current_language }}');
      }
    });
  }
});
</script>
```

## 🧩 Template Components

### header.liquid - Site Header

```liquid
<header class="site-header bg-white shadow-sm border-b border-gray-200">
  <div class="container mx-auto px-4">
    <div class="flex items-center justify-between h-16">
      <!-- Logo -->
      <div class="logo">
        <a href="{{ '/' | url: current_language }}" class="flex items-center">
          {% if site.logo %}
            <img src="{{ site.logo | asset_url }}" alt="{{ site.name }}" class="h-8">
          {% else %}
            <span class="text-xl font-bold text-gray-900">{{ site.name }}</span>
          {% endif %}
        </a>
      </div>
      
      <!-- Navigation -->
      <nav class="hidden md:flex items-center space-x-8">
        {% for item in navigation.header %}
          <a href="{{ item.url }}" 
             class="text-gray-600 hover:text-gray-900 px-3 py-2 text-sm font-medium">
            {{ item.title }}
          </a>
        {% endfor %}
      </nav>
      
      <!-- Right Side Actions -->
      <div class="flex items-center space-x-4">
        <!-- Language Switcher -->
        {% if site.languages.size > 1 %}
          <div class="language-switcher relative">
            <select class="appearance-none bg-white border border-gray-300 rounded px-3 py-1 text-sm">
              {% for lang_code, language in site.languages %}
                <option value="{{ lang_code }}" {% if lang_code == current_language %}selected{% endif %}>
                  {{ language.name }}
                </option>
              {% endfor %}
            </select>
          </div>
        {% endif %}
        
        <!-- Search -->
        {% include 'component/search-box.liquid' %}
        
        <!-- Cart Widget -->
        {% include 'component/cart-widget.liquid' %}
      </div>
    </div>
  </div>
</header>
```

### product-card.liquid - Product Card Component

```liquid
<article class="product-card bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow">
  <!-- Product Image -->
  <div class="product-image relative">
    <a href="{{ product.url }}" class="block aspect-w-1 aspect-h-1">
      {% if product.images.size > 0 %}
        <img src="{{ product.images[0] | img_url: 'medium' }}" 
             alt="{{ product.title }}" 
             class="w-full h-48 object-cover">
      {% else %}
        <div class="w-full h-48 bg-gray-200 flex items-center justify-center">
          <span class="text-gray-400">{{ 'No image' | t: current_language }}</span>
        </div>
      {% endif %}
    </a>
    
    <!-- Product Badges -->
    {% if product.featured %}
      <span class="absolute top-2 left-2 bg-yellow-500 text-white text-xs font-bold px-2 py-1 rounded">
        {{ 'Featured' | t: current_language }}
      </span>
    {% endif %}
    
    {% if product.sale_price and product.sale_price < product.price %}
      <span class="absolute top-2 right-2 bg-red-500 text-white text-xs font-bold px-2 py-1 rounded">
        {{ 'Sale' | t: current_language }}
      </span>
    {% endif %}
  </div>
  
  <!-- Product Info -->
  <div class="product-info p-4">
    <h3 class="product-title text-lg font-semibold mb-2">
      <a href="{{ product.url }}" class="text-gray-900 hover:text-blue-600">
        {{ product.title }}
      </a>
    </h3>
    
    <!-- Price -->
    <div class="product-price mb-3">
      {% if product.sale_price and product.sale_price < product.price %}
        <span class="sale-price text-lg font-bold text-red-600">
          {{ product.sale_price | money: current_language_data.currency }}
        </span>
        <span class="original-price text-sm text-gray-500 line-through ml-2">
          {{ product.price | money: current_language_data.currency }}
        </span>
      {% else %}
        <span class="current-price text-lg font-bold text-gray-900">
          {{ product.price | money: current_language_data.currency }}
        </span>
      {% endif %}
    </div>
    
    <!-- Stock Status -->
    <div class="stock-status mb-4">
      {% if product.stock > 0 %}
        <span class="text-green-600 text-sm">
          ✓ {{ 'In Stock' | t: current_language }}
        </span>
      {% else %}
        <span class="text-red-600 text-sm">
          ✗ {{ 'Out of Stock' | t: current_language }}
        </span>
      {% endif %}
    </div>
    
    <!-- Actions -->
    <div class="product-actions flex space-x-2">
      {% if product.stock > 0 %}
        <button class="quick-add flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2 px-4 rounded text-sm font-medium transition-colors"
                onclick="quickAddToCart('{{ product.sku }}')">
          {{ 'Add to Cart' | t: current_language }}
        </button>
      {% else %}
        <button class="flex-1 bg-gray-400 text-white py-2 px-4 rounded text-sm font-medium cursor-not-allowed" disabled>
          {{ 'Out of Stock' | t: current_language }}
        </button>
      {% endif %}
      
      <a href="{{ product.url }}" 
         class="view-product border border-gray-300 text-gray-700 hover:bg-gray-50 py-2 px-3 rounded text-sm font-medium transition-colors">
        {{ 'View' | t: current_language }}
      </a>
    </div>
  </div>
</article>
```

## 🔌 Plugin Zone System

Plugin zones allow third-party plugins to inject content into templates at specific points.

### Available Zones

**Global Zones:**
- `head` - Meta tags, analytics, custom CSS
- `body-start` - Tracking codes, A/B testing scripts
- `header-end` - After site header
- `main-start` - Beginning of main content
- `main-end` - End of main content  
- `before-footer` - Before site footer
- `footer-end` - After site footer
- `body-end` - Scripts, chat widgets

**Product-specific Zones:**
- `product-images-start/end` - Around product image gallery
- `product-info-start/end` - Around product information
- `product-before-add-to-cart` - Before add to cart button
- `product-after-add-to-cart` - After add to cart form
- `product-before-description` - Before product description
- `product-after-description` - After product description
- `product-end` - End of product page

**Category-specific Zones:**
- `category-start/end` - Beginning/end of category page
- `category-after-header` - After category header
- `category-before-products` - Before product listing

### Using Zones in Templates

```liquid
<!-- Basic zone placement -->
{% zone 'zone-name' %}

<!-- Zone with context data -->
{% zone 'product-recommendations' product: product, category: product.category %}

<!-- Conditional zones -->
{% if product.featured %}
  {% zone 'featured-product-badge' %}
{% endif %}
```

### Example Plugin Integration

```javascript
// Plugin registers content for zones
GitCart.plugin('reviews', {
  render: {
    'product-after-description': function(context) {
      return `<div class="product-reviews">
        <h3>Customer Reviews</h3>
        <!-- Review content -->
      </div>`;
    }
  }
});
```

## 🎨 Custom Filters

GitCart extends Liquid with e-commerce specific filters:

### URL Filters
```liquid
<!-- Asset URLs with versioning -->
{{ 'css/style.css' | asset_url }}
<!-- /assets/css/style.css -->

{{ 'css/style.css' | asset_url: 'versioned' }}
<!-- /assets/css/style.css?v=abc123 -->

<!-- Image URLs with size variants -->
{{ product.image | img_url: 'thumb' }}
<!-- /assets/images/product-image-100x100.jpg -->

{{ product.image | img_url: 'large' }}
<!-- /assets/images/product-image-1200x1200.jpg -->

<!-- Page URL generation from page names -->
{{ 'kontakt' | page_url: current_language }}
<!-- /kontakt/ (or /en/contact/ for multilingual) -->

{{ 'o-nas' | page_url }}
<!-- Uses current_language automatically -->

<!-- Section URL generation for blog and categories -->
{{ 'blog' | section_url: current_language }}
<!-- /blog/ -->

{{ 'elektronika' | section_url: current_language }}
<!-- /elektronika/ (or /en/electronics/) -->

<!-- Legacy: Direct URL strings (still supported) -->
{{ '/kontakt' | url: current_language }}
<!-- /kontakt -->

<!-- Locale-aware URLs -->
{{ page.url | url: 'en' }}
<!-- /en/page-url/ -->
```

### Money Filters
```liquid
<!-- Price formatting -->
{{ product.price | money: 'USD' }}
<!-- $29.99 -->

{{ product.price | money: 'CZK' }}
<!-- 599 Kč -->

<!-- With custom formatting -->
{{ product.price | money: 'EUR', decimal_separator: '.', thousands_separator: ',' }}
<!-- 29,99 € -->
```

### Text Filters
```liquid
<!-- Truncate words -->
{{ product.description | truncate_words: 20 }}

<!-- Strip HTML -->
{{ product.content | strip_html }}

<!-- Slugify for URLs -->
{{ category.name | slugify }}
<!-- "electronics-accessories" -->

<!-- HTML escape -->
{{ user_input | escape }}
```

### Date Filters
```liquid
<!-- Localized date formatting -->
{{ order.created_at | date: '%d.%m.%Y' }}
<!-- 15.03.2024 -->

{{ post.date | date: '%B %d, %Y' }}
<!-- March 15, 2024 -->
```

### Math Filters
```liquid
<!-- Basic math operations -->
{{ product.price | plus: shipping_cost }}
{{ product.price | minus: discount }}
{{ product.price | times: quantity }}
{{ product.price | divided_by: 2 }}

<!-- Rounding -->
{{ 99.99 | round: 0 }}
<!-- 100 -->
```

### Translation Filters
```liquid
<!-- Basic translation -->
{{ 'Add to Cart' | t: current_language }}

<!-- Translation with parameters -->
{{ 'Welcome back, %{name}!' | t: current_language, name: customer.name }}

<!-- Pluralization -->
{{ cart_count | pluralize: 'item', 'items' }}
<!-- "1 item" or "5 items" -->
```

## 🌍 Multilingual Templates

### Language-specific Templates
```
templates/
├── cs-cz/
│   ├── layout/
│   │   └── base.liquid
│   └── component/
│       └── header.liquid
├── en-us/
│   ├── layout/
│   │   └── base.liquid
│   └── component/
│       └── header.liquid
```

### Translation Context
```liquid
<!-- Current language data -->
{{ current_language }}          <!-- "cs-cz" -->
{{ current_language_data.name }} <!-- "Čeština" -->
{{ current_language_data.short }}<!-- "cs" -->
{{ current_language_data.currency }} <!-- "CZK" -->

<!-- Language switching -->
{% for lang_code, language in site.languages %}
  <a href="{{ page.url | url: lang_code }}">
    {{ language.name }}
  </a>
{% endfor %}

<!-- Conditional content by language -->
{% if current_language == 'cs-cz' %}
  <p>Czech-specific content</p>
{% elsif current_language == 'en-us' %}
  <p>English-specific content</p>
{% endif %}
```

## 🎯 Template Best Practices

### Performance Optimization
```liquid
<!-- ✅ Lazy load images -->
<img src="{{ product.image | img_url: 'thumb' }}" 
     loading="lazy" 
     alt="{{ product.title }}">

<!-- ✅ Optimize critical CSS -->
<style>
  /* Critical above-the-fold styles */
</style>
<link rel="preload" href="{{ 'css/styles.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">

<!-- ✅ Minimize JavaScript -->
<script src="{{ 'js/app.js' | asset_url }}" defer></script>
```

### SEO Best Practices
```liquid
<!-- ✅ Semantic HTML -->
<article itemscope itemtype="https://schema.org/Product">
  <h1 itemprop="name">{{ product.title }}</h1>
  <meta itemprop="sku" content="{{ product.sku }}">
  
  <div itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <meta itemprop="price" content="{{ product.price }}">
    <meta itemprop="priceCurrency" content="{{ current_language_data.currency }}">
    <meta itemprop="availability" content="{% if product.stock > 0 %}InStock{% else %}OutOfStock{% endif %}">
  </div>
</article>

<!-- ✅ Breadcrumbs -->
<nav aria-label="Breadcrumb" itemscope itemtype="https://schema.org/BreadcrumbList">
  <ol class="breadcrumb">
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <a itemprop="item" href="/"><span itemprop="name">Home</span></a>
      <meta itemprop="position" content="1">
    </li>
  </ol>
</nav>
```

### Accessibility
```liquid
<!-- ✅ Alt text for images -->
<img src="{{ product.image | img_url }}" 
     alt="{% if product.image.alt %}{{ product.image.alt }}{% else %}{{ product.title }}{% endif %}">

<!-- ✅ ARIA labels -->
<button aria-label="{{ 'Add' | t: current_language }} {{ product.title }} {{ 'to cart' | t: current_language }}"
        class="add-to-cart-btn">
  {{ 'Add to Cart' | t: current_language }}
</button>

<!-- ✅ Focus management -->
<nav aria-label="{{ 'Main navigation' | t: current_language }}">
  <ul class="nav-list">
    {% for item in navigation.main %}
      <li><a href="{{ item.url }}" {% if item.current %}aria-current="page"{% endif %}>{{ item.title }}</a></li>
    {% endfor %}
  </ul>
</nav>
```

### Error Handling
```liquid
<!-- ✅ Defensive coding -->
{% if product %}
  {% if product.images.size > 0 %}
    <img src="{{ product.images[0] | img_url }}" alt="{{ product.title }}">
  {% else %}
    <div class="no-image">{{ 'No image available' | t: current_language }}</div>
  {% endif %}
{% endif %}

<!-- ✅ Fallback values -->
<title>{{ page.seo_title | default: page.title | default: 'Untitled' }}</title>

<!-- ✅ Safe array access -->
{% assign first_image = product.images[0] %}
{% if first_image %}
  <img src="{{ first_image | img_url }}" alt="{{ product.title }}">
{% endif %}
```

## 🔧 Custom Template Development

### Creating a Custom Theme

1. **Create theme directory**
```bash
mkdir -p my-theme/layout
mkdir -p my-theme/component  
mkdir -p my-theme/assets/css
mkdir -p my-theme/assets/js
```

2. **Base layout** (`layout/base.liquid`)
```liquid
<!DOCTYPE html>
<html>
<head>
  <title>{{ page.title }} - {{ site.name }}</title>
  <link rel="stylesheet" href="{{ 'css/theme.css' | asset_url }}">
</head>
<body>
  {{ content }}
</body>
</html>
```

3. **Configure in site.json**
```json
{
  "template": "my-theme"
}
```

### Template Inheritance
```liquid
<!-- child-template.liquid -->
---
extends: layout/base.liquid
---

{% block main_content %}
  <div class="custom-content">
    {{ content }}
  </div>
{% endblock %}

{% block sidebar %}
  <aside class="sidebar">
    <!-- Custom sidebar -->
  </aside>
{% endblock %}
```

### Template Testing
```javascript
// __tests__/templates/template.test.js
const { TemplateEngine } = require('../../src/templates/TemplateEngine');

describe('Template Rendering', () => {
  test('should render product template correctly', async () => {
    const engine = new TemplateEngine();
    const template = await fs.readFile('templates/product.liquid', 'utf-8');
    
    const result = await engine.render(template, {
      product: {
        title: 'Test Product',
        price: 999,
        images: ['test.jpg']
      }
    });
    
    expect(result).toContain('Test Product');
    expect(result).toContain('999');
  });
});
```

---

This comprehensive template system provides flexibility, performance, and extensibility for creating custom e-commerce experiences with GitCart.