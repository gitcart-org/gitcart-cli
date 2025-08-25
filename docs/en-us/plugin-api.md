# GitCart - Plugin API

Complete reference guide for Plugin API and custom plugin development.

## 🔌 Plugin API Overview

GitCart Plugin API provides a powerful system for extending e-shop functionality without modifying the core code. Plugins are self-contained JavaScript files that automatically load and integrate with the GitCart system.

### Key Features

- **Self-contained** - Each plugin is a standalone file with configuration
- **Event-driven** - React to e-shop events
- **Zone rendering** - Insert HTML content into templates
- **Hot-loading** - Automatic loading on changes
- **Type-safe** - TypeScript definitions for better DX
- **Isolation** - Plugins don't interfere with each other

## 🚀 Basic Plugin Structure

### Minimal Plugin

```javascript
(function() {
  'use strict';
  
  // Plugin configuration
  const CONFIG = {
    name: 'my-plugin',
    version: '1.0.0',
    description: 'My first GitCart plugin',
    enabled: true
  };

  // Plugin logic
  const MyPlugin = {
    init() {
      console.log(`${CONFIG.name} v${CONFIG.version} loaded`);
    }
  };

  // Auto-registration
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('my-plugin', {
      config: CONFIG,
      init: MyPlugin.init.bind(MyPlugin)
    });
  }
})();
```

### Complete Plugin Structure

```javascript
(function() {
  'use strict';
  
  // 1. Plugin configuration
  const CONFIG = {
    name: 'advanced-plugin',
    version: '2.1.0',
    description: 'Advanced GitCart plugin with all features',
    author: 'Your Name',
    website: 'https://yoursite.com',
    enabled: true,
    
    // Plugin-specific settings
    apiKey: 'your-api-key',
    endpoint: 'https://api.example.com',
    features: {
      tracking: true,
      notifications: false,
      advanced: true
    }
  };

  // 2. Plugin class/object
  const AdvancedPlugin = {
    
    // Initialization
    init() {
      this.log('Initializing...');
      this.setupEventListeners();
      this.loadSettings();
      this.createUI();
    },

    // Setup after DOM loaded
    setup() {
      this.bindDOMEvents();
      this.startServices();
    },

    // Cleanup on disconnect
    destroy() {
      this.removeEventListeners();
      this.cleanup();
    },

    // Event handlers
    handleProductViewed(productData) {
      this.log('Product viewed:', productData.title);
      this.trackEvent('product_view', {
        product_id: productData.sku,
        product_name: productData.title,
        category: productData.category.title,
        price: productData.price
      });
    },

    handleCartItemAdded(cartData) {
      this.log('Item added to cart:', cartData);
      this.showNotification('Product added to cart!');
      this.updateCartAnalytics(cartData);
    },

    handleOrderCompleted(orderData) {
      this.log('Order completed:', orderData.id);
      this.sendOrderToAPI(orderData);
      this.trackPurchase(orderData);
    },

    // Zone rendering functions
    renderProductBadge(context) {
      if (context.product && context.product.featured) {
        return `<div class="plugin-badge featured-badge">
          <span class="badge-text">Featured</span>
        </div>`;
      }
      return '';
    },

    renderCheckoutExtra(context) {
      return `<div class="plugin-checkout-extra">
        <div class="trust-signals">
          <div class="signal">
            <img src="/assets/icons/secure.svg" alt="Secure">
            <span>Secure Checkout</span>
          </div>
          <div class="signal">
            <img src="/assets/icons/guarantee.svg" alt="Guarantee">
            <span>Money Back Guarantee</span>
          </div>
        </div>
      </div>`;
    },

    // Helper methods
    log(message, data = null) {
      if (CONFIG.debug) {
        console.log(`[${CONFIG.name}]`, message, data);
      }
    },

    trackEvent(eventName, properties = {}) {
      if (!CONFIG.features.tracking) return;
      
      // Send to analytics service
      this.sendToAnalytics('event', {
        event: eventName,
        properties: {
          plugin: CONFIG.name,
          version: CONFIG.version,
          timestamp: new Date().toISOString(),
          ...properties
        }
      });
    },

    async sendToAnalytics(type, data) {
      try {
        await fetch(`${CONFIG.endpoint}/analytics`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${CONFIG.apiKey}`
          },
          body: JSON.stringify({ type, data })
        });
      } catch (error) {
        this.log('Analytics error:', error);
      }
    },

    showNotification(message, type = 'success') {
      // Create notification UI
      const notification = document.createElement('div');
      notification.className = `plugin-notification ${type}`;
      notification.textContent = message;
      
      document.body.appendChild(notification);
      
      // Auto-remove after 3 seconds
      setTimeout(() => {
        if (notification.parentNode) {
          notification.parentNode.removeChild(notification);
        }
      }, 3000);
    }
  };

  // 3. Auto-registration with GitCart
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('advanced-plugin', {
      // Plugin metadata
      config: CONFIG,
      
      // Event handlers
      events: {
        'gitcart:page:loaded': AdvancedPlugin.setup.bind(AdvancedPlugin),
        'gitcart:product:viewed': AdvancedPlugin.handleProductViewed.bind(AdvancedPlugin),
        'gitcart:cart:item:added': AdvancedPlugin.handleCartItemAdded.bind(AdvancedPlugin),
        'gitcart:checkout:completed': AdvancedPlugin.handleOrderCompleted.bind(AdvancedPlugin)
      },

      // Zone renderers
      render: {
        'product-detail-after-images': AdvancedPlugin.renderProductBadge.bind(AdvancedPlugin),
        'checkout-trust-signals': AdvancedPlugin.renderCheckoutExtra.bind(AdvancedPlugin)
      },

      // Lifecycle hooks
      init: AdvancedPlugin.init.bind(AdvancedPlugin),
      destroy: AdvancedPlugin.destroy.bind(AdvancedPlugin)
    });
  } else {
    console.warn('GitCart not found - plugin not loaded');
  }
})();
```

## 📋 Plugin Configuration

### Required Fields

```javascript
const CONFIG = {
  name: 'plugin-name',           // Unique plugin identifier
  version: '1.0.0',              // Semantic version
  description: 'Plugin description' // Short description
};
```

### Optional Fields

```javascript
const CONFIG = {
  // Metadata
  author: 'Your Name',
  website: 'https://yoursite.com',
  license: 'MIT',
  keywords: ['ecommerce', 'analytics', 'seo'],
  
  // Control
  enabled: true,                 // Enable/disable plugin
  debug: false,                  // Debug mode
  priority: 10,                  // Loading priority (higher = later)
  
  // Dependencies
  requires: ['other-plugin'],    // Required plugins
  conflicts: ['conflicting-plugin'], // Conflicting plugins
  gitcart_version: '>=1.0.0',   // Required GitCart version
  
  // Features
  features: {
    analytics: true,
    notifications: false,
    advanced_ui: true
  },
  
  // API settings
  endpoints: {
    api: 'https://api.example.com',
    webhook: 'https://webhook.example.com'
  },
  
  // UI settings
  ui: {
    theme: 'dark',
    position: 'top-right',
    showOnPages: ['product', 'category']
  }
};
```

### Dynamic Configuration from site.json

**New feature!** Plugins can now load configuration directly from `site.json`, enabling configuration without modifying plugin code.

#### 1. Configuration in site.json

```json
{
  "plugins": {
    "google-analytics": {
      "trackingId": "G-ABCD123456", 
      "enableEcommerce": true,
      "debugMode": false
    },
    "email-notifications": {
      "provider": "mailgun",
      "mailgunApiKey": "key-xyz",
      "mailgunDomain": "myshop.com",
      "toEmail": "orders@myshop.com"
    },
    "chat-widget": {
      "provider": "tawk",
      "widgetId": "abcd1234",
      "position": "bottom-right"
    }
  }
}
```

#### 2. Plugin Implementation

```javascript
// Plugin configuration - default values
const DEFAULT_CONFIG = {
  name: 'my-plugin',
  version: '1.0.0',
  trackingId: 'YOUR_TRACKING_ID', // Will be overridden from site.json
  enableFeature: true
};

