# GitCart - Plugin Development

Complete guide for developing and using plugins in GitCart.

## 🔌 Plugin Architecture

GitCart uses an event-driven plugin system that allows extending functionality without modifying core code.

### Key Principles

- **Event-driven**: Plugins react to e-shop events
- **Zone rendering**: Plugins can inject HTML content into templates
- **Self-contained**: Each plugin is a standalone file with configuration
- **Hot-loading**: Plugins are automatically loaded during build

## 🚀 Creating Your First Plugin

### CLI Scaffolding

```bash
gitcart plugin create my-first-plugin
? Plugin name: My First Plugin
? Description: My first GitCart plugin
? Events: gitcart:product:viewed
? Rendering zones: product-detail-after-images
? Plugin type: content
```

### Basic Plugin Structure

```javascript
// plugins/my-first-plugin.js
(function() {
  'use strict';
  
  // Plugin configuration
  const CONFIG = {
    name: 'my-first-plugin',
    version: '1.0.0',
    description: 'My first GitCart plugin',
    author: 'Your Name',
    enabled: true,
    
    // Custom plugin settings
    customOption: 'default-value',
    apiKey: 'your-api-key-here'
  };

  // Plugin logic
  const MyPlugin = {
    
    init() {
      console.log(`${CONFIG.name} v${CONFIG.version} initialized`);
      this.setupEventListeners();
    },

    setupEventListeners() {
      // Custom event listeners
    },

    handleProductViewed(productData) {
      console.log('Product viewed:', productData.title);
    },

    renderProductContent(context) {
      const product = context.product;
      if (!product) return '';
      
      return `
        <div class="my-plugin-content">
          <h3>Plugin Content</h3>
          <p>Product: ${product.title}</p>
          <p>Price: ${product.price} ${product.currency}</p>
        </div>
      `;
    }
  };

  // Auto-registration
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('my-first-plugin', {
      config: CONFIG,
      
      events: {
        'gitcart:product:viewed': MyPlugin.handleProductViewed.bind(MyPlugin)
      },

      render: {
        'product-detail-after-images': MyPlugin.renderProductContent.bind(MyPlugin)
      },

      init: MyPlugin.init.bind(MyPlugin)
    });
  } else {
    console.warn('GitCart not found - plugin not registered');
  }
})();
```

## 📋 Event System

### Core Events

#### E-commerce Events

```javascript
// Page loaded
'gitcart:page:loaded': {
  page: 'product|category|home|cart|checkout',
  locale: 'en-us',
  url: '/products/iphone-15'
}

// Product viewed
'gitcart:product:viewed': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  price: 999,
  category: 'electronics',
  url: '/products/iphone-15'
}

// Products filtered
'gitcart:products:filtered': {
  query: 'iphone',
  filters: {
    category: 'electronics',
    price: { min: 500, max: 1500 }
  },
  results: 15
}
```

#### Cart Events

```javascript
// Item added to cart
'gitcart:cart:item:added': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  quantity: 1,
  price: 999,
  total: 999
}

// Item removed from cart
'gitcart:cart:item:removed': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  quantity: 1,
  price: 999
}

// Quantity changed
'gitcart:cart:quantity:changed': {
  sku: 'IPHONE15-128',
  oldQuantity: 1,
  newQuantity: 2,
  price: 999
}

// Cart updated
'gitcart:cart:updated': {
  items: [/* cart items */],
  itemCount: 3,
  subtotal: 1999,
  total: 2049
}
```

#### Checkout Events

```javascript
// Order started
'gitcart:order:started': {
  items: [/* cart items */],
  total: 2049,
  currency: 'USD'
}

// Shipping selected
'gitcart:shipping:selected': {
  method: 'fedex',
  methodName: 'FedEx Express',
  price: 50,
  total: 2049
}

// Payment selected
'gitcart:payment:selected': {
  method: 'card',
  methodName: 'Credit Card',
  total: 2049
}

// Order completed
'gitcart:checkout:completed': {
  id: 'order_1642234567890',
  items: [/* order items */],
  customer: {/* customer data */},
  shipping: {/* shipping data */},
  payment: {/* payment data */},
  totals: {/* totals */}
}
```

