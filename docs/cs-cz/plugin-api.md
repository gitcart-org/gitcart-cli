# GitCart - Plugin API

Kompletní referenční příručka pro Plugin API a vývoj vlastních pluginů.

## 🔌 Plugin API Přehled

GitCart Plugin API poskytuje výkonný systém pro rozšiřování funkcionality e-shopu bez zásahu do core kódu. Pluginy jsou samostatné JavaScript soubory, které se automaticky načítají a integrují s GitCart systémem.

### Klíčové vlastnosti

- **Self-contained** - Každý plugin je samostatný soubor s konfigurací
- **Event-driven** - Reagování na události v e-shopu
- **Zone rendering** - Vkládání HTML obsahu do templates
- **Hot-loading** - Automatické načítání při změnách
- **Type-safe** - TypeScript definice pro lepší DX
- **Isolace** - Pluginy neovlivňují navzájem svou funkcionalitou

## 🚀 Základní struktura pluginu

### Minimální plugin

```javascript
(function() {
  'use strict';
  
  // Plugin konfigurace
  const CONFIG = {
    name: 'my-plugin',
    version: '1.0.0',
    description: 'My first GitCart plugin',
    enabled: true
  };

  // Plugin logika
  const MyPlugin = {
    init() {
      console.log(`${CONFIG.name} v${CONFIG.version} loaded`);
    }
  };

  // Auto-registrace
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('my-plugin', {
      config: CONFIG,
      init: MyPlugin.init.bind(MyPlugin)
    });
  }
})();
```

### Kompletní plugin struktura

```javascript
(function() {
  'use strict';
  
  // 1. Konfigurace pluginu
  const CONFIG = {
    name: 'advanced-plugin',
    version: '2.1.0',
    description: 'Advanced GitCart plugin with all features',
    author: 'Your Name',
    website: 'https://yoursite.com',
    enabled: true,
    
    // Plugin-specific nastavení
    apiKey: 'your-api-key',
    endpoint: 'https://api.example.com',
    features: {
      tracking: true,
      notifications: false,
      advanced: true
    }
  };

  // 2. Plugin třída/objekt
  const AdvancedPlugin = {
    
    // Inicializace
    init() {
      this.log('Initializing...');
      this.setupEventListeners();
      this.loadSettings();
      this.createUI();
    },

    // Setup po načtení DOM
    setup() {
      this.bindDOMEvents();
      this.startServices();
    },

    // Cleanup při odpojení
    destroy() {
      this.removeEventListeners();
      this.cleanup();
    },

    // Event handlers
    handleProductViewed(productData) {
      if (CONFIG.features.tracking) {
        this.trackProductView(productData);
      }
    },

    handleCartUpdated(cartData) {
      this.updateWidget(cartData);
      this.syncToServer(cartData);
    },

    // Render funkce
    renderProductWidget(context) {
      if (!context.product) return '';
      
      return `
        <div class="${CONFIG.name}-widget">
          <h4>Plugin Content</h4>
          <p>Product: ${context.product.title}</p>
          <button onclick="${CONFIG.name}Action('${context.product.sku}')">
            Plugin Action
          </button>
        </div>
      `;
    },

    // Utility metody
    log(message) {
      if (CONFIG.enabled) {
        console.log(`[${CONFIG.name}]`, message);
      }
    },

    trackProductView(product) {
      // Analytics tracking
    },

    updateWidget(cartData) {
      // Update UI elements
    }
  };

  // 3. Globální funkce (pokud potřeba)
  window[`${CONFIG.name}Action`] = function(sku) {
    AdvancedPlugin.handleAction(sku);
  };

  // 4. Plugin registrace
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin(CONFIG.name, {
      
      // Metadata
      config: CONFIG,
      
      // Lifecycle hooks
      init: AdvancedPlugin.init.bind(AdvancedPlugin),
      setup: AdvancedPlugin.setup.bind(AdvancedPlugin),
      destroy: AdvancedPlugin.destroy.bind(AdvancedPlugin),
      
      // Event listenery
      events: {
        'gitcart:product:viewed': AdvancedPlugin.handleProductViewed.bind(AdvancedPlugin),
        'gitcart:cart:updated': AdvancedPlugin.handleCartUpdated.bind(AdvancedPlugin)
      },
      
      // Render zones
      render: {
        'product-detail-after-images': AdvancedPlugin.renderProductWidget.bind(AdvancedPlugin)
      },
      
      // Public metody
      methods: {
        getConfig: () => CONFIG,
        isEnabled: () => CONFIG.enabled,
        trackEvent: AdvancedPlugin.trackEvent?.bind(AdvancedPlugin)
      }
    });
  } else {
    console.warn(`${CONFIG.name}: GitCart not found - plugin not registered`);
  }
})();
```