const MyPlugin = {
  init() {
    // Load configuration from site.json via GitCart API - automatically merges defaults with site.json
    this.config = GitCart.getPluginConfig('my-plugin', DEFAULT_CONFIG);
    
    console.log('Plugin config:', this.config);
    // { name: 'my-plugin', trackingId: 'G-REAL123', enableFeature: true }
    
    // Use configuration
    if (!this.config.trackingId || this.config.trackingId === 'YOUR_TRACKING_ID') {
      console.warn('Please configure trackingId in site.json plugins section');
      return;
    }
    
    this.setupTracking(this.config.trackingId);
  }
};
```

#### 3. Benefits of site.json Configuration

- **🔧 Centralized config** - all plugin settings in one file
- **🔒 Security** - no API keys in plugin code  
- **🔄 Environment-specific** - different configs for dev/staging/prod
- **👨‍💼 Non-tech friendly** - marketers can change configurations
- **📝 Version controlled** - configuration versioned with project
- **🚀 Deployment** - no code changes when deploying

#### 4. Available Configuration

```javascript
// GitCart.getPluginConfig(pluginName, defaultConfig) returns:
{
  // Everything from site.json plugins.pluginName section
  ...siteJsonConfig,
  
  // Plus automatic values:
  locale: 'en-us',      // Current locale
  currency: 'USD',      // Current currency
  siteUrl: 'https://...' // Project URL
}
```

## 🔥 Event System

### Available Core Events

#### Initialization & Page

```javascript
// GitCart initialization completed
'gitcart:initialized': {
  config: object
}

// Page loaded
'gitcart:page:loaded': {
  page: 'product|category|home|cart|checkout',
  locale: 'en-us',
  url: '/products/iphone-15',
  title: 'iPhone 15'
}
```

#### Product Events

```javascript
// Product viewed (automatically on product pages)
'gitcart:product:viewed': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  price: 25999,
  category: 'electronics',
  url: '/products/iphone-15',
  brand: 'Apple',
  inStock: true
}
```

#### Search Events

```javascript
// Search initialized
'gitcart:search:initialized': {
  languages: ['en', 'cs'],
  config: object
}

// Search language changed
'gitcart:search:language:changed': {
  language: 'en'
}

