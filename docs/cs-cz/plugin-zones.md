# Plugin Zones - Průvodce pro vývojáře

Plugin Zones jsou místa v šablonách GitCartu, kde mohou pluginy vkládat svůj obsah. Tento systém umožňuje snadné rozšíření funkcionality e-shopu bez potřeby úprav šablon.

## Jak fungují zóny

Zóny jsou implementovány jako prázdné `<div>` elementy s prefixem `zone-` v ID:

```html
<div id="zone-after-header"></div>
```

Pluginy poté mohou do těchto zón vkládat obsah pomocí `render` sekce:

```javascript
GitCart.plugin('my-plugin', {
  render: {
    'zone-after-header': function(context) {
      return '<div class="banner">Speciální nabídka!</div>';
    }
  }
});
```

## Dostupné zóny

### 🏗️ Layout zóny (na všech stránkách)

| Zóna | Popis | Ideální pro |
|------|-------|-------------|
| `zone-head` | V `<head>` sekci (**server-side**) | Meta tagy, analytics |
| `zone-body-start` | Po otevření `<body>` tagu | Cookie lišty, loading indikátory |
| `zone-after-header` | Po header/navigaci | Bannery, oznámení |
| `zone-after-main` | Po hlavním obsahu | Newsletters, reklamy |
| `zone-before-footer` | Před footerem | CTA sekce, odkazy |
| `zone-footer-start` | V footer obsahu | Dodatečné informace |
| `zone-after-footer` | Po footeru | Tracking kódy |
| `zone-body-end` | Před uzavřením `</body>` | Bottom skripty |

### 🛍️ Produktové zóny

| Zóna | Popis | Ideální pro |
|------|-------|-------------|
| `zone-product-start` | Začátek produktu | Breadcrumbs, tracking |
| `zone-after-product-images` | Po galerii obrázků | Zoom tlačítka, lightbox |
| `zone-after-product-price` | Po ceně | Slevové značky, badges |
| `zone-after-add-to-cart` | Po tlačítku "koupit" | Cross-sell, související produkty |
| `zone-after-product-description` | Po popisu produktu | Reviews, Q&A, specifikace |
| `zone-product-end` | Konec produktu | Tracking, more products |

### 📂 Kategorie zóny

| Zóna | Popis | Ideální pro |
|------|-------|-------------|
| `zone-category-start` | Začátek kategorie | Breadcrumbs, tracking |
| `zone-after-category-header` | Po header kategorie | Filtry, sorting UI |
| `zone-before-products` | Před výpisem produktů | Bannery, reklamy |
| `zone-after-products` | Po výpisu produktů | Load more tlačítka, info |
| `zone-category-end` | Konec kategorie | Related categories |
| `zone-product-card-actions` | V product kartách | Wishlist, compare tlačítka |

### 🛒 Košík a checkout

| Zóna | Popis | Ideální pro |
|------|-------|-------------|
| `zone-cart-summary` | V souhrnu košíku | Info o poštovném, slevách |
| `zone-cart-before-checkout` | Před checkout tlačítkem | Slevové kódy, podmínky |
| `zone-shipping-methods` | Shipping možnosti | Dodatečné dopravce |
| `zone-payment-methods` | Payment možnosti | Nové platební brány |

### 🏠 Homepage

| Zóna | Popis | Ideální pro |
|------|-------|-------------|
| `zone-homepage-start` | Začátek homepage | Tracking, cookie consent |
| `zone-after-hero` | Po hero sekci | Speciální nabídky |
| `zone-before-featured-products` | Před featured produkty | Kategorie menu |
| `zone-homepage-end` | Konec homepage | Newsletter, social linky |

### 🔍 Vyhledávání a ostatní

| Zóna | Popis | Ideální pro |
|------|-------|-------------|
| `zone-before-search-results` | Před search výsledky | Filtry, sorting |
| `zone-after-search-results` | Po search výsledcích | Related content, reklamy |
| `zone-newsletter-signup` | Newsletter signup | Email formuláře |

## Context objekt

Každá zóna function dostává `context` parameter s aktuálními informacemi o stránce:

```javascript
const context = {
  // Informace o aktuální stránce
  page: {
    type: 'product',           // 'product', 'category', 'homepage', 'checkout-success', etc.
    url: '/elektronika/iphone-15/',
    title: 'iPhone 15',
    lang: 'cs'
  },
  
  // Informace o webu
  site: {
    name: 'Můj e-shop',
    url: 'https://myshop.com'
  },
  
  // Košík (pokud je dostupný)
  cart: {
    items: [...],
    count: 3,
    total: 25999
  },
  
  // Aktuální produkt (pouze na produktových stránkách)
  product: {
    sku: 'IPHONE15-128',
    title: 'iPhone 15',
    price: 25999,
    // ... další product data
  },
  
  // Aktuální kategorie (pouze na category stránkách)
  category: {
    slug: 'elektronika',
    title: 'Elektronika',
    // ... další category data
  }
};
```