## ⚙️ Plugin konfigurace

### CONFIG objekt

```javascript
const CONFIG = {
  // Povinné metadata
  name: 'string',           // Unikátní název pluginu (kebab-case)
  version: 'string',        // Semantic version (1.0.0)
  description: 'string',    // Krátký popis funkcionalitě
  
  // Volitelné metadata  
  author: 'string',         // Autor pluginu
  email: 'string',          // Kontakt na autora
  website: 'string',        // URL webové stránky
  repository: 'string',     // Git repository URL
  license: 'string',        // License (MIT, GPL, atd.)
  
  // Runtime nastavení
  enabled: boolean,         // Zapnout/vypnout plugin (výchozí: true)
  priority: number,         // Priorita načítání (výchozí: 100)
  
  // Dependencies
  requires: ['array'],      // Vyžadované pluginy
  conflicts: ['array'],     // Konfliktní pluginy
  gitcartVersion: 'string', // Minimální verze GitCart
  
  // Plugin-specific konfigurace
  apiKey: 'string',
  endpoint: 'string',
  customOptions: {}
};
```

### Dynamická konfigurace ze site.json

**Nová funkce!** Pluginy mohou načítat konfiguraci přímo ze `site.json`, což umožňuje konfiguraci bez úpravy plugin kódu.

#### 1. Konfigurace v site.json

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

#### 2. Plugin implementace

```javascript
// Plugin configuration - default values
const DEFAULT_CONFIG = {
  name: 'my-plugin',
  version: '1.0.0',
  trackingId: 'YOUR_TRACKING_ID', // Bude přepsáno z site.json
  enableFeature: true
};

const MyPlugin = {
  init() {
    // Načtení konfigurace ze site.json přes GitCart API - automaticky merguje default s site.json
    this.config = GitCart.getPluginConfig('my-plugin', DEFAULT_CONFIG);
    
    console.log('Plugin config:', this.config);
    // { name: 'my-plugin', trackingId: 'G-REAL123', enableFeature: true }
    
    // Použití konfigurace
    if (!this.config.trackingId || this.config.trackingId === 'YOUR_TRACKING_ID') {
      console.warn('Please configure trackingId in site.json plugins section');
      return;
    }
    
    this.setupTracking(this.config.trackingId);
    
    console.log('Using API key:', this.config.apiKey);
  }
};
```

#### 3. Výhody site.json konfigurace

- **🔧 Centralizovaná konfigurace** - všechny plugin nastavení v jednom souboru
- **🔒 Bezpečnost** - žádné API klíče v plugin kódu  
- **🔄 Prostředí** - různá konfigurace pro dev/staging/prod
- **👨‍💼 Non-tech friendly** - marketéři mohou měnit konfigurace
- **📝 Verze** - konfigurace je verzována s projektem
- **🚀 Deployment** - žádné změny kódu při nasazování

#### 4. Dostupné konfigurace

```javascript
// GitCart.getPluginConfig(pluginName, defaultConfig) vrací:
{
  // Vše ze site.json plugins.pluginName sekce
  ...siteJsonConfig,
  
  // Plus automatické hodnoty:
  locale: 'cs-cz',      // Aktuální jazyk
  currency: 'CZK',      // Aktuální měna
  siteUrl: 'https://...' // URL projektu
}
```

## 🎯 Event systém

### Registrace event listenerů

```javascript
const MyPlugin = {
  events: {
    // Jedna událost
    'gitcart:product:viewed': function(productData) {
      this.handleProductView(productData);
    },
    
    // Více událostí (oddělené mezerou)
    'gitcart:cart:item:added gitcart:cart:item:removed': function(data) {
      this.updateCartWidget();
    },
    
    // Wildcard - všechny cart události
    'gitcart:cart:*': function(data, eventName) {
      console.log('Cart event:', eventName, data);
    },
    
    // Custom události
    'myshop:customer:login': function(customerData) {
      this.personalizeExperience(customerData);
    }
  }
};
```

### Dostupné core události

#### Inicializace & Stránka

```javascript
// GitCart inicializace dokončena
'gitcart:initialized': {
  config: object
}

// Načtení stránky
'gitcart:page:loaded': {
  page: 'product|category|home|cart|checkout',
  locale: 'cs-cz',
  url: '/produkty/iphone-15',
  title: 'iPhone 15'
}
```

#### Produktové události

```javascript
// Zobrazení produktu (automaticky na product stránkách)
'gitcart:product:viewed': {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  price: 25999,
  category: 'elektronika',
  url: '/produkty/iphone-15',
  brand: 'Apple',
  inStock: true
}
```

#### Vyhledávání