// Product search performed
'gitcart:search:query': {
  term: 'iphone',
  filters: {
    category: 'electronics',
    price: { min: 10000, max: 30000 }
  },
  resultsCount: 15,
  duration: 245 // ms
}

// Filter applied
'gitcart:search:filtered': {
  results: Array,
  filters: object,
  originalCount: 50,
  filteredCount: 15
}

// Results sorted
'gitcart:search:sorted': {
  results: Array,
  field: 'price',
  direction: 'asc|desc'
}
```

#### Cart Events

```javascript
// Cart loaded from storage
'gitcart:cart:initialized': {
  items: Array
}

// Item added to cart
'gitcart:cart:item:added': {
  sku: 'IPHONE15-128',
  quantity: 1,
  options: {
    variant: 'blue-128gb'
  },
  timestamp: '2024-01-15T10:30:00Z'
}

// Cart item updated (quantity, variant, etc.)
'gitcart:cart:item:updated': {
  item: object,
  oldQuantity: 1,
  newQuantity: 2,
  sku: 'IPHONE15-128',
  variant: 'blue-128gb'
}

// Item removed from cart
'gitcart:cart:item:removed': {
  item: object
}

// Cart updated (after any change)
'gitcart:cart:updated': {
  cart: {
    items: Array,
    createdAt: string,
    updatedAt: string
  }
}

// Cart cleared
'gitcart:cart:cleared': {
  oldItems: Array
}
```

#### Checkout Events

```javascript
// Checkout started
'gitcart:checkout:started': {
  items: Array,
  itemCount: 3,
  subtotal: 45999,
  source: 'cart'
}

// Customer information entered
'gitcart:checkout:customer:set': {
  customer: {
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@example.com',
    phone: '+1234567890',
    company: '',
    note: ''
  },
  email: 'john@example.com',
  hasAccount: false,
  isReturningCustomer: true
}

// Shipping method selected
'gitcart:checkout:shipping:set': {
  shipping: {
    method: 'ups',
    methodName: 'UPS Standard',
    price: 149,
    address: object
  },
  method: 'ups',
  methodName: 'UPS Standard',
  price: 149,
  deliveryTime: '1-2 days',
  total: 46148
}

// Payment method selected
'gitcart:checkout:payment:set': {
  payment: {
    method: 'card',
    methodName: 'Credit Card'
  },
  method: 'card',
  methodName: 'Credit Card',
  provider: 'unknown',
  total: 46148
}

// Order completed
'gitcart:checkout:completed': {
  id: 'order_1642234567890',
  timestamp: '2024-01-15T10:30:00Z',
  items: Array,
  customer: Object,
  shipping: Object,
  payment: Object,
  totals: Object,
  source: 'web'
}
```

#### Plugin System

```javascript
// Plugin registered
'gitcart:plugin:registered': {
  name: 'google-analytics',
  plugin: object
}
```

### Event Handler Examples

```javascript
// Product view tracking
handleProductViewed(data) {
  console.log('Product viewed:', data.product.title);
  
  // Send to analytics
  gtag('event', 'view_item', {
    currency: data.product.currency,
    value: data.product.price,
    items: [{
      item_id: data.product.sku,
      item_name: data.product.title,
      item_category: data.product.category.title,
      price: data.product.price,
      quantity: 1
    }]
  });
}

// Cart abandonment detection
handleCartUpdated(data) {
  if (data.cart.items.length > 0) {
    // Set abandonment timer
    this.abandonmentTimer = setTimeout(() => {
      this.sendAbandonmentEmail(data.cart);
    }, 30 * 60 * 1000); // 30 minutes
  } else {
    // Clear timer if cart is emptied
    clearTimeout(this.abandonmentTimer);
  }
}

// Purchase conversion tracking
handleOrderCompleted(data) {
  // Google Analytics Enhanced Ecommerce
  gtag('event', 'purchase', {
    transaction_id: data.order.id,
    value: data.order.totals.total,
    currency: data.order.currency,
    items: data.order.items.map(item => ({
      item_id: item.sku,
      item_name: item.title,
      item_category: item.category,
      quantity: item.quantity,
      price: item.price
    }))
  });
  
  // Facebook Pixel
  fbq('track', 'Purchase', {
    value: data.order.totals.total,
    currency: data.order.currency
  });
}
```

## 🎨 Zone Rendering System

### Available Zones

#### Global Zones
```javascript
'zone-head'                    // <head> section - meta tags, analytics
'body-start'              // Start of <body> - tracking pixels
'header-start'            // Beginning of header
'header-end'              // End of header
'main-start'              // Start of main content
'main-end'                // End of main content
'before-footer'           // Before footer
'footer-start'            // Beginning of footer
'footer-end'              // End of footer
'body-end'                // End of <body> - scripts
```

#### Product Page Zones
```javascript
'product-detail-start'           // Start of product page
'product-images-start'           // Before product images
'product-images-end'             // After product images
'product-info-start'             // Start of product info
'product-before-add-to-cart'     // Before add to cart button
'product-after-add-to-cart'      // After add to cart form
'product-info-end'               // End of product info
'product-before-description'     // Before description
'product-after-description'      // After description
'product-before-reviews'         // Before reviews section
'product-after-reviews'          // After reviews section
'product-detail-end'             // End of product page
```

#### Category Page Zones
```javascript
'category-start'                 // Start of category page
'category-after-header'          // After category header
'category-before-filters'        // Before filter sidebar
'category-after-filters'         // After filter sidebar
'category-before-products'       // Before product grid
'category-after-products'        // After product grid
'category-before-pagination'     // Before pagination
'category-after-pagination'      // After pagination
'category-end'                   // End of category page
```

#### Cart & Checkout Zones
```javascript
'cart-start'                     // Start of cart page
'cart-before-items'              // Before cart items
'cart-after-items'               // After cart items
'cart-before-totals'             // Before cart totals
'cart-after-totals'              // After cart totals
'checkout-start'                 // Start of checkout
'checkout-customer-info'         // Customer information section
'checkout-shipping-options'      // Shipping options
'checkout-payment-options'       // Payment options
'checkout-order-review'          // Order review section
'checkout-trust-signals'         // Trust badges/security info
'order-success-info'             // Order confirmation page
```

### Zone Rendering Examples

```javascript
// Simple HTML injection
render: {
  'product-after-add-to-cart': function(context) {
    return '<div class="plugin-guarantee">30-day money back guarantee</div>';
  }
}