### Event Listening

```javascript
const MyPlugin = {
  events: {
    // Single event
    'gitcart:product:viewed': function(data) {
      this.trackProductView(data);
    },

    // Multiple events
    'gitcart:cart:item:added gitcart:cart:item:removed': function(data) {
      this.updateCartWidget();
    },

    // All cart events
    'gitcart:cart:*': function(data, eventName) {
      console.log('Cart event:', eventName, data);
    }
  }
};
```

## 🎨 Rendering Zones

### Available Zones

```javascript
const AVAILABLE_ZONES = {
  // HTML Head
  'head-meta': 'SEO meta tags, tracking codes, styles',
  
  // Body
  'body-start': 'Analytics codes, tracking pixels right after <body>',
  'body-end': 'Chat widgets, scripts before </body>',
  
  // Header
  'header-start': 'Before navigation',
  'header-end': 'After navigation', 
  
  // Product detail
  'product-detail-after-images': 'After product images',
  'product-detail-after-price': 'After product price',
  'product-detail-after-description': 'After product description',
  'product-detail-end': 'End of product page',
  
  // Category
  'category-before-products': 'Before product list',
  'category-after-products': 'After product list',
  
  // Cart
  'cart-item-actions': 'Actions for each cart item',
  'cart-summary': 'In cart summary',
  
  // Checkout
  'shipping-selection': 'Shipping selection',
  'payment-selection': 'Payment selection',
  'order-summary': 'Order summary',
  'order-success-info': 'Thank you page',
  
  // Footer
  'before-footer': 'Before footer',
  'footer-start': 'Footer start',
  'footer-end': 'Footer end'
};
```

### Rendering to Zones

```javascript
const MyPlugin = {
  render: {
    // Static HTML
    'product-detail-after-images': function(context) {
      return '<div class="plugin-content">Static HTML</div>';
    },

    // Dynamic content based on context
    'product-detail-after-price': function(context) {
      const product = context.product;
      if (!product) return '';
      
      return `
        <div class="product-features">
          <h4>Why buy ${product.title}?</h4>
          <ul>
            <li>✓ 2 year warranty</li>
            <li>✓ Free shipping over $50</li>
            <li>✓ 30 day returns</li>
          </ul>
        </div>
      `;
    },

    // Conditional rendering
    'head-meta': function(context) {
      if (context.page !== 'product') return '';
      
      const product = context.product;
      return `
        <meta name="product:price:amount" content="${product.price}">
        <meta name="product:price:currency" content="${product.currency}">
      `;
    }
  }
};
```

## 🛠️ Plugin Types and Examples

