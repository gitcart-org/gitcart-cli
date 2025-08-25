# Plugin Zones - Developer Guide

Plugin Zones are predefined locations in GitCart templates where plugins can inject their content. This system allows easy extension of e-shop functionality without modifying templates.

## How zones work

Zones are implemented as empty `<div>` elements with `zone-` prefix in ID:

```html
<div id="zone-after-header"></div>
```

Plugins can then inject content into these zones using the `render` section:

```javascript
GitCart.plugin('my-plugin', {
  render: {
    'zone-after-header': function(context) {
      return '<div class="banner">Special offer!</div>';
    }
  }
});
```

## Available zones

### 🏗️ Layout zones (available on all pages)

| Zone | Description | Perfect for |
|------|-------------|-------------|
| `zone-head` | In `<head>` section (**server-side only**) | Meta tags, analytics |
| `zone-body-start` | After opening `<body>` tag | Cookie bars, loading indicators |
| `zone-after-header` | After header/navigation | Banners, announcements |
| `zone-after-main` | After main content | Newsletters, ads |
| `zone-before-footer` | Before footer | CTA sections, links |
| `zone-footer-start` | In footer content | Additional information |
| `zone-after-footer` | After footer | Tracking codes |
| `zone-body-end` | Before closing `</body>` | Bottom scripts |

### 🛍️ Product zones

| Zone | Description | Perfect for |
|------|-------------|-------------|
| `zone-product-start` | Beginning of product page | Breadcrumbs, tracking |
| `zone-after-product-images` | After image gallery | Zoom buttons, lightbox |
| `zone-after-product-price` | After price display | Sale badges, price alerts |
| `zone-after-add-to-cart` | After "add to cart" button | Cross-sell, related products |
| `zone-after-product-description` | After product description | Reviews, Q&A, specifications |
| `zone-product-end` | End of product page | Tracking, more products |

### 📂 Category zones

| Zone | Description | Perfect for |
|------|-------------|-------------|
| `zone-category-start` | Beginning of category page | Breadcrumbs, tracking |
| `zone-after-category-header` | After category header | Filters, sorting UI |
| `zone-before-products` | Before product listing | Banners, ads |
| `zone-after-products` | After product listing | Load more buttons, info |
| `zone-category-end` | End of category page | Related categories |
| `zone-product-card-actions` | In product cards | Wishlist, compare buttons |

### 🛒 Cart and checkout

| Zone | Description | Perfect for |
|------|-------------|-------------|
| `zone-cart-summary` | In cart summary | Shipping info, discounts |
| `zone-cart-before-checkout` | Before checkout button | Coupon codes, terms |
| `zone-shipping-methods` | Shipping options | Additional carriers |
| `zone-payment-methods` | Payment options | New payment gateways |

### 🏠 Homepage

| Zone | Description | Perfect for |
|------|-------------|-------------|
| `zone-homepage-start` | Beginning of homepage | Tracking, cookie consent |
| `zone-after-hero` | After hero section | Special offers |
| `zone-before-featured-products` | Before featured products | Category menu |
| `zone-homepage-end` | End of homepage | Newsletter, social links |

### 🔍 Search and other

| Zone | Description | Perfect for |
|------|-------------|-------------|
| `zone-before-search-results` | Before search results | Filters, sorting |
| `zone-after-search-results` | After search results | Related content, ads |
| `zone-newsletter-signup` | Newsletter signup area | Email forms |

## Context Object

Each zone function receives a `context` parameter with current page information:

```javascript
const context = {
  // Current page information
  page: {
    type: 'product',           // 'product', 'category', 'homepage', 'checkout-success', etc.
    url: '/electronics/iphone-15/',
    title: 'iPhone 15',
    lang: 'en'
  },
  
  // Site information
  site: {
    name: 'My E-shop',
    url: 'https://myshop.com'
  },
  
  // Shopping cart (if available)
  cart: {
    items: [...],
    count: 3,
    total: 599.99
  },
  
  // Current product (only on product pages)
  product: {
    sku: 'IPHONE15-128',
    title: 'iPhone 15',
    price: 599.99,
    // ... other product data
  },
  
  // Current category (only on category pages)
  category: {
    slug: 'electronics',
    title: 'Electronics',
    // ... other category data
  }
};
```

## Practical usage examples

### 1. Google Analytics plugin

```javascript
GitCart.plugin('google-analytics', {
  config: {
    trackingId: 'GA_MEASUREMENT_ID'
  },
  
  render: {
    'zone-head': function(context) {
      return `
        <!-- Google Analytics -->
        <script async src="https://www.googletagmanager.com/gtag/js?id=${this.config.trackingId}"></script>
        <script>
          window.dataLayer = window.dataLayer || [];
          function gtag(){dataLayer.push(arguments);}
          gtag('js', new Date());
          gtag('config', '${this.config.trackingId}');
        </script>
      `;
    }
  },
  
  events: {
    'gitcart:product:viewed': function(data) {
      // Track product views
      gtag('event', 'view_item', {
        currency: 'USD',
        value: data.price,
        item_id: data.sku
      });
    }
  }
});
```