// Dynamic content based on context
render: {
  'product-detail-after-images': function(context) {
    if (context.product.featured) {
      return `<div class="featured-badge">
        <span class="badge-text">Staff Pick</span>
        <span class="badge-subtitle">Recommended by our experts</span>
      </div>`;
    }
    return '';
  }
}

// Complex UI with interactions
render: {
  'product-before-add-to-cart': function(context) {
    const product = context.product;
    return `
      <div class="plugin-size-guide" data-product-id="${product.sku}">
        <button type="button" class="size-guide-trigger" onclick="openSizeGuide('${product.sku}')">
          📏 Size Guide
        </button>
        <div class="size-guide-modal" id="size-guide-${product.sku}" style="display: none;">
          <div class="modal-content">
            <span class="close" onclick="closeSizeGuide('${product.sku}')">&times;</span>
            <h3>Size Guide for ${product.title}</h3>
            <div class="size-chart">
              <!-- Size chart content -->
            </div>
          </div>
        </div>
      </div>
      <script>
        function openSizeGuide(productId) {
          document.getElementById('size-guide-' + productId).style.display = 'block';
        }
        function closeSizeGuide(productId) {
          document.getElementById('size-guide-' + productId).style.display = 'none';
        }
      </script>
    `;
  }
}

// Conditional rendering with multiple zones
render: {
  'checkout-trust-signals': function(context) {
    const signals = [
      { icon: '🔒', text: 'Secure SSL encryption' },
      { icon: '🚚', text: 'Free shipping over $50' },
      { icon: '↩️', text: '30-day return policy' }
    ];
    
    return `<div class="trust-signals">
      ${signals.map(signal => `
        <div class="trust-signal">
          <span class="signal-icon">${signal.icon}</span>
          <span class="signal-text">${signal.text}</span>
        </div>
      `).join('')}
    </div>`;
  },

  'order-success-info': function(context) {
    return `<div class="plugin-order-success">
      <h3>Thank you for your order!</h3>
      <p>Order #${context.order.id} has been confirmed.</p>
      <p>Tracking information will be sent to ${context.customer.email}</p>
    </div>`;
  }
}
```

## 🔧 Plugin Utilities and Helpers

### Storage API
```javascript
// Local storage helpers
const PluginStorage = {
  set(key, value) {
    const pluginKey = `${CONFIG.name}_${key}`;
    localStorage.setItem(pluginKey, JSON.stringify(value));
  },

  get(key, defaultValue = null) {
    const pluginKey = `${CONFIG.name}_${key}`;
    const stored = localStorage.getItem(pluginKey);
    return stored ? JSON.parse(stored) : defaultValue;
  },

  remove(key) {
    const pluginKey = `${CONFIG.name}_${key}`;
    localStorage.removeItem(pluginKey);
  }
};

// Usage
PluginStorage.set('user_preferences', { theme: 'dark', notifications: true });
const preferences = PluginStorage.get('user_preferences', {});
```

### HTTP Client
```javascript
const PluginHTTP = {
  async request(url, options = {}) {
    const defaultOptions = {
      headers: {
        'Content-Type': 'application/json',
        'X-Plugin-Name': CONFIG.name,
        'X-Plugin-Version': CONFIG.version
      }
    };

    const response = await fetch(url, { ...defaultOptions, ...options });
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return await response.json();
  },

  async get(url, headers = {}) {
    return this.request(url, { method: 'GET', headers });
  },

  async post(url, data, headers = {}) {
    return this.request(url, {
      method: 'POST',
      headers,
      body: JSON.stringify(data)
    });
  }
};

