# GitCart - User Guide

Complete guide for using the GitCart static e-shop generator.

## 🚀 Getting Started

### System Requirements

- Node.js 18+ 
- npm 8+
- Git (recommended)
- Text editor (VS Code, Sublime Text, etc.)

### First Project Step by Step

#### 1. GitCart CLI Installation

```bash
# Global installation (recommended)
npm install -g gitcart-cli

# Verify installation
gitcart --version
```

#### 2. Creating a New Project

```bash
# Interactive creation
gitcart init my-shop

# Or with parameters
gitcart init my-shop --template default --language en-us
```

#### 3. Basic Configuration

Edit the `site.json` file:

```json
{
  "name": "My E-shop",
  "url": "https://my-shop.com",
  "description": "Quality products at great prices",
  "template_engine": "liquid",
  "url_strategy": "default_clean",
  "url_locale_format": "short",
  
  "languages": {
    "en-us": {
      "name": "English", 
      "default": true, 
      "short": "en", 
      "currency": "USD"
    }
  },
  
  "pagination": {
    "products_per_page": 12,
    "max_pagination_pages": 10
  },
  
  "variables": {
    "contact": {
      "email": "info@my-shop.com",
      "phone": "+1 555 123 456",
      "address": "123 Main Street, New York, NY"
    },
    "social": {
      "facebook": "https://facebook.com/my-shop",
      "instagram": "@my_shop"
    },
    "navigation": {
      "header": [
        {"page": "about-us"},
        {"page": "contact"}
      ],
      "footer": {
        "left": [
          {"page": "contact"},
          {"page": "about-us"}
        ],
        "right": [
          {"page": "shipping-payment"},
          {"page": "terms-conditions"}
        ]
      }
    }
  }
}
```

## 📁 Content Management

### Creating Categories

#### CLI Method (recommended)
```bash
gitcart category add
```

The interactive wizard will ask for:
- Category name
- URL slug 
- Category description
- Parent category
- Language
- Whether to create a sample product

#### Manual Creation

1. Create folder: `en-us/categories/electronics/`
2. Create file `electronics.md`:

```markdown
---
title: Electronics
description: Latest electronic devices
sort_order: 1
featured: true
banner_image: electronics-banner.jpg
seo_title: "Electronics - Best Prices in US"
seo_description: "Buy quality electronics at great prices. Phones, tablets, computers and more."
---

# Electronics

Welcome to our electronics section! You'll find the latest:

## 📱 Mobile Phones
- iPhone, Samsung, Google Pixel
- Latest models at great prices
- Warranty and service

## 💻 Computers and Tablets
- Laptops for work and gaming
- Tablets for everyday use
- Accessories and peripherals

## 🎧 Audio Equipment
- Headphones and speakers
- Hi-Fi devices
- Home theaters
```

### Creating Products

#### CLI Method
```bash
gitcart product add
```

#### Manual Creation

1. Create file in category: `en-us/categories/electronics/.products/iphone-15.md`

```markdown
---
title: iPhone 15
price: 999
sale_price: 899
sku: IPHONE15-128-BLUE
stock: 15
variant: blue-128gb
featured: true
tags: 
  - smartphone
  - apple
  - 5g
images: 
  - iphone-15-blue-front.jpg
  - iphone-15-blue-back.jpg
  - iphone-15-gallery-1.jpg
  - iphone-15-gallery-2.jpg
specifications:
  display: "6.1\" Super Retina XDR"
  processor: "A17 Bionic chip"
  storage: "128GB"
  camera: "48MP Main + 12MP Ultra Wide"
  battery: "Up to 20 hours video playback"
  connectivity: "5G, Wi-Fi 6, Bluetooth 5.3"
seo_title: "iPhone 15 128GB Blue - Best Price in US"
seo_description: "iPhone 15 with 128GB storage in blue color. A17 Bionic chip, USB-C, 48MP camera. Free shipping!"
---

# iPhone 15 - A New Era of Innovation

The iPhone 15 represents a significant leap forward in smartphone technology. With the revolutionary USB-C connector, powerful A17 Bionic chip, and advanced camera system, you get a device that redefines mobile technology.

## 🌟 Key Features

### A17 Bionic Chip
- Most advanced chip in iPhone history
- 6-core CPU with 2 performance and 4 efficiency cores
- 5-core GPU for great gaming performance
- 16-core Neural Engine for AI features

### Advanced Camera System
- **48MP Main camera** with f/1.78 aperture
- **12MP Ultra Wide** with f/2.4 aperture and 120° field of view
- **12MP TrueDepth** front camera
- Portrait mode with background blur
- 4K Dolby Vision HDR recording

### USB-C Connectivity
- Universal USB-C port
- Fast charging and data transfer
- Compatibility with wide range of accessories

### Battery Life
- Up to 20 hours of video playback
- Up to 16 hours of video streaming
- Fast charging - 50% in 30 minutes with 20W adapter

## 📱 Technical Specifications

| Feature | Value |
|---------|--------|
| **Display** | 6.1" Super Retina XDR OLED |
| **Resolution** | 2556 × 1179 px, 460 ppi |
| **Processor** | A17 Bionic |
| **Storage** | 128GB |
| **Memory** | 6GB RAM |
| **Main Camera** | 48MP f/1.78 + 12MP f/2.4 |
| **Front Camera** | 12MP f/1.9 |
| **Video** | 4K Dolby Vision HDR |
| **Connectivity** | 5G, Wi-Fi 6E, Bluetooth 5.3 |
| **Water Resistance** | IP68 |
| **Dimensions** | 147.6 × 71.6 × 7.80 mm |
| **Weight** | 171 g |

## 🎨 Available Colors

- **Blue** (current selection)
- Pink
- Yellow
- Green
- Black

## 📦 Box Contents

- iPhone 15
- USB-C to USB-C cable
- Documentation

*Power adapter sold separately*

## 🛡️ Warranty and Service

- **2 years hardware warranty**
- **90 days free support**
- Authorized Apple service in US
- AppleCare+ available for extended coverage
```

