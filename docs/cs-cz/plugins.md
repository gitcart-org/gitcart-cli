# GitCart - Vývoj pluginů

Kompletní příručka pro vývoj a použití pluginů v GitCart.

## 🔌 Plugin architektura

GitCart používá event-driven plugin systém, který umožňuje rozšířit funkcionalitu bez zásahu do core kódu.

### Klíčové principy

- **Event-driven**: Pluginy reagují na události v e-shopu
- **Zone rendering**: Pluginy mohou vkládat HTML obsah do templates
- **Self-contained**: Každý plugin je samostatný soubor s konfigurací
- **Hot-loading**: Pluginy se automaticky načítají při buildu

## 🚀 Vytvoření prvního pluginu

### CLI scaffolding

```bash
gitcart plugin create my-first-plugin
? Plugin name: My First Plugin
? Description: My first GitCart plugin
? Events: gitcart:product:viewed
? Rendering zones: product-detail-after-images
? Plugin type: content
```

### Základní struktura pluginu

```javascript
// plugins/my-first-plugin.js
(function() {
  'use strict';
  
  // Plugin konfigurace
  const CONFIG = {
    name: 'my-first-plugin',
    version: '1.0.0',
    description: 'My first GitCart plugin',
    author: 'Your Name',
    enabled: true,
    
    // Vlastní nastavení pluginu
    customOption: 'default-value',
    apiKey: 'your-api-key-here'
  };

  // Plugin logika
  const MyPlugin = {
    
    init() {
      console.log(`${CONFIG.name} v${CONFIG.version} initialized`);
      this.setupEventListeners();
    },

    setupEventListeners() {
      // Vlastní event listenery
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

  // Auto-registrace pluginu
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

## 📋 Event systém

### Core události

#### E-commerce události

```javascript
// Načtení stránky
'gitcart:page:loaded': {
  page: 'product|category|home|cart|checkout',
  locale: 'cs-cz',
  url: '/produkty/iphone-15'
}

// Zobrazení produktu
'gitcart:product:viewed': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  price: 25999,
  category: 'elektronika',
  url: '/produkty/iphone-15'
}

// Filtrování produktů
'gitcart:products:filtered': {
  query: 'iphone',
  filters: {
    category: 'elektronika',
    price: { min: 10000, max: 30000 }
  },
  results: 15
}
```

#### Košík události

```javascript
// Přidání do košíku
'gitcart:cart:item:added': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  quantity: 1,
  price: 25999,
  total: 25999
}

// Odebrání z košíku
'gitcart:cart:item:removed': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  quantity: 1,
  price: 25999
}

// Změna množství
'gitcart:cart:quantity:changed': {
  sku: 'IPHONE15-128',
  oldQuantity: 1,
  newQuantity: 2,
  price: 25999
}

// Aktualizace košíku
'gitcart:cart:updated': {
  items: [/* cart items */],
  itemCount: 3,
  subtotal: 45999,
  total: 46148
}
```

#### Checkout události

```javascript
// Zahájení objednávky
'gitcart:order:started': {
  items: [/* cart items */],
  total: 46148,
  currency: 'CZK'
}

// Výběr dopravy
'gitcart:shipping:selected': {
  method: 'ppl',
  methodName: 'PPL kurýr',
  price: 149,
  total: 46148
}

// Výběr platby
'gitcart:payment:selected': {
  method: 'card',
  methodName: 'Platební karta',
  total: 46148
}