```javascript
// Vyhledávání inicializováno
'gitcart:search:initialized': {
  languages: ['cs', 'en'],
  config: object
}

// Změna jazyka vyhledávání
'gitcart:search:language:changed': {
  language: 'en'
}

// Hledání produktů
'gitcart:search:query': {
  term: 'iphone',
  filters: {
    category: 'elektronika',
    price: { min: 10000, max: 30000 }
  },
  resultsCount: 15,
  duration: 245 // ms
}

// Aplikování filtrů
'gitcart:search:filtered': {
  results: Array,
  filters: object,
  originalCount: 50,
  filteredCount: 15
}

// Seřazení výsledků
'gitcart:search:sorted': {
  results: Array,
  field: 'price',
  direction: 'asc|desc'
}
```

#### Cart události

```javascript
// Košík načten z úložiště
'gitcart:cart:initialized': {
  items: Array
}

// Přidání do košíku
'gitcart:cart:item:added': {
  sku: 'IPHONE15-128',
  quantity: 1,
  options: {
    variant: 'blue-128gb'
  },
  timestamp: '2024-01-15T10:30:00Z'
}

// Aktualizace položky v košíku (množství, varianta, atd.)
'gitcart:cart:item:updated': {
  item: object,
  oldQuantity: 1,
  newQuantity: 2,
  sku: 'IPHONE15-128',
  variant: 'blue-128gb'
}

// Odebrání z košíku
'gitcart:cart:item:removed': {
  item: object
}

// Update košíku (po jakékoliv změně)
'gitcart:cart:updated': {
  cart: {
    items: Array,
    createdAt: string,
    updatedAt: string
  }
}

// Vymazání košíku
'gitcart:cart:cleared': {
  oldItems: Array
}
```

#### Checkout události

```javascript
// Zahájení checkout
'gitcart:checkout:started': {
  items: Array,
  itemCount: 3,
  subtotal: 45999,
  source: 'cart'
}

// Vyplnění zákaznických dat
'gitcart:checkout:customer:set': {
  customer: {
    firstName: 'Jan',
    lastName: 'Novák',
    email: 'jan@example.com',
    phone: '+420123456789',
    company: '',
    note: ''
  },
  email: 'jan@example.com',
  hasAccount: false,
  isReturningCustomer: true
}

// Výběr dopravy
'gitcart:checkout:shipping:set': {
  shipping: {
    method: 'ppl',
    methodName: 'PPL kurýr',
    price: 149,
    address: object
  },
  method: 'ppl',
  methodName: 'PPL kurýr',
  price: 149,
  deliveryTime: '1-2 dny',
  total: 46148
}

// Výběr platby
'gitcart:checkout:payment:set': {
  payment: {
    method: 'card',
    methodName: 'Platební karta'
  },
  method: 'card',
  methodName: 'Platební karta',
  provider: 'unknown',
  total: 46148
}

// Dokončení objednávky
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
// Plugin zaregistrován
'gitcart:plugin:registered': {
  name: 'google-analytics',
  plugin: object
}
```

### Vlastní události

Pluginy mohou vytvářet vlastní události:

```javascript
const MyPlugin = {
  
  trackProductFavorited(product) {
    // Vlastní událost
    GitCart.emit('myshop:product:favorited', {
      sku: product.sku,
      title: product.title,
      userId: this.getCurrentUserId(),
      timestamp: new Date().toISOString()
    });
  },
  
  // Jiné pluginy mohou poslouchat
  events: {
    'myshop:product:favorited': function(data) {
      console.log('Product favorited by user:', data.userId);
      this.updateWishlist(data);
    }
  }
};
```

## 🎨 Render zones

Zones umožňují pluginům vkládat HTML obsah do konkrétních míst v templates.

### Dostupné zones

```javascript
const AVAILABLE_ZONES = {
  // HTML Head
  'head-meta': 'SEO meta tagy, tracking kódy, CSS',
  'head-scripts': 'JavaScript v hlavičce',
  
  // Body
  'body-start': 'Analytics kódy, tracking pixely',
  'body-end': 'Chat widgety, skripty před </body>',
  
  // Header & Navigation
  'header-start': 'Před navigací',
  'header-end': 'Za navigací',
  'navigation-primary': 'V hlavní navigaci',
  'navigation-secondary': 'V secondary navigaci',
  
  // Product detail
  'product-detail-before-title': 'Před názvem produktu',
  'product-detail-after-title': 'Za názvem produktu',
  'product-detail-after-images': 'Za obrázky produktu',
  'product-detail-after-price': 'Za cenou',
  'product-detail-after-description': 'Za popisem',
  'product-detail-before-add-to-cart': 'Před tlačítkem přidat do košíku',
  'product-detail-after-add-to-cart': 'Za tlačítkem přidat do košíku',
  'product-detail-tabs': 'V tabech produktu',
  'product-detail-end': 'Konec stránky produktu',
  
  // Category listing
  'category-before-products': 'Před seznamem produktů',
  'category-after-products': 'Za seznamem produktů',
  'category-sidebar': 'V sidebar kategorie',
  'product-card-actions': 'Akce na kartě produktu',
  
  // Cart & Checkout
  'cart-item-actions': 'Akce u položky v košíku',
  'cart-summary': 'V souhrnu košíku',
  'cart-before-checkout': 'Před checkout tlačítkem',
  
  'checkout-customer-form': 'Ve formuláři zákazníka',
  'shipping-selection': 'Výběr dopravy',
  'payment-selection': 'Výběr platby',
  'order-summary': 'Souhrn objednávky',
  'order-success-info': 'Thank you page',
  
  // Search & Filters
  'search-results-before': 'Před výsledky hledání',
  'search-results-after': 'Za výsledky',
  'filters-sidebar': 'V filter sidebar',
  
  // Footer & Global
  'before-footer': 'Před footrem',
  'footer-start': 'Začátek footru',
  'footer-end': 'Konec footru',
  'floating-elements': 'Plovoucí elementy (chat, back-to-top)'
};
```