// Usage
try {
  const result = await PluginHTTP.post('/api/track', {
    event: 'product_view',
    product_id: product.sku
  });
} catch (error) {
  console.error('API request failed:', error);
}
```

### Event Emitter
```javascript
const PluginEvents = {
  listeners: new Map(),

  on(event, callback) {
    if (!this.listeners.has(event)) {
      this.listeners.set(event, []);
    }
    this.listeners.get(event).push(callback);
  },

  emit(event, data) {
    const callbacks = this.listeners.get(event) || [];
    callbacks.forEach(callback => {
      try {
        callback(data);
      } catch (error) {
        console.error(`Plugin event error in ${event}:`, error);
      }
    });
  },

  off(event, callback) {
    const callbacks = this.listeners.get(event) || [];
    const index = callbacks.indexOf(callback);
    if (index > -1) {
      callbacks.splice(index, 1);
    }
  }
};
```

### DOM Utilities
```javascript
const PluginDOM = {
  ready(callback) {
    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', callback);
    } else {
      callback();
    }
  },

  createElement(html) {
    const template = document.createElement('template');
    template.innerHTML = html.trim();
    return template.content.firstChild;
  },

  addCSS(css) {
    const style = document.createElement('style');
    style.textContent = css;
    document.head.appendChild(style);
    return style;
  },

  loadScript(src) {
    return new Promise((resolve, reject) => {
      const script = document.createElement('script');
      script.src = src;
      script.onload = resolve;
      script.onerror = reject;
      document.head.appendChild(script);
    });
  }
};

// Usage
PluginDOM.ready(() => {
  console.log('DOM is ready');
});