2. Add images to folder: `en-us/categories/electronics/.images/`

### Creating Static Pages

#### CLI Method
```bash
gitcart page add
```

#### Manual Creation

Create file `en-us/pages/contact.md`:

```markdown
---
title: Contact Us
description: Our contact information and details
nav_footer: true
nav_order: 1
icon: phone
show_map: true
custom_class: contact-page
seo_title: "Contact - My E-shop"
seo_description: "Contact information, opening hours, and map to our store."
---

# Contact Us

## 📍 Find Us

**Business Address:**
123 Main Street
New York, NY 10001
United States

## 📞 Contact Information

- **Phone:** +1 555 123 456
- **Email:** info@my-shop.com
- **Customer Service:** 800 123 456 (toll-free)

## 🕐 Opening Hours

| Day | Opening Hours |
|-----|---------------|
| Monday - Friday | 9:00 AM - 6:00 PM |
| Saturday | 9:00 AM - 3:00 PM |
| Sunday | Closed |

## 💬 Have a Question?

Contact us and we'll get back to you within 24 hours!
```

## 🛠 Development and Build

### Development Server
```bash
# Start with live reload
gitcart dev --watch

# Specific port
gitcart dev --port 3000 --watch
```

### Production Build
```bash
# Basic build
gitcart build

# Build with detailed information
gitcart build --verbose

# Build with optimizations
gitcart build --minify --optimize-images
```

### Output Structure
```
dist/
├── index.html                 # Homepage
├── electronics/               # Category pages
│   ├── index.html
│   ├── 2/index.html          # Pagination
│   └── iphone-15/
│       └── index.html
├── contact/
│   └── index.html            # Static pages
├── assets/
│   ├── css/styles.css
│   ├── js/
│   │   ├── gitcart.js        # Core functionality
│   │   └── plugins/          # Plugin files
│   ├── images/               # Optimized images
│   └── data/
│       └── search-index.json # Search data
├── sitemap.xml
└── robots.txt
```

## 🌐 Multilingual Support

### Language Configuration

In `site.json`:

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
    "fr-fr": {
      "name": "Français",
      "short": "fr",
      "currency": "EUR"
    }
  },
  "url_strategy": "default_clean",
  "url_locale_format": "short"
}
```

### URL Strategies

- `"always_full"`: `/en-us/electronics/iphone-15/`
- `"always_short"`: `/en/electronics/iphone-15/`
- `"default_clean"`: `/electronics/iphone-15/` (default lang), `/es/electronics/iphone-15/` (others)
- `"default_clean_full"`: `/electronics/iphone-15/` (default), `/es-es/electronics/iphone-15/` (others)

### Creating Multilingual Content

1. **Folders by locales:**
```
en-us/                     # English (default)
├── categories/
└── pages/

es-es/                     # Spanish
├── categories/
│   └── electronica/       # Translated category names
└── pages/

