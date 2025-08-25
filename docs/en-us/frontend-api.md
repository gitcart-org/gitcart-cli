# GitCart - Frontend API

Complete reference guide for GitCart.js frontend API.

## 🎯 Frontend API Overview

GitCart.js provides complete JavaScript API for e-commerce functionality in the browser. The API is designed as a lightweight, modular and easily extensible library.

### Key Features

- **Plug & Play** - No configuration, works out-of-the-box
- **localStorage persistence** - Cart and data survive page reload
- **Event-driven** - Modular architecture with events
- **Plugin system** - Easily extensible functionality
- **TypeScript ready** - Complete type definitions
- **Lightweight** - < 15kb minified + gzipped

## 🚀 Initialization and Basic Usage

### Automatic Loading

GitCart.js is automatically loaded in all GitCart templates:

```html
<!-- Automatically injected into <head> -->
<script src="/assets/js/gitcart.js"></script>

<script>
// GitCart is available globally
console.log(GitCart.version); // "1.0.0"

// Automatic initialization on DOMContentLoaded
document.addEventListener('DOMContentLoaded', function() {
  // GitCart is ready to use
  console.log('Cart has', GitCart.cart.getItemCount(), 'items');
});
</script>
```

### Manual Initialization

```javascript
// Manual initialization with configuration
GitCart.init({
  currency: 'USD',
  locale: 'en-us',
  debug: true,
  
  // E-commerce settings
  cart: {
    persistent: true,
    storageKey: 'gitcart_cart'
  },
  
  // Checkout settings
  checkout: {
    requireEmail: true,
    requirePhone: true,
    validateAddress: true
  }
});
```

## 🛒 Cart API

Shopping cart management with localStorage persistence.

### `GitCart.cart.add(sku, quantity, options)`

Adds a product to the cart.

```javascript
// Basic product addition
GitCart.cart.add('IPHONE15-128', 1);

// With additional options
GitCart.cart.add('IPHONE15-128', 2, {
  variant: 'blue-128gb',
  customization: 'engraving',
  note: 'Christmas gift',
  giftWrap: true
});

// With custom price (sale price)
GitCart.cart.add('IPHONE15-128', 1, {
  price: 899, // Overrides default price
  originalPrice: 999
});
```

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `sku` | string | ✅ | Unique product identifier |
| `quantity` | number | - | Quantity (default: 1) |
| `options` | object | - | Additional options |

#### Options

```javascript
{
  variant: 'string',          // Product variant
  price: number,             // Custom price
  originalPrice: number,     // Original price for discounts
  title: 'string',          // Product title (fallback)
  image: 'string',          // Image URL
  url: 'string',            // Product URL
  category: 'string',       // Category
  customization: 'object',  // Customization
  note: 'string',           // Note
  giftWrap: boolean,        // Gift wrapping
  metadata: object          // Custom metadata
}
```

### `GitCart.cart.remove(sku, options)`

Removes a product from the cart.

```javascript
// Remove product
GitCart.cart.remove('IPHONE15-128');

// Remove specific variant
GitCart.cart.remove('IPHONE15-128', { variant: 'blue-128gb' });
```

### `GitCart.cart.updateQuantity(sku, quantity, options)`

Updates product quantity.

```javascript
// Change quantity
GitCart.cart.updateQuantity('IPHONE15-128', 3);

// For specific variant
GitCart.cart.updateQuantity('IPHONE15-128', 2, { variant: 'blue-128gb' });

// Setting to 0 = removal
GitCart.cart.updateQuantity('IPHONE15-128', 0);
```

### `GitCart.cart.clear()`

Clears entire cart.

```javascript
// Clear cart
GitCart.cart.clear();

// With confirmation
if (confirm('Are you sure you want to clear the cart?')) {
  GitCart.cart.clear();
}
```

### `GitCart.cart.getItems()`

Returns all items in the cart.

```javascript
const items = GitCart.cart.getItems();
console.log(items);

// Output:
[
  {
    sku: 'IPHONE15-128',
    title: 'iPhone 15 128GB',
    price: 899,
    originalPrice: 999,
    quantity: 1,
    variant: 'blue-128gb',
    subtotal: 899,
    image: '/assets/images/iphone-15.jpg',
    url: '/electronics/iphone-15/',
    category: 'electronics',
    addedAt: '2024-01-15T10:30:00Z',
    options: {
      giftWrap: true,
      note: 'Christmas gift'
    }
  },
  // ... more items
]
```

### `GitCart.cart.getItem(sku, options)`