### Render funkce

```javascript
const MyPlugin = {
  
  render: {
    
    // Statický HTML
    'product-detail-after-images': function(context) {
      return '<div class="my-plugin-banner">Special Offer!</div>';
    },
    
    // Dynamický obsah podle kontextu
    'product-detail-after-price': function(context) {
      const product = context.product;
      if (!product || product.price < 1000) return '';
      
      return `
        <div class="free-shipping-badge">
          <i class="icon-truck"></i>
          <span>Free shipping for orders over ${this.formatPrice(1000)}</span>
        </div>
      `;
    },
    
    // Conditional rendering
    'head-meta': function(context) {
      if (context.page !== 'product') return '';
      
      const product = context.product;
      return `
        <meta name="product:availability" content="${product.inStock ? 'instock' : 'outofstock'}">
        <meta name="product:condition" content="new">
        <meta name="product:price:amount" content="${product.price}">
        <meta name="product:price:currency" content="${product.currency}">
      `;
    },
    
    // Komplexní widget
    'cart-summary': function(context) {
      const cart = context.cart;
      if (!cart || cart.itemCount === 0) return '';
      
      const savings = this.calculateSavings(cart);
      if (savings <= 0) return '';
      
      return `
        <div class="savings-widget">
          <div class="savings-amount">
            You saved ${this.formatPrice(savings)}!
          </div>
          <div class="savings-details">
            ${this.getSavingsBreakdown(cart)}
          </div>
        </div>
      `;
    }
  }
};
```

### Render context

Každá render funkce dostává context objekt s relevantními daty:

```javascript
// Context pro product pages
{
  page: 'product',
  locale: 'cs-cz',
  product: {
    sku: 'IPHONE15-128',
    title: 'iPhone 15',
    price: 25999,
    category: 'elektronika',
    inStock: true,
    images: [...],
    // ... všechna product data
  },
  category: {
    title: 'Elektronika',
    url: '/elektronika',
    // ... category data
  },
  site: {
    name: 'Můj E-shop',
    url: 'https://myshop.com',
    // ... site config
  }
}

// Context pro cart pages
{
  page: 'cart',
  cart: {
    items: [...],
    itemCount: 3,
    subtotal: 45999,
    total: 46148
  },
  shipping: {
    method: 'ppl',
    price: 149
  }
}
```

### Advanced rendering

```javascript
const AdvancedRenderer = {
  
  render: {
    
    // Async rendering s loading state
    'product-detail-end': async function(context) {
      const reviews = await this.loadReviews(context.product.sku);
      
      return `
        <div class="product-reviews">
          <h3>Customer Reviews (${reviews.length})</h3>
          ${reviews.map(review => this.renderReview(review)).join('')}
        </div>
      `;
    },
    
    // Conditional na základě user permissions
    'product-detail-after-price': function(context) {
      if (!this.userHasPermission('see_dealer_prices')) return '';
      
      const dealerPrice = context.product.dealerPrice;
      return `
        <div class="dealer-price">
          <span>Dealer Price: ${this.formatPrice(dealerPrice)}</span>
        </div>
      `;
    },
    
    // Responsive rendering
    'category-before-products': function(context) {
      const isMobile = window.innerWidth < 768;
      
      if (isMobile) {
        return '<div class="mobile-category-banner">Mobile Banner</div>';
      } else {
        return `
          <div class="desktop-category-hero">
            <h1>${context.category.title}</h1>
            <p>${context.category.description}</p>
          </div>
        `;
      }
    }
  }
};
```

## 🛠️ Plugin metody a utility

### Vestavěné utility metody

Pluginy mají přístup k utility metodám přes `this` kontext:

```javascript
const MyPlugin = {
  
  init() {
    // Formátování ceny
    const formatted = this.formatPrice(25999, 'CZK');
    console.log(formatted); // "25 999 Kč"
    
    // Formátování data
    const date = this.formatDate(new Date(), 'cs-cz');
    
    // HTTP požadavky
    const data = await this.http.get('/api/data');
    await this.http.post('/api/track', { event: 'plugin_loaded' });
    
    // Storage
    this.storage.set('plugin_data', { initialized: true });
    const data = this.storage.get('plugin_data');
    
    // DOM utility
    const element = this.dom.create('div', { class: 'plugin-widget' });
    this.dom.append(document.body, element);
    
    // Event utility
    this.events.on(document, 'click', '.my-button', this.handleClick.bind(this));
  },
  
  handleClick(event) {
    event.preventDefault();
    console.log('Button clicked');
  }
};
```

### HTTP client

```javascript
const MyPlugin = {
  
  async loadData() {
    try {
      // GET request
      const products = await this.http.get('/api/products');
      
      // POST s daty
      const result = await this.http.post('/api/analytics', {
        event: 'product_viewed',
        sku: 'IPHONE15-128',
        timestamp: Date.now()
      });
      
      // PUT/PATCH
      await this.http.put('/api/user/preferences', userPrefs);
      await this.http.patch('/api/product/IPHONE15-128', updates);
      
      // DELETE
      await this.http.delete('/api/cart/item/ABC123');
      
      // Konfigurace
      const data = await this.http.request('/api/data', {
        method: 'GET',
        headers: {
          'Authorization': 'Bearer ' + this.config.apiToken,
          'Content-Type': 'application/json'
        },
        timeout: 10000
      });
      
    } catch (error) {
      this.log('HTTP error:', error);
      this.handleError(error);
    }
  }
};
```

### Storage utility

```javascript
const MyPlugin = {
  
  saveUserPreferences(prefs) {
    // Uložení do localStorage
    this.storage.set(`${this.config.name}_prefs`, prefs);
    
    // S expirací (24 hodin)
    this.storage.set('temp_data', data, 24 * 60 * 60 * 1000);
    
    // Session storage
    this.storage.session.set('cart_state', cartData);
  },
  
  loadUserPreferences() {
    // S fallback hodnotou
    return this.storage.get(`${this.config.name}_prefs`, {
      theme: 'light',
      language: 'cs-cz'
    });
  },
  
  clearPluginData() {
    // Vymazání plugin dat
    this.storage.remove(`${this.config.name}_prefs`);
    this.storage.removeAll(this.config.name); // Všechny keys obsahující název
  }
};
```

### DOM utility

```javascript
const MyPlugin = {
  
  createUI() {
    // Vytvoření elementu
    const widget = this.dom.create('div', {
      class: 'my-plugin-widget',
      id: 'my-widget',
      'data-plugin': this.config.name
    });
    
    // Přidání obsahu
    widget.innerHTML = `
      <h3>Plugin Widget</h3>
      <button class="action-button">Click me</button>
    `;
    
    // Vložení do stránky
    const container = this.dom.find('.plugin-container') || document.body;
    this.dom.append(container, widget);
    
    // Event listener
    this.dom.on(widget, 'click', '.action-button', this.handleAction.bind(this));
  },
  
  updateWidget(data) {
    const widget = this.dom.find('#my-widget');
    if (widget) {
      this.dom.find('.data-display', widget).textContent = data.value;
    }
  },
  
  removeWidget() {
    const widget = this.dom.find('#my-widget');
    if (widget) {
      this.dom.remove(widget);
    }
  },
  
  handleAction(event) {
    const button = event.target;
    this.dom.addClass(button, 'loading');
    
    setTimeout(() => {
      this.dom.removeClass(button, 'loading');
      this.dom.addClass(button, 'success');
    }, 1000);
  }
};
```

## 🔒 Plugin security

### Input validace

```javascript
const SecurePlugin = {
  
  validateUserInput(data) {
    // Validace typů
    if (typeof data.email !== 'string') {
      throw new Error('Email must be a string');
    }
    
    // Email validace
    if (!this.isValidEmail(data.email)) {
      throw new Error('Invalid email format');
    }
    
    // Sanitizace HTML
    data.message = this.sanitizeHtml(data.message);
    
    // Length limits
    if (data.message.length > 1000) {
      throw new Error('Message too long');
    }
    
    return data;
  },
  
  isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  },
  
  sanitizeHtml(html) {
    return html
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#39;');
  }
};
```

### API security