fr-fr/                     # French
├── categories/
└── pages/
```

2. **Navigation translations:**
```json
{
  "variables": {
    "navigation": {
      "header": {
        "en-us": [
          {"page": "about-us", "title": "About Us"},
          {"page": "contact", "title": "Contact"}
        ],
        "es-es": [
          {"page": "acerca-de", "title": "Acerca de"},
          {"page": "contacto", "title": "Contacto"}
        ]
      }
    }
  }
}
```

## 🔌 Plugin System

### Installing Plugins
```bash
# From GitCart repository
gitcart plugin add email-notifications
gitcart plugin add google-analytics
gitcart plugin add facebook-pixel

# Local plugin
gitcart plugin add ./plugins/custom-shipping.js
```

### Creating Custom Plugin
```bash
gitcart plugin create my-plugin
```

### Example Plugin - Email Notifications

`plugins/email-notifications.js`:

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'Email Notifications',
    version: '1.0.0',
    apiKey: 'your-mailgun-api-key',
    domain: 'your-mailgun-domain.com',
    toEmail: 'orders@my-shop.com'
  };

  const EmailPlugin = {
    init() {
      console.log(`${CONFIG.name} plugin loaded`);
    },

    async sendOrderEmail(orderData) {
      const formData = new FormData();
      formData.append('from', `E-shop <noreply@${CONFIG.domain}>`);
      formData.append('to', CONFIG.toEmail);
      formData.append('subject', `New order ${orderData.id}`);
      formData.append('text', this.buildEmailText(orderData));

      try {
        await fetch(`https://api.mailgun.net/v3/${CONFIG.domain}/messages`, {
          method: 'POST',
          headers: {
            'Authorization': 'Basic ' + btoa('api:' + CONFIG.apiKey)
          },
          body: formData
        });
      } catch (error) {
        console.error('Email send failed:', error);
      }
    },

    buildEmailText(orderData) {
      return `
New order: ${orderData.id}
Customer: ${orderData.customer.firstName} ${orderData.customer.lastName}
Email: ${orderData.customer.email}
Phone: ${orderData.customer.phone}

Total amount: ${orderData.totals.total} ${orderData.currency}

Products:
${orderData.items.map(item => 
  `- ${item.title} x${item.quantity} = ${item.subtotal} ${orderData.currency}`
).join('\n')}

Shipping: ${orderData.shipping.methodName} (${orderData.shipping.price} ${orderData.currency})
Payment: ${orderData.payment.methodName}

Delivery address:
${orderData.shipping.address.street}
${orderData.shipping.address.city}, ${orderData.shipping.address.zip}
${orderData.shipping.address.country}
      `;
    }
  };

  // Auto-registration
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('email-notifications', {
      config: CONFIG,
      
      events: {
        'gitcart:checkout:completed': EmailPlugin.sendOrderEmail.bind(EmailPlugin)
      },

      render: {
        'order-success-info': function(context) {
          return '<div class="alert alert-success">Order confirmation sent to your email!</div>';
        }
      },

      init: EmailPlugin.init
    });
  }
})();
```

### Rendering Zones

Available zones in templates:

**Global zones:**
- `head` - Meta tags, analytics
- `body-start` - Tracking codes
- `header-end` - After header
- `before-footer` - Before footer
- `footer-end` - After footer  
- `body-end` - Scripts at end

**Product zones:**
- `product-images-start/end` - Around product images
- `product-details-end` - After product details
- `product-before-description` - Before description
- `product-after-description` - After description
- `product-end` - At end of product

**Category zones:**
- `category-start/end` - Beginning/end of category
- `category-after-header` - After category header
- `category-before-products` - Before product list

## 🔍 Search and Filters

### Search Configuration

GitCart uses Fuse.js for client-side search:

```json
{
  "search": {
    "enabled": true,
    "min_query_length": 2,
    "max_results": 50,
    "fuse_options": {
      "threshold": 0.3,
      "includeScore": true,
      "keys": [
        "title",
        "description", 
        "tags",
        "category"
      ]
    }
  }
}
```

### Frontend API

```javascript
// Search products
const results = await GitCart.search.query("iphone", {
  category: "electronics",
  price: { min: 100, max: 1000 },
  tags: ["smartphone"]
});

// Apply filters
GitCart.search.filter({
  category: "electronics",
  price: { min: 50, max: 500 }
});

// Sort results  
GitCart.search.sort("price", "asc");
```

## 🛒 E-commerce Features

### GitCart.js API

#### Shopping Cart
```javascript
// Add to cart
GitCart.cart.add('IPHONE15-128', 1, {
  title: 'iPhone 15',
  price: 999,
  image: 'iphone-front.jpg',
  variant: 'blue-128gb'
});

// Remove from cart
GitCart.cart.remove('IPHONE15-128');