const modal = PluginDOM.createElement(`
  <div class="modal">
    <div class="modal-content">
      <h2>Plugin Modal</h2>
      <p>This is a plugin-generated modal.</p>
    </div>
  </div>
`);
```

## 🎯 Real-World Plugin Examples

### 1. Google Analytics 4 Plugin

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'google-analytics-4',
    version: '1.0.0',
    description: 'Google Analytics 4 integration for GitCart',
    author: 'GitCart Team',
    ga4_id: 'G-XXXXXXXXXX', // Replace with your GA4 ID
    enhanced_ecommerce: true,
    debug: false
  };

  const GA4Plugin = {
    init() {
      this.loadGA4();
      this.setupEnhancedEcommerce();
    },

    loadGA4() {
      // Load gtag script
      const script = document.createElement('script');
      script.async = true;
      script.src = `https://www.googletagmanager.com/gtag/js?id=${CONFIG.ga4_id}`;
      document.head.appendChild(script);

      // Initialize gtag
      window.dataLayer = window.dataLayer || [];
      window.gtag = function(){dataLayer.push(arguments);};
      
      gtag('js', new Date());
      gtag('config', CONFIG.ga4_id, {
        enhanced_ecommerce: CONFIG.enhanced_ecommerce,
        debug_mode: CONFIG.debug
      });
    },

    setupEnhancedEcommerce() {
      if (!CONFIG.enhanced_ecommerce) return;

      // Configure enhanced ecommerce settings
      gtag('config', CONFIG.ga4_id, {
        custom_map: {
          custom_parameter_1: 'item_category'
        }
      });
    },

    trackPageView(data) {
      gtag('event', 'page_view', {
        page_title: data.title,
        page_location: window.location.href,
        page_path: data.url
      });
    },

    trackProductView(data) {
      gtag('event', 'view_item', {
        currency: data.product.currency,
        value: data.product.price,
        items: [{
          item_id: data.product.sku,
          item_name: data.product.title,
          item_category: data.product.category.title,
          item_brand: data.product.brand || '',
          price: data.product.sale_price || data.product.price,
          quantity: 1
        }]
      });
    },

    trackAddToCart(data) {
      gtag('event', 'add_to_cart', {
        currency: data.item.currency || 'USD',
        value: data.item.price * data.item.quantity,
        items: [{
          item_id: data.item.sku,
          item_name: data.item.title,
          quantity: data.item.quantity,
          price: data.item.price
        }]
      });
    },

    trackPurchase(data) {
      gtag('event', 'purchase', {
        transaction_id: data.order.id,
        value: data.order.totals.total,
        currency: data.order.currency,
        items: data.order.items.map(item => ({
          item_id: item.sku,
          item_name: item.title,
          item_category: item.category || '',
          quantity: item.quantity,
          price: item.price
        }))
      });
    }
  };

  // Plugin registration
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('google-analytics-4', {
      config: CONFIG,
      
      events: {
        'gitcart:page:loaded': GA4Plugin.trackPageView.bind(GA4Plugin),
        'gitcart:product:viewed': GA4Plugin.trackProductView.bind(GA4Plugin),
        'gitcart:cart:item:added': GA4Plugin.trackAddToCart.bind(GA4Plugin),
        'gitcart:checkout:completed': GA4Plugin.trackPurchase.bind(GA4Plugin)
      },

      init: GA4Plugin.init.bind(GA4Plugin)
    });
  }
})();
```

### 2. Email Marketing Plugin

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'email-marketing',
    version: '1.2.0',
    description: 'Email marketing automation for GitCart',
    author: 'Marketing Team',
    
    // Mailchimp configuration
    mailchimp_api_key: 'your-api-key',
    mailchimp_server: 'us1',
    list_id: 'your-list-id',
    
    // Email triggers
    triggers: {
      cart_abandonment: true,
      welcome_series: true,
      order_confirmation: true,
      review_request: true
    },
    
    // Timing settings
    cart_abandonment_delay: 30, // minutes
    review_request_delay: 7      // days
  };

  const EmailPlugin = {
    init() {
      this.setupAbandonmentTracking();
      this.createNewsletterSignup();
    },

    setupAbandonmentTracking() {
      this.abandonmentTimer = null;
    },

    async subscribeToNewsletter(email, firstName = '', lastName = '') {
      const data = {
        email_address: email,
        status: 'subscribed',
        merge_fields: {
          FNAME: firstName,
          LNAME: lastName
        }
      };

      try {
        const response = await fetch(`https://${CONFIG.mailchimp_server}.api.mailchimp.com/3.0/lists/${CONFIG.list_id}/members`, {
          method: 'POST',
          headers: {
            'Authorization': `apikey ${CONFIG.mailchimp_api_key}`,
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        });

        if (response.ok) {
          this.showNotification('Successfully subscribed to newsletter!', 'success');
          return true;
        } else {
          throw new Error('Subscription failed');
        }
      } catch (error) {
        this.showNotification('Subscription failed. Please try again.', 'error');
        console.error('Newsletter subscription error:', error);
        return false;
      }
    },

    trackCartAbandonment(cartData) {
      if (!CONFIG.triggers.cart_abandonment) return;
      
      // Clear existing timer
      if (this.abandonmentTimer) {
        clearTimeout(this.abandonmentTimer);
      }

      // Set new abandonment timer
      this.abandonmentTimer = setTimeout(() => {
        this.sendAbandonmentEmail(cartData);
      }, CONFIG.cart_abandonment_delay * 60 * 1000);
    },

    async sendAbandonmentEmail(cartData) {
      // Implementation would send email via your email service
      console.log('Sending cart abandonment email for cart:', cartData);
      
      // Track the abandonment
      this.trackEvent('cart_abandoned', {
        cart_value: cartData.total,
        item_count: cartData.items.length,
        customer_email: cartData.customer_email || 'anonymous'
      });
    },

    createNewsletterSignup() {
      const signupHTML = `
        <div class="newsletter-signup" style="background: #f8f9fa; padding: 20px; border-radius: 8px; margin: 20px 0;">
          <h3 style="margin-bottom: 10px;">Stay Updated!</h3>
          <p style="margin-bottom: 15px; color: #666;">Subscribe to get exclusive offers and new product updates.</p>
          <form id="newsletter-form" class="newsletter-form" style="display: flex; gap: 10px;">
            <input type="email" 
                   id="newsletter-email" 
                   placeholder="Enter your email" 
                   required 
                   style="flex: 1; padding: 10px; border: 1px solid #ddd; border-radius: 4px;">
            <button type="submit" 
                    style="padding: 10px 20px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer;">
              Subscribe
            </button>
          </form>
        </div>
      `;

      return signupHTML;
    },

    bindNewsletterForm() {
      const form = document.getElementById('newsletter-form');
      if (form) {
        form.addEventListener('submit', async (e) => {
          e.preventDefault();
          const email = document.getElementById('newsletter-email').value;
          await this.subscribeToNewsletter(email);
        });
      }
    },

    handleOrderCompleted(orderData) {
      if (CONFIG.triggers.order_confirmation) {
        this.sendOrderConfirmation(orderData);
      }

      if (CONFIG.triggers.review_request) {
        this.scheduleReviewRequest(orderData);
      }
    },

    showNotification(message, type = 'info') {
      const notification = document.createElement('div');
      notification.className = `email-plugin-notification ${type}`;
      notification.style.cssText = `
        position: fixed;
        top: 20px;
        right: 20px;
        padding: 15px 20px;
        border-radius: 4px;
        color: white;
        z-index: 1000;
        background: ${type === 'success' ? '#28a745' : type === 'error' ? '#dc3545' : '#007bff'};
      `;
      notification.textContent = message;
      
      document.body.appendChild(notification);
      
      setTimeout(() => {
        if (notification.parentNode) {
          notification.parentNode.removeChild(notification);
        }
      }, 5000);
    }
  };

  // Plugin registration
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('email-marketing', {
      config: CONFIG,
      
      events: {
        'gitcart:cart:updated': EmailPlugin.trackCartAbandonment.bind(EmailPlugin),
        'gitcart:checkout:completed': EmailPlugin.handleOrderCompleted.bind(EmailPlugin),
        'gitcart:page:loaded': EmailPlugin.bindNewsletterForm.bind(EmailPlugin)
      },

      render: {
        'before-footer': EmailPlugin.createNewsletterSignup.bind(EmailPlugin)
      },

      init: EmailPlugin.init.bind(EmailPlugin)
    });
  }
})();
```

### 3. Live Chat Plugin

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'live-chat',
    version: '1.0.0',
    description: 'Live chat support integration',
    author: 'Support Team',
    
    // Chat service configuration
    service: 'intercom', // 'intercom', 'zendesk', 'crisp', 'custom'
    app_id: 'your-app-id',
    
    // Display settings
    position: 'bottom-right',
    theme: 'blue',
    show_on_pages: ['product', 'cart', 'checkout'],
    auto_open: false,
    
    // Behavior settings
    offline_message: 'Leave us a message and we\'ll get back to you!',
    welcome_message: 'Hi! How can we help you today?'
  };

  const ChatPlugin = {
    init() {
      this.loadChatService();
      this.setupChatTriggers();
    },

    loadChatService() {
      switch (CONFIG.service) {
        case 'intercom':
          this.loadIntercom();
          break;
        case 'zendesk':
          this.loadZendesk();
          break;
        case 'crisp':
          this.loadCrisp();
          break;
        default:
          this.loadCustomChat();
      }
    },

    loadIntercom() {
      window.intercomSettings = {
        app_id: CONFIG.app_id,
        name: "Customer",
        email: "customer@example.com" // Will be updated with real customer data
      };

      const script = document.createElement('script');
      script.src = 'https://widget.intercom.io/widget/' + CONFIG.app_id;
      script.async = true;
      document.head.appendChild(script);

      // Intercom API becomes available after load
      script.onload = () => {
        if (window.Intercom) {
          window.Intercom('boot', window.intercomSettings);
        }
      };
    },

    setupChatTriggers() {
      // Auto-open chat on certain conditions
      this.setupAbandonmentTrigger();
      this.setupProductViewTrigger();
    },

    setupAbandonmentTrigger() {
      let abandonmentTimer;

      // Trigger chat offer when user is about to leave
      document.addEventListener('mouseleave', (e) => {
        if (e.clientY <= 0) { // Mouse moved to top of screen
          abandonmentTimer = setTimeout(() => {
            this.offerHelp('Leaving so soon? Need help with anything?');
          }, 2000);
        }
      });

      document.addEventListener('mouseenter', () => {
        if (abandonmentTimer) {
          clearTimeout(abandonmentTimer);
        }
      });
    },

    setupProductViewTrigger() {
      let productViewCount = 0;
      
      // Offer help after viewing multiple products
      this.productViewHandler = () => {
        productViewCount++;
        if (productViewCount >= 3) {
          this.offerHelp('Looking for something specific? We\'re here to help!');
        }
      };
    },

    offerHelp(message) {
      if (CONFIG.service === 'intercom' && window.Intercom) {
        window.Intercom('showNewMessage', message);
      } else {
        this.showChatOffer(message);
      }
    },

    showChatOffer(message) {
      const offer = document.createElement('div');
      offer.className = 'chat-offer';
      offer.innerHTML = `
        <div class="chat-offer-bubble" style="
          position: fixed;
          bottom: 80px;
          right: 20px;
          background: #007bff;
          color: white;
          padding: 15px;
          border-radius: 20px;
          box-shadow: 0 4px 20px rgba(0,0,0,0.15);
          cursor: pointer;
          z-index: 1000;
          max-width: 250px;
          animation: slideIn 0.3s ease;
        ">
          <div class="message">${message}</div>
          <div class="actions" style="margin-top: 10px; text-align: right;">
            <button onclick="this.parentElement.parentElement.parentElement.remove()" 
                    style="background: none; border: none; color: white; margin-right: 10px; cursor: pointer;">
              No thanks
            </button>
            <button onclick="openChat()" 
                    style="background: rgba(255,255,255,0.2); border: none; color: white; padding: 5px 10px; border-radius: 10px; cursor: pointer;">
              Chat now
            </button>
          </div>
        </div>
      `;

      document.body.appendChild(offer);

      // Auto-remove after 10 seconds
      setTimeout(() => {
        if (offer.parentNode) {
          offer.parentNode.removeChild(offer);
        }
      }, 10000);
    },

    updateCustomerInfo(customerData) {
      if (CONFIG.service === 'intercom' && window.Intercom) {
        window.Intercom('update', {
          name: `${customerData.firstName} ${customerData.lastName}`,
          email: customerData.email,
          phone: customerData.phone
        });
      }
    },

    sendCartContext(cartData) {
      const context = `Cart Contents:\n${cartData.items.map(item => 
        `- ${item.title} x${item.quantity} ($${item.price})`
      ).join('\n')}\nTotal: $${cartData.total}`;

      if (CONFIG.service === 'intercom' && window.Intercom) {
        window.Intercom('update', {
          custom_attributes: {
            cart_value: cartData.total,
            cart_items: cartData.items.length,
            cart_contents: context
          }
        });
      }
    },

    renderChatButton() {
      if (CONFIG.service === 'custom') {
        return `
          <div id="chat-button" class="chat-button" onclick="openChat()" style="
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background: ${CONFIG.theme === 'blue' ? '#007bff' : '#28a745'};
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 1000;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
          ">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="white">
              <path d="M20 2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h4l4 4 4-4h4c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2z"/>
            </svg>
          </div>
        `;
      }
      return '';
    }
  };

  // Global function for chat button
  window.openChat = function() {
    if (CONFIG.service === 'intercom' && window.Intercom) {
      window.Intercom('show');
    } else {
      // Custom chat implementation
      console.log('Opening custom chat...');
    }
  };

  // Plugin registration
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('live-chat', {
      config: CONFIG,
      
      events: {
        'gitcart:product:viewed': ChatPlugin.productViewHandler,
        'gitcart:customer:data:entered': ChatPlugin.updateCustomerInfo.bind(ChatPlugin),
        'gitcart:cart:updated': ChatPlugin.sendCartContext.bind(ChatPlugin)
      },

      render: {
        'body-end': ChatPlugin.renderChatButton.bind(ChatPlugin)
      },

      init: ChatPlugin.init.bind(ChatPlugin)
    });
  }
})();
```