Returns specific item from cart.

```javascript
// Get item
const item = GitCart.cart.getItem('IPHONE15-128');

// With variant
const item = GitCart.cart.getItem('IPHONE15-128', { variant: 'blue-128gb' });

if (item) {
  console.log('Item price:', item.price);
  console.log('Item quantity:', item.quantity);
}
```

### `GitCart.cart.hasItem(sku, options)`

Checks if product is in cart.

```javascript
// Check if item exists
if (GitCart.cart.hasItem('IPHONE15-128')) {
  console.log('iPhone is in cart');
}

// Check specific variant
if (GitCart.cart.hasItem('IPHONE15-128', { variant: 'blue-128gb' })) {
  console.log('Blue iPhone is in cart');
}
```

### `GitCart.cart.getItemCount()`

Returns total number of items in cart.

```javascript
const itemCount = GitCart.cart.getItemCount();
console.log(`Cart has ${itemCount} items`);

// Update cart badge
document.querySelector('.cart-badge').textContent = itemCount;
```

### `GitCart.cart.getTotal(options)`

Returns cart total with various calculation options.

```javascript
// Basic total
const total = GitCart.cart.getTotal();
console.log(`Total: $${total}`);

// With breakdown
const totals = GitCart.cart.getTotal({
  includeShipping: true,
  includeTax: true,
  includeDiscount: true
});

console.log('Breakdown:', totals);
// Output:
{
  subtotal: 1899,
  shipping: 50,
  tax: 152.41,
  discount: -100,
  total: 2001.41,
  currency: 'USD'
}
```

## 🔍 Search API

Client-side product search with Fuse.js.

### `GitCart.search.query(searchTerm, filters)`

Searches products with filters.

```javascript
// Basic search
const results = GitCart.search.query('iphone');

// Search with filters
const results = GitCart.search.query('smartphone', {
  category: 'electronics',
  price: { min: 500, max: 1500 },
  tags: ['5g', 'premium'],
  inStock: true
});

console.log(`Found ${results.length} products`);
```

### `GitCart.search.filter(filters)`

Filters products without search term.

```javascript
// Filter by category
const electronics = GitCart.search.filter({
  category: 'electronics'
});

// Multiple filters
const premiumPhones = GitCart.search.filter({
  category: 'electronics/phones',
  price: { min: 800 },
  tags: ['premium'],
  featured: true
});
```

### `GitCart.search.sort(products, field, direction)`

Sorts product results.

```javascript
const products = GitCart.search.query('phone');

// Sort by price (ascending)
const sortedByPrice = GitCart.search.sort(products, 'price', 'asc');

// Sort by name (descending)
const sortedByName = GitCart.search.sort(products, 'title', 'desc');

// Sort by popularity
const sortedByPopularity = GitCart.search.sort(products, 'featured', 'desc');
```

### Search Configuration

```javascript
GitCart.search.configure({
  // Fuse.js options
  threshold: 0.4,
  keys: ['title', 'description', 'tags', 'category'],
  
  // Custom options
  maxResults: 50,
  highlightMatches: true,
  trackSearches: true
});
```

## 💳 Checkout API

Complete checkout process management.

### `GitCart.checkout.start(options)`

Initiates checkout process.

```javascript
// Start basic checkout
GitCart.checkout.start();

// Start with pre-filled data
GitCart.checkout.start({
  customer: {
    email: 'john@example.com',
    firstName: 'John',
    lastName: 'Doe'
  },
  shipping: {
    method: 'standard'
  }
});
```

### `GitCart.checkout.setCustomer(customerData)`

Sets customer information.

```javascript
GitCart.checkout.setCustomer({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john@example.com',
  phone: '+1-555-123-4567',
  company: 'Acme Corp',
  note: 'Call before delivery'
});
```

### `GitCart.checkout.setShipping(method, address)`

Sets shipping method and address.

```javascript
// Set shipping method
GitCart.checkout.setShipping('fedex-express', {
  street: '123 Main Street',
  city: 'New York',
  state: 'NY',
  zip: '10001',
  country: 'US'
});

// Available shipping methods are configured per project
const methods = GitCart.checkout.getShippingMethods();
console.log('Available shipping:', methods);
```

### `GitCart.checkout.setPayment(method, details)`

Sets payment method.

```javascript
// Set payment method
GitCart.checkout.setPayment('stripe', {
  token: 'tok_visa_4242',
  last4: '4242',
  brand: 'Visa'
});

// Available payment methods
const methods = GitCart.checkout.getPaymentMethods();
console.log('Available payments:', methods);
```