```javascript
const SecurePlugin = {
  
  async sendToAPI(data) {
    try {
      // Validace před odesláním
      const validatedData = this.validateUserInput(data);
      
      // Rate limiting
      if (!this.checkRateLimit()) {
        throw new Error('Rate limit exceeded');
      }
      
      // Authenticated request
      const response = await this.http.post('/api/secure-endpoint', validatedData, {
        headers: {
          'Authorization': `Bearer ${this.getApiToken()}`,
          'X-Plugin-Version': this.config.version,
          'X-Site-Origin': window.location.origin
        }
      });
      
      return response;
      
    } catch (error) {
      // Log bez sensitive dat
      this.log('API error:', {
        message: error.message,
        endpoint: '/api/secure-endpoint',
        timestamp: new Date().toISOString()
      });
      
      throw error;
    }
  },
  
  checkRateLimit() {
    const now = Date.now();
    const windowStart = now - 60000; // 1 minuta
    
    // Počet requestů v posledních 60 sekundách
    this.apiCalls = this.apiCalls || [];
    this.apiCalls = this.apiCalls.filter(time => time > windowStart);
    
    if (this.apiCalls.length >= 10) { // Max 10 per minute
      return false;
    }
    
    this.apiCalls.push(now);
    return true;
  }
};
```

### Error handling

```javascript
const RobustPlugin = {
  
  init() {
    try {
      this.setupPlugin();
    } catch (error) {
      this.handleError('Plugin initialization failed', error);
    }
  },
  
  async handleOrderCompleted(orderData) {
    try {
      await this.processOrder(orderData);
    } catch (error) {
      // Log error bez expose sensitive dat
      this.handleError('Order processing failed', error, {
        orderId: orderData.id,
        step: 'process_order'
      });
      
      // Neblokuj user workflow
      return;
    }
  },
  
  handleError(message, error, context = {}) {
    const errorInfo = {
      plugin: this.config.name,
      message: message,
      error: error.message,
      stack: error.stack,
      context: context,
      timestamp: new Date().toISOString(),
      url: window.location.href,
      userAgent: navigator.userAgent
    };
    
    // Local logging
    console.error(`[${this.config.name}] ${message}:`, errorInfo);
    
    // Send to error tracking (bez sensitive dat)
    this.sendErrorReport({
      plugin: errorInfo.plugin,
      message: errorInfo.message,
      error: errorInfo.error,
      timestamp: errorInfo.timestamp
    });
  },
  
  sendErrorReport(errorData) {
    // Rate limited error reporting
    const key = `error_${errorData.error}`;
    const lastSent = this.storage.get(`last_error_${key}`);
    const now = Date.now();
    
    // Pouze jednou za 5 minut pro stejnou chybu
    if (lastSent && (now - lastSent) < 300000) {
      return;
    }
    
    this.storage.set(`last_error_${key}`, now);
    
    // Async send without blocking
    setTimeout(() => {
      this.http.post('/api/errors', errorData).catch(() => {
        // Silent fail pro error reporting
      });
    }, 0);
  }
};
```

## 📊 Plugin analytics a metrics

### Performance monitoring

```javascript
const AnalyticsPlugin = {
  
  init() {
    this.startTime = performance.now();
    this.metrics = {
      apiCalls: 0,
      renderTime: 0,
      eventsFired: 0
    };
  },
  
  // Wrapper pro měření výkonu
  measure(operation, fn) {
    return async (...args) => {
      const start = performance.now();
      
      try {
        const result = await fn.apply(this, args);
        const duration = performance.now() - start;
        
        this.recordMetric(operation, duration);
        return result;
        
      } catch (error) {
        const duration = performance.now() - start;
        this.recordError(operation, error, duration);
        throw error;
      }
    };
  },
  
  recordMetric(operation, duration) {
    this.metrics[operation] = this.metrics[operation] || [];
    this.metrics[operation].push({
      duration: duration,
      timestamp: Date.now()
    });
    
    // Performance warning
    if (duration > 100) {
      console.warn(`[${this.config.name}] Slow operation: ${operation} took ${duration.toFixed(2)}ms`);
    }
  },
  
  getPerformanceReport() {
    const report = {
      plugin: this.config.name,
      uptime: performance.now() - this.startTime,
      metrics: {}
    };
    
    for (const [operation, measurements] of Object.entries(this.metrics)) {
      if (Array.isArray(measurements)) {
        const durations = measurements.map(m => m.duration);
        report.metrics[operation] = {
          calls: durations.length,
          avgDuration: durations.reduce((a, b) => a + b, 0) / durations.length,
          maxDuration: Math.max(...durations),
          minDuration: Math.min(...durations)
        };
      }
    }
    
    return report;
  }
};
```

### Usage tracking