### 2. Product Reviews plugin

```javascript
GitCart.plugin('product-reviews', {
  render: {
    'zone-after-product-description': function(context) {
      // Only on product pages
      if (!context.product) return '';
      
      return `
        <div class="product-reviews">
          <h3>Customer Reviews</h3>
          <div class="reviews-list">
            <div class="review">
              <div class="rating">⭐⭐⭐⭐⭐</div>
              <p>"Great product, highly recommend!"</p>
              <small>- John Doe</small>
            </div>
          </div>
          <button onclick="showReviewForm()">Add Review</button>
        </div>
      `;
    }
  },
  
  events: {
    'gitcart:product:viewed': function(data) {
      // Load reviews for this product
      this.loadReviews(data.sku);
    }
  }
});
```

### 3. Cart Discount plugin

```javascript
GitCart.plugin('cart-discounts', {
  render: {
    'zone-cart-summary': function(context) {
      // Only on cart page
      if (!context.cart) return '';
      
      const total = context.cart.total || 0;
      
      if (total > 50) {
        return '<div class="discount-info">🎉 Free shipping on orders over $50!</div>';
      } else {
        const remaining = 50 - total;
        return `<div class="discount-info">📦 Add $${remaining} more for free shipping</div>`;
      }
    },
    
    'zone-cart-before-checkout': function(context) {
      return `
        <div class="coupon-section">
          <label>Coupon code:</label>
          <input type="text" id="coupon-code" placeholder="Enter coupon code">
          <button onclick="applyCoupon()">Apply</button>
        </div>
      `;
    }
  },
  
  events: {
    'gitcart:cart:updated': function(data) {
      // Re-render zones when cart changes
      GitCart.reRenderZones(['zone-cart-summary']);
    }
  }
});
```

### 4. Newsletter Signup plugin

```javascript
GitCart.plugin('newsletter', {
  render: {
    'zone-newsletter-signup': function(context) {
      return `
        <div class="newsletter-form">
          <h4>Don't miss our updates!</h4>
          <form onsubmit="return subscribeNewsletter(event)">
            <input type="email" placeholder="Your email" required>
            <button type="submit">Subscribe</button>
          </form>
        </div>
      `;
    },
    
    'zone-after-hero': function(context) {
      // Newsletter popup on homepage
      if (context.page && context.page.type === 'homepage') {
        return `
          <div class="newsletter-popup" id="newsletter-popup" style="display:none;">
            <div class="popup-content">
              <h3>Get 10% off!</h3>
              <p>Subscribe to our newsletter and get a discount on your first order.</p>
              <form onsubmit="return subscribeNewsletter(event, true)">
                <input type="email" placeholder="Your email" required>
                <button type="submit">Get Discount</button>
              </form>
              <button onclick="closeNewsletterPopup()">×</button>
            </div>
          </div>
        `;
      }
      return '';
    }
  }
});

function subscribeNewsletter(event, withDiscount = false) {
  event.preventDefault();
  // Newsletter subscription logic
  if (withDiscount) {
    alert('Thank you! Your discount code: WELCOME10');
    closeNewsletterPopup();
  } else {
    alert('Thank you for subscribing!');
  }
  return false;
}
```

## Creating custom zones

If you need custom zones, simply add them to your templates:

```html
<!-- In template -->
<div class="my-custom-section">
  <h2>My Section</h2>
  <div id="zone-my-custom-zone"></div>
</div>
```

Then use them in plugins:

```javascript
GitCart.plugin('my-custom-plugin', {
  render: {
    'zone-my-custom-zone': function(context) {
      return '<p>Custom content!</p>';
    }
  }
});
```

## Server-side vs Client-side

### Server-side zones
- **Only `zone-head`** - processed during build
- Use `render` section in plugin
- Perfect for critical content (meta tags, analytics)

### Client-side zones
- **All other zones** - processed in browser
- Use `render` section in plugin
- More flexible, responds to user actions
- Have access to `context` object with page data

## Tips and best practices

1. **Use the `context` object** - contains page data (product, category, cart...)
2. **Check context** - return empty string if zone is not relevant
3. **Use CSS classes** for styling instead of inline styles
4. **Test on mobile devices** - some zones might be hidden
5. **Cache render functions** - store complex calculations in variables
6. **Watch performance** - complex render functions can slow down page loading

More information in [complete plugin documentation](./plugins.md).