### `GitCart.checkout.calculate()`

Calculates order totals with shipping and tax.

```javascript
const totals = GitCart.checkout.calculate();
console.log('Order totals:', totals);

// Output:
{
  subtotal: 1899,
  shipping: {
    method: 'fedex-express',
    cost: 25
  },
  tax: {
    rate: 0.08,
    amount: 153.92
  },
  discount: {
    code: 'SAVE10',
    amount: -100
  },
  total: 1977.92,
  currency: 'USD'
}
```

### `GitCart.checkout.complete()`

Completes the checkout process.

```javascript
try {
  const order = await GitCart.checkout.complete();
  
  console.log('Order completed:', order.id);
  
  // Redirect to success page
  window.location.href = `/order-success/?id=${order.id}`;
  
} catch (error) {
  console.error('Checkout failed:', error);
  alert('Order failed: ' + error.message);
}
```

## 📊 Analytics and Events

Event system for tracking user behavior.

### Core Events

```javascript
// Page events
GitCart.on('gitcart:page:loaded', function(data) {
  console.log('Page loaded:', data.page, data.url);
});

// Product events
GitCart.on('gitcart:product:viewed', function(product) {
  console.log('Product viewed:', product.title);
});

// Cart events
GitCart.on('gitcart:cart:item:added', function(item) {
  console.log('Added to cart:', item.title, 'x' + item.quantity);
});

GitCart.on('gitcart:cart:item:removed', function(item) {
  console.log('Removed from cart:', item.title);
});

GitCart.on('gitcart:cart:updated', function(cart) {
  console.log('Cart updated:', cart.itemCount, 'items, total:', cart.total);
});

// Search events
GitCart.on('gitcart:search:query', function(data) {
  console.log('Search:', data.query, 'found', data.results.length, 'results');
});

// Checkout events
GitCart.on('gitcart:order:started', function(data) {
  console.log('Checkout started with', data.itemCount, 'items');
});

GitCart.on('gitcart:checkout:completed', function(order) {
  console.log('Order completed:', order.id, 'total:', order.total);
});
```

### Custom Event Tracking

```javascript
// Track custom events
GitCart.track('custom-event', {
  category: 'user-interaction',
  action: 'newsletter-signup',
  value: 1
});

// Track product interactions
GitCart.track('product-interaction', {
  sku: 'IPHONE15-128',
  action: 'zoom-image',
  imageIndex: 2
});

// Track performance
GitCart.track('performance', {
  metric: 'page-load-time',
  value: 1200, // ms
  page: 'product-detail'
});
```

## 🔌 Plugin System

Extend functionality with plugins.

### Plugin Registration

```javascript
GitCart.plugin('my-plugin', {
  // Plugin configuration
  config: {
    name: 'My Plugin',
    version: '1.0.0',
    apiKey: 'your-api-key'
  },
  
  // Event listeners
  events: {
    'gitcart:product:viewed': function(product) {
      this.trackProductView(product);
    },
    
    'gitcart:cart:item:added': function(item) {
      this.trackAddToCart(item);
    }
  },
  
  // Custom methods
  trackProductView: function(product) {
    console.log('Tracking product view:', product.title);
  },
  
  trackAddToCart: function(item) {
    console.log('Tracking add to cart:', item.title);
  },
  
  // Initialize plugin
  init: function() {
    console.log('My Plugin initialized');
  }
});
```

### Plugin Events

```javascript
// Plugin can emit custom events
GitCart.plugin('analytics', {
  events: {
    'gitcart:checkout:completed': function(order) {
      // Send to analytics service
      this.sendToAnalytics(order);
      
      // Emit custom event
      GitCart.emit('analytics:order:tracked', {
        orderId: order.id,
        value: order.total
      });
    }
  }
});

// Other plugins can listen to custom events
GitCart.on('analytics:order:tracked', function(data) {
  console.log('Order tracked by analytics:', data.orderId);
});
```

## 🛠️ Utility Functions

Helper functions for common tasks.

### `GitCart.utils.formatPrice(amount, currency, locale)`

Formats price according to locale.

```javascript
// Basic formatting
const price = GitCart.utils.formatPrice(999, 'USD');
console.log(price); // "$999.00"

// With locale
const price = GitCart.utils.formatPrice(999, 'EUR', 'de-DE');
console.log(price); // "999,00 €"

// With options
const price = GitCart.utils.formatPrice(999.99, 'USD', 'en-US', {
  minimumFractionDigits: 0,
  maximumFractionDigits: 0
});
console.log(price); // "$1,000"
```