### Analytics Plugin

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'google-analytics',
    version: '1.2.0',
    trackingId: 'G-XXXXXXXXXX',
    enableEcommerce: true,
    anonymizeIp: true
  };

  const GoogleAnalytics = {
    
    init() {
      this.loadGoogleAnalytics();
      this.setupEcommerce();
    },

    loadGoogleAnalytics() {
      // Load GA script
      const script = document.createElement('script');
      script.src = `https://www.googletagmanager.com/gtag/js?id=${CONFIG.trackingId}`;
      document.head.appendChild(script);
      
      // Initialize GA
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      window.gtag = gtag;
      gtag('js', new Date());
      gtag('config', CONFIG.trackingId, {
        anonymize_ip: CONFIG.anonymizeIp
      });
    },

    trackPurchase(orderData) {
      if (!CONFIG.enableEcommerce) return;
      
      gtag('event', 'purchase', {
        transaction_id: orderData.id,
        value: orderData.totals.total,
        currency: orderData.currency,
        items: orderData.items.map(item => ({
          item_id: item.sku,
          item_name: item.title,
          category: item.category,
          quantity: item.quantity,
          price: item.price
        }))
      });
    },

    trackViewItem(productData) {
      gtag('event', 'view_item', {
        currency: productData.currency,
        value: productData.price,
        items: [{
          item_id: productData.sku,
          item_name: productData.title,
          category: productData.category,
          price: productData.price
        }]
      });
    },

    trackAddToCart(cartData) {
      gtag('event', 'add_to_cart', {
        currency: cartData.currency || 'USD',
        value: cartData.price * cartData.quantity,
        items: [{
          item_id: cartData.sku,
          item_name: cartData.title,
          category: cartData.category,
          quantity: cartData.quantity,
          price: cartData.price
        }]
      });
    }
  };

  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('google-analytics', {
      config: CONFIG,
      
      events: {
        'gitcart:product:viewed': GoogleAnalytics.trackViewItem.bind(GoogleAnalytics),
        'gitcart:cart:item:added': GoogleAnalytics.trackAddToCart.bind(GoogleAnalytics),
        'gitcart:checkout:completed': GoogleAnalytics.trackPurchase.bind(GoogleAnalytics)
      },

      render: {
        'head-meta': function() {
          return `<!-- Google Analytics by GitCart Plugin -->`;
        }
      },

      init: GoogleAnalytics.init.bind(GoogleAnalytics)
    });
  }
})();
```

### Email Notifications Plugin

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'email-notifications',
    version: '1.0.0',
    
    // Mailgun config
    apiKey: 'key-your-mailgun-key',
    domain: 'your-domain.com',
    apiUrl: 'https://api.mailgun.net/v3',
    
    // Email settings
    fromEmail: 'noreply@your-domain.com',
    fromName: 'Your Shop',
    ownerEmail: 'orders@your-domain.com',
    
    // Feature flags
    enableOwnerNotification: true,
    enableCustomerConfirmation: true
  };

  const EmailNotifications = {
    
    async sendOrderNotification(orderData) {
      try {
        if (CONFIG.enableOwnerNotification) {
          await this.sendOwnerNotification(orderData);
        }
        
        if (CONFIG.enableCustomerConfirmation) {
          await this.sendCustomerConfirmation(orderData);
        }
      } catch (error) {
        console.error('Failed to send order notifications:', error);
      }
    },

    async sendOwnerNotification(orderData) {
      const emailData = {
        from: `${CONFIG.fromName} <${CONFIG.fromEmail}>`,
        to: CONFIG.ownerEmail,
        subject: `New Order ${orderData.id}`,
        html: this.buildOwnerEmailHtml(orderData),
        text: this.buildOwnerEmailText(orderData)
      };
      
      return this.sendEmail(emailData);
    },

    async sendEmail(emailData) {
      const formData = new FormData();
      Object.keys(emailData).forEach(key => {
        formData.append(key, emailData[key]);
      });
      
      const response = await fetch(`${CONFIG.apiUrl}/${CONFIG.domain}/messages`, {
        method: 'POST',
        headers: {
          'Authorization': 'Basic ' + btoa('api:' + CONFIG.apiKey)
        },
        body: formData
      });
      
      if (!response.ok) {
        throw new Error(`Email send failed: ${response.statusText}`);
      }
      
      return response.json();
    }
  };

  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('email-notifications', {
      config: CONFIG,
      
      events: {
        'gitcart:checkout:completed': EmailNotifications.sendOrderNotification.bind(EmailNotifications)
      },

      render: {
        'order-success-info': function() {
          return `
            <div class="email-notification-info">
              <p>✓ Order confirmation has been sent to your email address.</p>
            </div>
          `;
        }
      }
    });
  }
})();
```

## 🔧 Plugin Development Workflow

### 1. Local Development

```bash
# Create new plugin
gitcart plugin create my-plugin

# Dev server with watch mode
gitcart dev --watch

# Plugin auto-loads on changes
```

### 2. Plugin Testing

```javascript
// __tests__/plugins/my-plugin.test.js
describe('My Plugin', () => {
  let plugin;
  
  beforeEach(() => {
    // Mock GitCart
    global.GitCart = {
      plugin: jest.fn(),
      on: jest.fn(),
      emit: jest.fn()
    };
    
    // Load plugin
    require('../../plugins/my-plugin.js');
  });

  test('should register with correct config', () => {
    expect(GitCart.plugin).toHaveBeenCalledWith('my-plugin', expect.objectContaining({
      config: expect.objectContaining({
        name: 'my-plugin',
        version: expect.any(String)
      })
    }));
  });

  test('should handle product viewed event', () => {
    const mockProduct = {
      sku: 'TEST-123',
      title: 'Test Product',
      price: 999
    };
    
    // Simulate event
    const pluginCall = GitCart.plugin.mock.calls[0][1];
    const eventHandler = pluginCall.events['gitcart:product:viewed'];
    
    expect(() => eventHandler(mockProduct)).not.toThrow();
  });
});
```