```javascript
const TrackingPlugin = {
  
  init() {
    this.setupUsageTracking();
  },
  
  setupUsageTracking() {
    // Track plugin usage
    this.trackEvent('plugin_initialized', {
      version: this.config.version,
      features: Object.keys(this.config.features || {})
    });
    
    // Track feature usage
    this.originalMethods = {};
    this.wrapMethodsWithTracking();
  },
  
  wrapMethodsWithTracking() {
    const methodsToTrack = ['handleProductViewed', 'handleCartUpdated', 'renderWidget'];
    
    methodsToTrack.forEach(methodName => {
      if (typeof this[methodName] === 'function') {
        this.originalMethods[methodName] = this[methodName];
        this[methodName] = (...args) => {
          this.trackEvent(`method_${methodName}`, { args: args.length });
          return this.originalMethods[methodName].apply(this, args);
        };
      }
    });
  },
  
  trackEvent(eventName, data = {}) {
    const eventData = {
      plugin: this.config.name,
      event: eventName,
      timestamp: Date.now(),
      url: window.location.pathname,
      ...data
    };
    
    // Local storage pro offline tracking
    const events = this.storage.get('plugin_events', []);
    events.push(eventData);
    
    // Keep only last 100 events
    if (events.length > 100) {
      events.splice(0, events.length - 100);
    }
    
    this.storage.set('plugin_events', events);
    
    // Send to analytics service
    this.sendAnalytics(eventData);
  },
  
  async sendAnalytics(eventData) {
    try {
      await this.http.post('/api/plugin-analytics', eventData);
    } catch (error) {
      // Silent fail pro analytics
      console.debug('Analytics send failed:', error.message);
    }
  }
};
```

## 🧪 Plugin testing

### Unit testing

```javascript
// __tests__/plugins/my-plugin.test.js
describe('My Plugin', () => {
  let plugin;
  let mockGitCart;
  
  beforeEach(() => {
    // Mock GitCart
    mockGitCart = {
      plugin: jest.fn(),
      on: jest.fn(),
      emit: jest.fn(),
      cart: {
        getItems: jest.fn().mockReturnValue([]),
        add: jest.fn()
      }
    };
    
    global.GitCart = mockGitCart;
    
    // Load plugin
    delete require.cache[require.resolve('../../plugins/my-plugin.js')];
    require('../../plugins/my-plugin.js');
    
    // Get plugin instance
    plugin = mockGitCart.plugin.mock.calls[0][1];
  });

  test('should register with correct config', () => {
    expect(mockGitCart.plugin).toHaveBeenCalledWith('my-plugin', expect.objectContaining({
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
    
    const eventHandler = plugin.events['gitcart:product:viewed'];
    expect(eventHandler).toBeDefined();
    
    expect(() => eventHandler(mockProduct)).not.toThrow();
  });

  test('should render content for product zone', () => {
    const context = {
      product: {
        sku: 'TEST-123',
        title: 'Test Product',
        price: 999
      }
    };
    
    const renderer = plugin.render['product-detail-after-images'];
    const result = renderer(context);
    
    expect(result).toContain('Test Product');
    expect(result).toContain('plugin-content');
  });

  test('should handle API errors gracefully', async () => {
    // Mock failed API call
    const httpMock = jest.fn().mockRejectedValue(new Error('API Error'));
    plugin.http = { post: httpMock };
    
    const consoleErrorSpy = jest.spyOn(console, 'error').mockImplementation();
    
    await plugin.sendData({ test: 'data' });
    
    expect(consoleErrorSpy).toHaveBeenCalledWith(
      expect.stringContaining('API Error')
    );
    
    consoleErrorSpy.mockRestore();
  });
});
```

### Integration testing

```javascript
// __tests__/plugins/integration.test.js
describe('Plugin Integration', () => {
  let page;
  
  beforeAll(async () => {
    page = await browser.newPage();
    await page.goto('http://localhost:3000/test-product');
  });

  test('plugin should load and initialize', async () => {
    const pluginLoaded = await page.evaluate(() => {
      return window.GitCart && window.GitCart.getPlugin('my-plugin');
    });
    
    expect(pluginLoaded).toBeTruthy();
  });

  test('plugin should render content in zones', async () => {
    const pluginContent = await page.$('.my-plugin-widget');
    expect(pluginContent).toBeTruthy();
    
    const text = await page.evaluate(el => el.textContent, pluginContent);
    expect(text).toContain('Plugin Content');
  });

  test('plugin should handle cart events', async () => {
    // Simulate add to cart
    await page.click('.add-to-cart-button');
    
    // Wait for plugin to process event
    await page.waitForTimeout(100);
    
    const pluginResponse = await page.evaluate(() => {
      return window.testPluginLastEvent;
    });
    
    expect(pluginResponse).toMatchObject({
      type: 'gitcart:cart:item:added',
      sku: expect.any(String)
    });
  });
});
```

## 📦 Plugin distribuce

### NPM package