## 🧪 Plugin Testing

### Unit Testing

```javascript
// __tests__/plugins/my-plugin.test.js
describe('MyPlugin', () => {
  let plugin;
  let mockGitCart;

  beforeEach(() => {
    // Mock GitCart API
    mockGitCart = {
      plugin: jest.fn(),
      on: jest.fn(),
      emit: jest.fn(),
      cart: {
        getItems: jest.fn(() => []),
        add: jest.fn(),
        remove: jest.fn()
      }
    };

    global.GitCart = mockGitCart;
    
    // Load plugin
    require('../../plugins/my-plugin');
    
    // Get registered plugin
    const pluginCall = mockGitCart.plugin.mock.calls[0];
    plugin = pluginCall[1];
  });

  test('should register with correct name', () => {
    expect(mockGitCart.plugin).toHaveBeenCalledWith('my-plugin', expect.any(Object));
  });

  test('should handle product view event', () => {
    const productData = { product: { sku: 'TEST-123', title: 'Test Product' } };
    
    plugin.events['gitcart:product:viewed'](productData);
    
    // Assert expected behavior
    expect(console.log).toHaveBeenCalledWith(
      expect.stringContaining('Product viewed:'),
      'Test Product'
    );
  });

  test('should render zone content', () => {
    const context = { product: { featured: true } };
    
    const result = plugin.render['product-detail-after-images'](context);
    
    expect(result).toContain('featured-badge');
  });
});
```