### 3. Plugin Distribution

#### Local Plugin
```
your-project/
├── plugins/
│   ├── my-custom-plugin.js
│   └── another-plugin.js
```

#### Shared Plugin
```bash
# Publish to npm
npm publish gitcart-plugin-my-plugin

# Install
npm install gitcart-plugin-my-plugin
gitcart plugin add gitcart-plugin-my-plugin
```

## 🛡️ Plugin Security

### Input Validation

```javascript
const CONFIG = {
  name: 'secure-plugin',
  apiKey: process.env.PLUGIN_API_KEY || 'your-api-key-here'
};

const SecurePlugin = {
  validateOrderData(orderData) {
    // Validate required fields
    const required = ['id', 'customer', 'items', 'totals'];
    for (const field of required) {
      if (!orderData[field]) {
        throw new Error(`Missing required field: ${field}`);
      }
    }
    
    // Validate email
    const email = orderData.customer.email;
    if (!email || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      throw new Error('Invalid email address');
    }
    
    // Validate items
    orderData.items.forEach((item, index) => {
      if (!item.sku || !item.title || !item.price) {
        throw new Error(`Invalid item at index ${index}`);
      }
    });
    
    return true;
  },

  sanitizeHtml(html) {
    // Basic HTML sanitization
    return html
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#39;');
  }
};
```

### Error Handling

```javascript
const SafePlugin = {
  async handleOrderCompleted(orderData) {
    try {
      this.validateOrderData(orderData);
      await this.processOrder(orderData);
    } catch (error) {
      // Log error without exposing sensitive data
      console.error('Plugin error:', {
        plugin: CONFIG.name,
        error: error.message,
        orderId: orderData.id
      });
      
      // Don't block user workflow
      return;
    }
  },

  renderSafeContent(context) {
    try {
      const content = this.generateContent(context);
      return this.sanitizeHtml(content);
    } catch (error) {
      console.warn('Render error:', error.message);
      return ''; // Graceful fallback
    }
  }
};
```

## 🎯 Best Practices

### 1. Plugin Configuration

```javascript
// ✅ Good - configuration at the start
const CONFIG = {
  name: 'my-plugin',
  version: '1.0.0',
  enabled: true,
  apiKey: 'configurable-key'
};

// ❌ Bad - hardcoded values
function sendRequest() {
  fetch('https://api.example.com/endpoint', {
    headers: {
      'Authorization': 'Bearer hardcoded-key'
    }
  });
}
```

### 2. Event Handling

```javascript
// ✅ Good - defensive programming
events: {
  'gitcart:product:viewed': function(productData) {
    if (!productData || !productData.sku) {
      console.warn('Invalid product data received');
      return;
    }
    
    this.trackProductView(productData);
  }
}

// ❌ Bad - assumes correct data
events: {
  'gitcart:product:viewed': function(productData) {
    gtag('event', 'view_item', {
      item_id: productData.sku, // might be undefined
      value: productData.price
    });
  }
}
```

### 3. Rendering

```javascript
// ✅ Good - conditional rendering
render: {
  'product-detail-after-images': function(context) {
    if (!context.product || !CONFIG.enabled) {
      return '';
    }
    
    return this.renderProductFeatures(context.product);
  }
}

// ❌ Bad - always renders
render: {
  'product-detail-after-images': function(context) {
    return '<div>Always visible content</div>';
  }
}
```

### 4. Error Handling

```javascript
// ✅ Good - graceful degradation
async processOrder(orderData) {
  try {
    await this.sendToExternalAPI(orderData);
  } catch (error) {
    console.error('External API failed:', error.message);
    // Continue without external service
    return this.fallbackProcess(orderData);
  }
}

// ❌ Bad - blocking error
async processOrder(orderData) {
  await this.sendToExternalAPI(orderData); // might crash
}
```