```json
{
  "name": "gitcart-plugin-my-plugin",
  "version": "1.0.0",
  "description": "My awesome GitCart plugin",
  "main": "dist/my-plugin.js",
  "keywords": ["gitcart", "plugin", "ecommerce"],
  "author": "Your Name",
  "license": "MIT",
  "peerDependencies": {
    "gitcart-cli": ">=1.0.0"
  },
  "gitcart": {
    "plugin": true,
    "compatibility": ">=1.0.0",
    "category": "analytics"
  }
}
```

### Plugin instalace

```bash
# Instalace z npm
npm install gitcart-plugin-my-plugin
gitcart plugin add gitcart-plugin-my-plugin

# Z GitHub
gitcart plugin add github:user/gitcart-plugin-repo

# Lokální soubor
gitcart plugin add ./plugins/my-plugin.js
```

### Plugin registry

```javascript
// Registrace v GitCart plugin registry
const PLUGIN_MANIFEST = {
  name: 'my-plugin',
  version: '1.0.0',
  description: 'My awesome plugin',
  category: 'analytics',
  tags: ['tracking', 'ecommerce', 'gtag'],
  compatibility: '>=1.0.0',
  
  // Screenshots a demo
  screenshots: [
    'https://example.com/screenshot1.png'
  ],
  demo: 'https://demo.example.com',
  
  // Support
  homepage: 'https://github.com/user/my-plugin',
  repository: 'https://github.com/user/my-plugin',
  bugs: 'https://github.com/user/my-plugin/issues',
  
  // Rating a stats
  rating: 4.8,
  downloads: 1250,
  lastUpdated: '2024-01-15T10:30:00Z'
};
```

## 🎯 Příklady pokročilých pluginů

### Multi-provider Analytics Plugin

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'multi-analytics',
    version: '2.0.0',
    description: 'Multi-provider analytics tracking',
    
    providers: {
      googleAnalytics: {
        enabled: true,
        measurementId: 'G-XXXXXXXXXX'
      },
      facebookPixel: {
        enabled: true,
        pixelId: 'your-pixel-id'
      },
      customAnalytics: {
        enabled: false,
        endpoint: '/api/analytics'
      }
    }
  };

  const MultiAnalytics = {
    
    init() {
      this.initializeProviders();
      this.setupEventListeners();
    },
    
    initializeProviders() {
      if (CONFIG.providers.googleAnalytics.enabled) {
        this.initGA4();
      }
      
      if (CONFIG.providers.facebookPixel.enabled) {
        this.initFacebookPixel();
      }
      
      if (CONFIG.providers.customAnalytics.enabled) {
        this.initCustomAnalytics();
      }
    },
    
    initGA4() {
      // Google Analytics 4 initialization
      const script = document.createElement('script');
      script.src = `https://www.googletagmanager.com/gtag/js?id=${CONFIG.providers.googleAnalytics.measurementId}`;
      document.head.appendChild(script);
      
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      window.gtag = gtag;
      gtag('js', new Date());
      gtag('config', CONFIG.providers.googleAnalytics.measurementId);
    },
    
    trackPurchase(orderData) {
      const purchaseData = this.formatPurchaseData(orderData);
      
      // GA4
      if (CONFIG.providers.googleAnalytics.enabled && window.gtag) {
        gtag('event', 'purchase', purchaseData.ga4);
      }
      
      // Facebook Pixel
      if (CONFIG.providers.facebookPixel.enabled && window.fbq) {
        fbq('track', 'Purchase', purchaseData.facebook);
      }
      
      // Custom Analytics
      if (CONFIG.providers.customAnalytics.enabled) {
        this.http.post(CONFIG.providers.customAnalytics.endpoint, {
          event: 'purchase',
          data: purchaseData.custom
        });
      }
    },
    
    formatPurchaseData(orderData) {
      return {
        ga4: {
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
        },
        
        facebook: {
          content_ids: orderData.items.map(item => item.sku),
          content_type: 'product',
          value: orderData.totals.total,
          currency: orderData.currency,
          num_items: orderData.items.length
        },
        
        custom: {
          orderId: orderData.id,
          total: orderData.totals.total,
          currency: orderData.currency,
          items: orderData.items,
          customer: orderData.customer,
          timestamp: orderData.timestamp
        }
      };
    }
  };

  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('multi-analytics', {
      config: CONFIG,
      
      events: {
        'gitcart:product:viewed': MultiAnalytics.trackProductView.bind(MultiAnalytics),
        'gitcart:cart:item:added': MultiAnalytics.trackAddToCart.bind(MultiAnalytics),
        'gitcart:checkout:completed': MultiAnalytics.trackPurchase.bind(MultiAnalytics)
      },
      
      init: MultiAnalytics.init.bind(MultiAnalytics)
    });
  }
})();
```

Plugin API GitCart poskytuje výkonné nástroje pro vytváření pokročilých e-commerce rozšíření s minimální komplexitou a maximální flexibilitou.