// Dokončení objednávky
'gitcart:checkout:completed': {
  id: 'order_1642234567890',
  items: [/* order items */],
  customer: {/* customer data */},
  shipping: {/* shipping data */},
  payment: {/* payment data */},
  totals: {/* totals */}
}
```

### Poslouchání událostí

```javascript
const MyPlugin = {
  events: {
    // Jedna událost
    'gitcart:product:viewed': function(data) {
      this.trackProductView(data);
    },

    // Více událostí
    'gitcart:cart:item:added gitcart:cart:item:removed': function(data) {
      this.updateCartWidget();
    },

    // Všechny cart události
    'gitcart:cart:*': function(data, eventName) {
      console.log('Cart event:', eventName, data);
    }
  }
};
```

## 🎨 Rendering zones

### Dostupné zones

```javascript
const AVAILABLE_ZONES = {
  // HTML Head
  'head-meta': 'SEO meta tagy, tracking kódy, styles',
  
  // Body
  'body-start': 'Analytics kódy, tracking pixely hned po <body>',
  'body-end': 'Chat widgety, skripty před </body>',
  
  // Hlavička
  'header-start': 'Před navigací',
  'header-end': 'Za navigací', 
  
  // Produkt detail
  'product-detail-after-images': 'Za obrázky produktu',
  'product-detail-after-price': 'Za cenou produktu',
  'product-detail-after-description': 'Za popisem produktu',
  'product-detail-end': 'Konec stránky produktu',
  
  // Kategorie
  'category-before-products': 'Před seznamem produktů',
  'category-after-products': 'Za seznamem produktů',
  
  // Košík
  'cart-item-actions': 'Akce u každé položky v košíku',
  'cart-summary': 'V souhrnu košíku',
  
  // Checkout
  'shipping-selection': 'Výběr dopravy',
  'payment-selection': 'Výběr platby',
  'order-summary': 'Souhrn objednávky',
  'order-success-info': 'Thank you page',
  
  // Footer
  'before-footer': 'Před footrem',
  'footer-start': 'Začátek footru',
  'footer-end': 'Konec footru'
};
```

### Rendering do zones

```javascript
const MyPlugin = {
  render: {
    // Statický HTML
    'product-detail-after-images': function(context) {
      return '<div class="plugin-content">Static HTML</div>';
    },

    // Dynamický obsah podle kontextu
    'product-detail-after-price': function(context) {
      const product = context.product;
      if (!product) return '';
      
      return `
        <div class="product-features">
          <h4>Proč koupit ${product.title}?</h4>
          <ul>
            <li>✓ 2 roky záruky</li>
            <li>✓ Doprava zdarma nad 1000 Kč</li>
            <li>✓ 30 dní na vrácení</li>
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

## 🛠️ Plugin typy a příklady

### Analytics plugin

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
        currency: cartData.currency || 'CZK',
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

### Email notifications plugin

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

    async sendCustomerConfirmation(orderData) {
      const customer = orderData.customer;
      const emailData = {
        from: `${CONFIG.fromName} <${CONFIG.fromEmail}>`,
        to: customer.email,
        subject: `Order Confirmation ${orderData.id}`,
        html: this.buildCustomerEmailHtml(orderData),
        text: this.buildCustomerEmailText(orderData)
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
    },

    buildOwnerEmailHtml(orderData) {
      return `
        <h2>New Order Received</h2>
        <p><strong>Order ID:</strong> ${orderData.id}</p>
        <p><strong>Customer:</strong> ${orderData.customer.firstName} ${orderData.customer.lastName}</p>
        <p><strong>Email:</strong> ${orderData.customer.email}</p>
        <p><strong>Total:</strong> ${orderData.totals.total} ${orderData.currency}</p>
        
        <h3>Items:</h3>
        <ul>
          ${orderData.items.map(item => `
            <li>${item.title} x${item.quantity} = ${item.subtotal} ${orderData.currency}</li>
          `).join('')}
        </ul>
        
        <h3>Shipping:</h3>
        <p>${orderData.shipping.methodName} - ${orderData.shipping.price} ${orderData.currency}</p>
        <p>${orderData.shipping.address.street}, ${orderData.shipping.address.city} ${orderData.shipping.address.zip}</p>
        
        <h3>Payment:</h3>
        <p>${orderData.payment.methodName}</p>
      `;
    },

    buildCustomerEmailHtml(orderData) {
      return `
        <h2>Thank you for your order!</h2>
        <p>Dear ${orderData.customer.firstName},</p>
        <p>Your order <strong>${orderData.id}</strong> has been received and is being processed.</p>
        
        <h3>Order Summary:</h3>
        <table border="1" cellpadding="10">
          <tr>
            <th>Product</th>
            <th>Quantity</th>
            <th>Price</th>
            <th>Total</th>
          </tr>
          ${orderData.items.map(item => `
            <tr>
              <td>${item.title}</td>
              <td>${item.quantity}</td>
              <td>${item.price} ${orderData.currency}</td>
              <td>${item.subtotal} ${orderData.currency}</td>
            </tr>
          `).join('')}
        </table>
        
        <p><strong>Subtotal:</strong> ${orderData.totals.subtotal} ${orderData.currency}</p>
        <p><strong>Shipping:</strong> ${orderData.totals.shipping} ${orderData.currency}</p>
        <p><strong>Total:</strong> ${orderData.totals.total} ${orderData.currency}</p>
        
        <p>We'll send you another email when your order ships.</p>
        <p>Thank you for shopping with us!</p>
      `;
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

### Chat widget plugin

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'chat-widget',
    version: '1.0.0',
    
    // Provider: 'tawk', 'intercom', 'crisp', 'custom'
    provider: 'tawk',
    
    // Provider specific settings
    tawk: {
      widgetId: 'your-tawk-widget-id',
      chatId: 'your-tawk-chat-id'
    },
    
    intercom: {
      appId: 'your-intercom-app-id'
    },
    
    crisp: {
      websiteId: 'your-crisp-website-id'
    },
    
    // Widget settings
    position: 'bottom-right',
    showOnMobile: true,
    showOnPages: ['product', 'category', 'cart'],
    
    // Custom widget settings
    customText: {
      title: 'Need Help?',
      subtitle: 'Chat with us',
      offline: 'Leave a message',
      minimized: '💬'
    }
  };

  const ChatWidget = {
    
    init() {
      this.loadChatProvider();
      this.setupEventListeners();
    },

    loadChatProvider() {
      switch (CONFIG.provider) {
        case 'tawk':
          this.loadTawkTo();
          break;
        case 'intercom':
          this.loadIntercom();
          break;
        case 'crisp':
          this.loadCrisp();
          break;
        case 'custom':
          this.loadCustomWidget();
          break;
      }
    },

    loadTawkTo() {
      var Tawk_API = Tawk_API || {};
      var Tawk_LoadStart = new Date();
      
      (function(){
        var s1 = document.createElement("script");
        var s0 = document.getElementsByTagName("script")[0];
        s1.async = true;
        s1.src = `https://embed.tawk.to/${CONFIG.tawk.widgetId}/${CONFIG.tawk.chatId}`;
        s1.charset = 'UTF-8';
        s1.setAttribute('crossorigin','*');
        s0.parentNode.insertBefore(s1, s0);
      })();
    },

    loadCustomWidget() {
      // Custom chat widget implementation
      const widget = this.createCustomWidget();
      document.body.appendChild(widget);
      
      // Add CSS
      const style = document.createElement('style');
      style.textContent = this.getCustomWidgetCSS();
      document.head.appendChild(style);
    },

    createCustomWidget() {
      const widget = document.createElement('div');
      widget.id = 'custom-chat-widget';
      widget.className = `chat-widget ${CONFIG.position}`;
      
      widget.innerHTML = `
        <div class="chat-minimized" onclick="openChat()">
          <span class="chat-icon">${CONFIG.customText.minimized}</span>
        </div>
        <div class="chat-expanded" style="display: none;">
          <div class="chat-header">
            <h4>${CONFIG.customText.title}</h4>
            <button onclick="closeChat()" class="close-btn">×</button>
          </div>
          <div class="chat-body">
            <p>${CONFIG.customText.subtitle}</p>
            <div class="chat-buttons">
              <button onclick="startChat('general')">General Question</button>
              <button onclick="startChat('order')">Order Support</button>
              <button onclick="startChat('technical')">Technical Help</button>
            </div>
          </div>
        </div>
      `;
      
      return widget;
    },

    getCustomWidgetCSS() {
      return `
        .chat-widget {
          position: fixed;
          z-index: 9999;
          font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }
        
        .chat-widget.bottom-right {
          bottom: 20px;
          right: 20px;
        }
        
        .chat-minimized {
          width: 60px;
          height: 60px;
          background: #007cba;
          border-radius: 50%;
          display: flex;
          align-items: center;
          justify-content: center;
          cursor: pointer;
          box-shadow: 0 2px 12px rgba(0,0,0,0.15);
          transition: transform 0.2s;
        }
        
        .chat-minimized:hover {
          transform: scale(1.05);
        }
        
        .chat-icon {
          font-size: 24px;
          color: white;
        }
        
        .chat-expanded {
          width: 300px;
          height: 400px;
          background: white;
          border-radius: 8px;
          box-shadow: 0 5px 25px rgba(0,0,0,0.2);
          display: flex;
          flex-direction: column;
        }
        
        .chat-header {
          background: #007cba;
          color: white;
          padding: 15px;
          border-radius: 8px 8px 0 0;
          display: flex;
          justify-content: space-between;
          align-items: center;
        }
        
        .chat-header h4 {
          margin: 0;
          font-size: 16px;
        }
        
        .close-btn {
          background: none;
          border: none;
          color: white;
          font-size: 20px;
          cursor: pointer;
        }
        
        .chat-body {
          padding: 20px;
          flex: 1;
        }
        
        .chat-buttons button {
          display: block;
          width: 100%;
          margin-bottom: 10px;
          padding: 12px;
          background: #f8f9fa;
          border: 1px solid #dee2e6;
          border-radius: 4px;
          cursor: pointer;
          transition: background-color 0.2s;
        }
        
        .chat-buttons button:hover {
          background: #e9ecef;
        }
        
        @media (max-width: 768px) {
          .chat-expanded {
            width: calc(100vw - 40px);
            height: calc(100vh - 40px);
            position: fixed;
            top: 20px;
            left: 20px;
            right: 20px;
            bottom: 20px;
          }
        }
      `;
    },

    handleOrderCompleted(orderData) {
      // Auto-open chat for order support
      if (CONFIG.provider === 'tawk' && window.Tawk_API) {
        Tawk_API.maximize();
      }
    }
  };

  // Global functions for custom widget
  window.openChat = function() {
    document.querySelector('.chat-minimized').style.display = 'none';
    document.querySelector('.chat-expanded').style.display = 'flex';
  };

  window.closeChat = function() {
    document.querySelector('.chat-minimized').style.display = 'flex';
    document.querySelector('.chat-expanded').style.display = 'none';
  };

  window.startChat = function(type) {
    alert(`Starting ${type} chat support...`);
    // Integration with actual chat service
  };

  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('chat-widget', {
      config: CONFIG,
      
      events: {
        'gitcart:checkout:completed': ChatWidget.handleOrderCompleted.bind(ChatWidget)
      },

      render: {
        'body-end': function(context) {
          if (!CONFIG.showOnPages.includes(context.page)) {
            return '';
          }
          
          return '<!-- Chat widget will be loaded by JavaScript -->';
        }
      },

      init: ChatWidget.init.bind(ChatWidget)
    });
  }
})();
```

## 🔧 Plugin development workflow

### 1. Lokální vývoj

```bash
# Vytvoření nového pluginu
gitcart plugin create my-plugin