// Update quantity
GitCart.cart.updateQuantity('IPHONE15-128', 2);

// Get items
const items = GitCart.cart.getItems();
const count = GitCart.cart.getItemCount();
const total = GitCart.cart.getTotal();

// Clear cart
GitCart.cart.clear();
```

#### Checkout Process
```javascript
// Start checkout
GitCart.checkout.start();

// Set customer data
GitCart.checkout.setCustomer({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john@example.com',
  phone: '+1555123456'
});

// Choose shipping
GitCart.checkout.setShipping('ups', {
  street: '123 Main Street',
  city: 'New York',
  zip: '10001',
  country: 'US'
});

// Choose payment
GitCart.checkout.setPayment('card');

// Complete order
const order = await GitCart.checkout.complete();
```

#### Event Handling
```javascript
// Listen to events
GitCart.on('gitcart:cart:item:added', function(data) {
  console.log('Added to cart:', data);
  showNotification('Product added to cart!');
});

GitCart.on('gitcart:checkout:completed', function(orderData) {
  console.log('Order completed:', orderData);
  // Redirect to thank you page
  window.location.href = '/thank-you/?order=' + orderData.id;
});
```

## 📊 SEO Optimization

### Automatic SEO
GitCart automatically generates:
- Meta tags based on frontmatter
- Structured data (JSON-LD)
- Sitemap.xml
- Robots.txt
- Canonical URLs
- OpenGraph tags

### Manual Optimization

In products and pages:
```markdown
---
title: iPhone 15 128GB Blue
seo_title: "iPhone 15 128GB Blue - Best Price in US"
seo_description: "iPhone 15 with 128GB storage in blue color. A17 Bionic chip, USB-C, 48MP camera. Free shipping on orders over $100!"
seo_keywords: "iphone 15, apple, smartphone, 128gb, blue"
og_image: "iphone-15-blue-og.jpg"
og_type: "product"
---
```

### Schema.org Markup

Automatically generated for products:
```json
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "iPhone 15",
  "image": "/assets/images/iphone-15-blue-front.jpg",
  "description": "iPhone 15 with revolutionary USB-C connector...",
  "sku": "IPHONE15-128-BLUE",
  "brand": {
    "@type": "Brand",
    "name": "Apple"
  },
  "offers": {
    "@type": "Offer",
    "price": "899",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock"
  }
}
```

## 🚀 Deployment

### GitHub/GitLab Pages

1. **Push to repository:**
```bash
git add .
git commit -m "Initial e-shop setup"
git push origin main
```

2. **GitHub Actions workflow** (`.github/workflows/deploy.yml`):
```yaml
name: Deploy GitCart Site

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        
    - name: Install GitCart CLI
      run: npm install -g gitcart-cli
      
    - name: Build site
      run: gitcart build --optimize-images
      
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

### Netlify

1. **Connect repository to Netlify**
2. **Build settings:**
   - Build command: `npm install -g gitcart-cli && gitcart build`
   - Publish directory: `dist`
3. **Environment variables:**
   - NODE_VERSION: `18`

### Vercel

```json
{
  "buildCommand": "npm install -g gitcart-cli && gitcart build",
  "outputDirectory": "dist",
  "installCommand": "npm install"
}
```

## 🛠 Troubleshooting

### Common Errors

#### "Template not found"
```bash
# Check template_engine in site.json
# Verify template files exist
ls templates/default/layouts/
```

#### "SKU must be unique"
```bash
# Find duplicate SKUs
gitcart validate --check-sku
```

#### "Missing required field"
```bash
# Validate content
gitcart validate --verbose
```

### Debug Mode
```bash
# Detailed output during build
gitcart build --verbose --debug

# Check before build
gitcart validate --fix-auto
```

### Performance Optimization

#### Images
```bash
# Optimize images during build
gitcart build --optimize-images

# Generate WebP formats
gitcart build --webp
```

#### CSS/JS
```bash
# Minify assets
gitcart build --minify

# Remove unused CSS classes
gitcart build --purge-css
```

## 📞 Support

### Documentation
- [Official documentation](https://gitcart.dev/docs)
- [Template reference](https://gitcart.dev/docs/templates)
- [Plugin API](https://gitcart.dev/docs/plugins)

### Community
- [GitHub Issues](https://github.com/gitcart/gitcart/issues)
- [Discord server](https://discord.gg/gitcart)
- [Forum](https://forum.gitcart.dev)

### Support
- Email: support@gitcart.dev
- Documentation: docs.gitcart.dev
- Examples: examples.gitcart.dev