### `GitCart.utils.formatDate(date, format, locale)`

Formats date according to locale.

```javascript
const date = new Date('2024-01-15');

// Basic formatting
const formatted = GitCart.utils.formatDate(date);
console.log(formatted); // "January 15, 2024"

// With custom format
const formatted = GitCart.utils.formatDate(date, 'short');
console.log(formatted); // "1/15/24"

// With locale
const formatted = GitCart.utils.formatDate(date, 'long', 'cs-CZ');
console.log(formatted); // "15. ledna 2024"
```

### `GitCart.utils.slugify(text)`

Converts text to URL-friendly slug.

```javascript
const slug = GitCart.utils.slugify('iPhone 15 Pro Max!');
console.log(slug); // "iphone-15-pro-max"

const slug = GitCart.utils.slugify('Špičkový telefon 2024');
console.log(slug); // "spickovy-telefon-2024"
```

### `GitCart.utils.debounce(func, wait)`

Debounces function calls.

```javascript
// Debounced search
const debouncedSearch = GitCart.utils.debounce(function(query) {
  const results = GitCart.search.query(query);
  displayResults(results);
}, 300);

// Use in search input
document.getElementById('search').addEventListener('input', function(e) {
  debouncedSearch(e.target.value);
});
```

### `GitCart.utils.storage`

localStorage wrapper with fallbacks.

```javascript
// Set data
GitCart.utils.storage.set('user-preferences', {
  theme: 'dark',
  currency: 'USD'
});

// Get data
const prefs = GitCart.utils.storage.get('user-preferences');
console.log(prefs.theme); // "dark"

// Remove data
GitCart.utils.storage.remove('user-preferences');

// Clear all GitCart data
GitCart.utils.storage.clear();
```

## 🔧 Configuration

Global GitCart configuration options.

### Default Configuration

```javascript
GitCart.config = {
  // Basic settings
  currency: 'USD',
  locale: 'en-US',
  debug: false,
  
  // Cart settings
  cart: {
    persistent: true,
    storageKey: 'gitcart_cart',
    maxItems: 100,
    autoSave: true
  },
  
  // Search settings
  search: {
    enabled: true,
    minQueryLength: 2,
    maxResults: 50,
    highlightMatches: true
  },
  
  // Checkout settings
  checkout: {
    requireEmail: true,
    requirePhone: false,
    validateAddress: true,
    allowGuestCheckout: true
  },
  
  // Analytics settings
  analytics: {
    enabled: true,
    trackPageViews: true,
    trackCartEvents: true,
    trackSearchEvents: true
  }
};
```

### Runtime Configuration

```javascript
// Update configuration at runtime
GitCart.configure({
  currency: 'EUR',
  locale: 'de-DE',
  cart: {
    maxItems: 50
  }
});

// Get current configuration
const config = GitCart.getConfig();
console.log('Current locale:', config.locale);
```

## 🚨 Error Handling

Comprehensive error handling and debugging.

### Error Events

```javascript
// Listen to errors
GitCart.on('error', function(error) {
  console.error('GitCart error:', error);
  
  // Log to external service
  if (window.Sentry) {
    Sentry.captureException(error);
  }
});

// Specific error types
GitCart.on('error:cart', function(error) {
  console.error('Cart error:', error.message);
  alert('Cart operation failed. Please try again.');
});

GitCart.on('error:checkout', function(error) {
  console.error('Checkout error:', error.message);
  alert('Checkout failed: ' + error.message);
});
```

### Debug Mode

```javascript
// Enable debug mode
GitCart.configure({ debug: true });

// Debug mode provides:
// - Verbose console logging
// - Performance timing
// - State change tracking
// - Validation warnings

// Manual debugging
GitCart.debug('Custom debug message', { data: 'value' });
```

### Validation

```javascript
// Validate cart item before adding
try {
  GitCart.cart.validate({
    sku: 'INVALID-SKU',
    quantity: -1,
    price: 'not-a-number'
  });
} catch (error) {
  console.error('Validation failed:', error.message);
}

// Validate checkout data
try {
  GitCart.checkout.validate({
    customer: { email: 'invalid-email' },
    shipping: { method: 'non-existent' }
  });
} catch (error) {
  console.error('Checkout validation failed:', error.message);
}
```

This completes the comprehensive GitCart.js Frontend API documentation. The API provides a complete e-commerce solution with cart management, search functionality, checkout process, event system, plugin architecture, and utility functions - all designed to be lightweight, flexible, and easy to use.