# Dev server s watch módem
gitcart dev --watch

# Plugin se automaticky načte při změně
```

### 2. Testing pluginu

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

### 3. Plugin distribuce

#### Lokální plugin
```
your-project/
├── plugins/
│   ├── my-custom-plugin.js
│   └── another-plugin.js
```

#### Sdílený plugin
```bash
# Publikování na npm
npm publish gitcart-plugin-my-plugin

# Instalace
npm install gitcart-plugin-my-plugin
gitcart plugin add gitcart-plugin-my-plugin
```

## 🛡️ Bezpečnost pluginů

### Validace inputů

```javascript
const CONFIG = {
  name: 'secure-plugin',
  apiKey: process.env.PLUGIN_API_KEY || 'your-api-key-here'
};

const SecurePlugin = {
  validateOrderData(orderData) {
    // Validace povinných polí
    const required = ['id', 'customer', 'items', 'totals'];
    for (const field of required) {
      if (!orderData[field]) {
        throw new Error(`Missing required field: ${field}`);
      }
    }
    
    // Validace email
    const email = orderData.customer.email;
    if (!email || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      throw new Error('Invalid email address');
    }
    
    // Validace položek
    orderData.items.forEach((item, index) => {
      if (!item.sku || !item.title || !item.price) {
        throw new Error(`Invalid item at index ${index}`);
      }
    });
    
    return true;
  },

  sanitizeHtml(html) {
    // Základní HTML sanitizace
    return html
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#39;');
  }
};
```

### Error handling

```javascript
const SafePlugin = {
  async handleOrderCompleted(orderData) {
    try {
      this.validateOrderData(orderData);
      await this.processOrder(orderData);
    } catch (error) {
      // Log error bez expose sensitive dat
      console.error('Plugin error:', {
        plugin: CONFIG.name,
        error: error.message,
        orderId: orderData.id
      });
      
      // Neblokuj uživatelský workflow
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

## 📊 Plugin analytics

### Performance monitoring

```javascript
const PerformantPlugin = {
  init() {
    this.startTime = performance.now();
    console.log(`${CONFIG.name} initializing...`);
  },

  measurePerformance(operation, fn) {
    return function(...args) {
      const start = performance.now();
      const result = fn.apply(this, args);
      const end = performance.now();
      
      console.log(`${CONFIG.name}.${operation}: ${(end - start).toFixed(2)}ms`);
      return result;
    };
  },

  trackEvent(eventName, data) {
    // Track plugin usage
    const event = {
      plugin: CONFIG.name,
      event: eventName,
      timestamp: new Date().toISOString(),
      data: data
    };
    
    // Send to analytics service
    this.sendAnalytics(event);
  }
};
```

## 🎯 Best practices

### 1. Plugin konfigurace

```javascript
// ✅ Dobré - konfigurace na začátku
const CONFIG = {
  name: 'my-plugin',
  version: '1.0.0',
  enabled: true,
  apiKey: 'configurable-key'
};

// ❌ Špatné - hardcoded hodnoty
function sendRequest() {
  fetch('https://api.example.com/endpoint', {
    headers: {
      'Authorization': 'Bearer hardcoded-key'
    }
  });
}
```

### 2. Event handling

```javascript
// ✅ Dobré - defensive programming
events: {
  'gitcart:product:viewed': function(productData) {
    if (!productData || !productData.sku) {
      console.warn('Invalid product data received');
      return;
    }
    
    this.trackProductView(productData);
  }
}

// ❌ Špatné - předpokládá správná data
events: {
  'gitcart:product:viewed': function(productData) {
    gtag('event', 'view_item', {
      item_id: productData.sku, // může být undefined
      value: productData.price
    });
  }
}
```

### 3. Rendering

```javascript
// ✅ Dobré - conditional rendering
render: {
  'product-detail-after-images': function(context) {
    if (!context.product || !CONFIG.enabled) {
      return '';
    }
    
    return this.renderProductFeatures(context.product);
  }
}

// ❌ Špatné - vždy renderuje
render: {
  'product-detail-after-images': function(context) {
    return '<div>Always visible content</div>';
  }
}
```

### 4. Error handling

```javascript
// ✅ Dobré - graceful degradation
async processOrder(orderData) {
  try {
    await this.sendToExternalAPI(orderData);
  } catch (error) {
    console.error('External API failed:', error.message);
    // Pokračuj bez external služby
    return this.fallbackProcess(orderData);
  }
}

// ❌ Špatné - blokující chyba
async processOrder(orderData) {
  await this.sendToExternalAPI(orderData); // může spadnout
}
```