## Praktické příklady použití

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
        currency: 'CZK',
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
      // Pouze na produktových stránkách
      if (!context.product) return '';
      
      return `
        <div class="product-reviews">
          <h3>Hodnocení zákazníků</h3>
          <div class="reviews-list">
            <div class="review">
              <div class="rating">⭐⭐⭐⭐⭐</div>
              <p>"Skvělý produkt, doporučuji!"</p>
              <small>- Jan Novák</small>
            </div>
          </div>
          <button onclick="showReviewForm()">Přidat hodnocení</button>
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

### 3. Special Offers plugin s kontextem

```javascript
GitCart.plugin('special-offers', {
  render: {
    'zone-after-header': function(context) {
      // Zobrazit pouze na homepage
      if (context.page.type !== 'homepage') return '';
      
      return `
        <div class="special-offer-banner">
          <h3>🎉 Speciální nabídka dne!</h3>
          <p>Sleva 15% na všechny produkty v kategorii elektronika</p>
        </div>
      `;
    },
    
    'zone-after-product-price': function(context) {
      // Zobrazit pouze u produktů v kategorii elektronika
      if (!context.product || !context.product.category || context.product.category !== 'elektronika') {
        return '';
      }
      
      const discountPrice = Math.round(context.product.price * 0.85);
      
      return `
        <div class="product-special-offer">
          <span class="discount-badge">-15%</span>
          <p class="discount-price">Speciální cena: ${formatPrice(discountPrice)} CZK</p>
          <small>Nabídka platí pouze dnes!</small>
        </div>
      `;
    }
  }
});
```

### 4. Cart Discount plugin

```javascript
GitCart.plugin('cart-discounts', {
  render: {
    'zone-cart-summary': function(context) {
      // Pouze pokud je košík neprázdný
      if (!context.cart || context.cart.count === 0) return '';
      
      const total = context.cart.total || 0;
      
      if (total > 1000) {
        return '<div class="discount-info">🎉 Doprava zdarma při nákupu nad 1000 Kč!</div>';
      } else {
        const remaining = 1000 - total;
        return `<div class="discount-info">📦 Do dopravy zdarma vám zbývá ${remaining} Kč</div>`;
      }
    },
    
    'zone-cart-before-checkout': function(context) {
      return `
        <div class="coupon-section">
          <label>Slevový kód:</label>
          <input type="text" id="coupon-code" placeholder="Zadejte slevový kód">
          <button onclick="applyCoupon()">Použít</button>
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
          <h4>Nezmeškejte novinky!</h4>
          <form onsubmit="return subscribeNewsletter(event)">
            <input type="email" placeholder="Váš email" required>
            <button type="submit">Odebírat</button>
          </form>
        </div>
      `;
    },
    
    'zone-after-hero': function(context) {
      // Newsletter popup na homepage
      if (context.page && context.page.type === 'homepage') {
        return `
          <div class="newsletter-popup" id="newsletter-popup" style="display:none;">
            <div class="popup-content">
              <h3>Získejte 10% slevu!</h3>
              <p>Přihlaste se k odběru novinek a získejte slevu na první nákup.</p>
              <form onsubmit="return subscribeNewsletter(event, true)">
                <input type="email" placeholder="Váš email" required>
                <button type="submit">Získat slevu</button>
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
    alert('Děkujeme! Váš slevový kód: WELCOME10');
    closeNewsletterPopup();
  } else {
    alert('Děkujeme za přihlášení!');
  }
  return false;
}
```

## Vytváření vlastních zón

Pokud potřebujete vlastní zóny, jednoduše je přidejte do svých šablon:

```html
<!-- V šabloně -->
<div class="my-custom-section">
  <h2>Moje sekce</h2>
  <div id="zone-my-custom-zone"></div>
</div>
```

Poté je můžete použít v pluginech:

```javascript
GitCart.plugin('my-custom-plugin', {
  render: {
    'zone-my-custom-zone': function(context) {
      return '<p>Vlastní obsah!</p>';
    }
  }
});
```

## Server-side vs Client-side

### Server-side zóny
- **Pouze `zone-head`** - zpracovává se během buildu
- Použijte `render` sekci v pluginu
- Ideální pro kritický obsah (meta tagy, analytics)

### Client-side zóny
- **Všechny ostatní zóny** - zpracovává se v prohlížeči
- Použijte `render` sekci v pluginu
- Flexibilnější, reaguje na user akce
- Mají přístup k `context` objektu s daty stránky

## Tips a best practices

1. **Používejte `context` objekt** - obsahuje data stránky (product, category, cart...)
2. **Kontrolujte kontext** - vracejte prázdný string pokud zóna není relevantní
3. **Používejte CSS třídy** pro styling místo inline stylů
4. **Testujte na mobilních zařízeních** - některé zóny mohou být skryté
5. **Cachujte render funkce** - složité výpočty ukládejte do proměnných
6. **Sledujte performance** - složité render funkce mohou zpomalit načítání

Více informací najdete v [kompletní dokumentaci pluginů](./plugins.md).