### Integration Testing

```javascript
// __tests__/integration/plugin-system.test.js
describe('Plugin System Integration', () => {
  test('should load and initialize plugins', async () => {
    const mockPlugin = {
      config: { name: 'test-plugin' },
      init: jest.fn(),
      events: {
        'gitcart:product:viewed': jest.fn()
      }
    };

    GitCart.plugin('test-plugin', mockPlugin);
    
    // Simulate GitCart initialization
    await GitCart.init();
    
    expect(mockPlugin.init).toHaveBeenCalled();
  });

  test('should emit events to plugins', () => {
    const handler = jest.fn();
    
    GitCart.plugin('test-plugin', {
      config: { name: 'test-plugin' },
      events: {
        'gitcart:cart:item:added': handler
      }
    });

    GitCart.emit('gitcart:cart:item:added', { sku: 'TEST-123' });
    
    expect(handler).toHaveBeenCalledWith({ sku: 'TEST-123' });
  });
});
```

## 🔍 Plugin Debugging

### Debug Mode

```javascript
const CONFIG = {
  name: 'my-plugin',
  debug: true, // Enable debug mode
  // ... other config
};

const MyPlugin = {
  log(message, data = null) {
    if (CONFIG.debug) {
      console.group(`[${CONFIG.name}] ${message}`);
      if (data) console.log(data);
      console.trace(); // Show stack trace in debug mode
      console.groupEnd();
    }
  },

  handleEvent(eventData) {
    this.log('Event received', eventData);
    
    try {
      // Plugin logic
      this.processEvent(eventData);
    } catch (error) {
      this.log('Error processing event', error);
      if (CONFIG.debug) {
        throw error; // Re-throw in debug mode
      }
    }
  }
};
```

### Performance Monitoring

```javascript
const PluginProfiler = {
  timers: new Map(),

  start(operation) {
    this.timers.set(operation, performance.now());
  },

  end(operation) {
    const startTime = this.timers.get(operation);
    if (startTime) {
      const duration = performance.now() - startTime;
      console.log(`[${CONFIG.name}] ${operation}: ${duration.toFixed(2)}ms`);
      this.timers.delete(operation);
    }
  }
};

// Usage
PluginProfiler.start('product-processing');
// ... plugin work
PluginProfiler.end('product-processing');
```

## 📦 Plugin Distribution

### Plugin Metadata

```javascript
const CONFIG = {
  // Required metadata
  name: 'my-awesome-plugin',
  version: '1.2.3',
  description: 'An awesome plugin for GitCart',
  
  // Distribution metadata
  author: 'Your Name',
  license: 'MIT',
  homepage: 'https://github.com/yourname/my-awesome-plugin',
  repository: 'https://github.com/yourname/my-awesome-plugin.git',
  keywords: ['ecommerce', 'analytics', 'conversion'],
  
  // Compatibility
  gitcart_version: '>=1.0.0',
  requires: ['analytics-base'],
  conflicts: ['old-analytics'],
  
  // Documentation
  readme: 'https://github.com/yourname/my-awesome-plugin/blob/main/README.md',
  changelog: 'https://github.com/yourname/my-awesome-plugin/blob/main/CHANGELOG.md'
};
```

### Plugin Packaging

```bash
# Create plugin package
mkdir my-awesome-plugin
cd my-awesome-plugin

# Create plugin files
touch my-awesome-plugin.js
touch README.md
touch CHANGELOG.md
touch package.json

# Package for distribution
npm pack
```

This comprehensive Plugin API documentation provides everything needed to create powerful, maintainable plugins for GitCart. The event-driven architecture and zone rendering system allow for unlimited customization while maintaining clean separation of concerns.