# GitCart - Frontend API

Kompletní referenční příručka pro GitCart.js frontend API.

## 🎯 Přehled Frontend API

GitCart.js poskytuje kompletní JavaScript API pro e-commerce funkcionalité v prohlížeči. API je navrženo jako lightweight, modulární a snadno rozšiřitelná knihovna.

### Klíčové vlastnosti

- **Plug & Play** - Žádná konfigurace, funguje out-of-the-box
- **localStorage persistence** - Košík a data přežijí reload stránky
- **Event-driven** - Modulární architektura s událostmi
- **Plugin systém** - Snadno rozšiřitelné funkcionalitou
- **TypeScript ready** - Kompletní type definitions
- **Lightweight** - < 15kb minified + gzipped

## 🚀 Inicializace a základní použití

### Automatické načtení

GitCart.js se automaticky načte ve všech GitCart templates:

```html
<!-- Automaticky vloženo do <head> -->
<script src="/assets/js/gitcart.js"></script>

<script>
// GitCart je dostupné globálně
console.log(GitCart.version); // "1.0.0"

// Automatická inicializace při DOMContentLoaded
document.addEventListener('DOMContentLoaded', function() {
  // GitCart je připravené k použití
  console.log('Košík má', GitCart.cart.getItemCount(), 'položek');
});
</script>
```

### Manuální inicializace

```javascript
// Manuální inicializace s konfigurací
GitCart.init({
  currency: 'CZK',
  locale: 'cs-cz',
  debug: true,
  
  // E-commerce nastavení
  cart: {
    persistent: true,
    storageKey: 'gitcart_cart'
  },
  
  // Checkout nastavení
  checkout: {
    requireEmail: true,
    requirePhone: true,
    validateAddress: true
  }
});
```

## 🛒 Cart API

Správa nákupního košíku s persistence v localStorage.

### `GitCart.cart.add(sku, quantity, options)`

Přidá produkt do košíku.

```javascript
// Základní přidání produktu
GitCart.cart.add('IPHONE15-128', 1);

// S dodatečnými možnostmi
GitCart.cart.add('IPHONE15-128', 2, {
  variant: 'blue-128gb',
  customization: 'engraving',
  note: 'Vánoční dárek',
  giftWrap: true
});

// S custom cenou (akční cena)
GitCart.cart.add('IPHONE15-128', 1, {
  price: 23999, // Přepíše výchozí cenu
  originalPrice: 25999
});
```

#### Parametry

| Parametr | Typ | Povinný | Popis |
|----------|-----|---------|-------|
| `sku` | string | ✅ | Unikátní identifikátor produktu |
| `quantity` | number | - | Množství (výchozí: 1) |
| `options` | object | - | Dodatečné možnosti |

#### Options

```javascript
{
  variant: 'string',          // Varianta produktu
  price: number,             // Custom cena
  originalPrice: number,     // Původní cena pro slevy
  title: 'string',          // Název produktu (fallback)
  image: 'string',          // URL obrázku
  url: 'string',            // URL produktu
  category: 'string',       // Kategorie
  customization: 'object',  // Customizace
  note: 'string',           // Poznámka
  giftWrap: boolean,        // Dárkové balení
  metadata: object          // Vlastní metadata
}
```

### `GitCart.cart.remove(sku, options)`

Odebere produkt z košíku.

```javascript
// Odebrání produktu
GitCart.cart.remove('IPHONE15-128');

// Odebrání specifické varianty
GitCart.cart.remove('IPHONE15-128', { variant: 'blue-128gb' });
```

### `GitCart.cart.updateQuantity(sku, quantity, options)`

Aktualizuje množství produktu.

```javascript
// Změna množství
GitCart.cart.updateQuantity('IPHONE15-128', 3);

// Pro specifickou variantu
GitCart.cart.updateQuantity('IPHONE15-128', 2, { variant: 'blue-128gb' });

// Nastavení na 0 = odebrání
GitCart.cart.updateQuantity('IPHONE15-128', 0);
```

### `GitCart.cart.clear()`

Vymaže celý košík.

```javascript
// Vymazání košíku
GitCart.cart.clear();

// S potvrzením
if (confirm('Opravdu chcete vymazat košík?')) {
  GitCart.cart.clear();
}
```

### `GitCart.cart.getItems()`

Vrátí všechny položky v košíku.

```javascript
const items = GitCart.cart.getItems();
console.log(items);

// Výstup:
[
  {
    sku: 'IPHONE15-128',
    title: 'iPhone 15 128GB',
    price: 23999,
    originalPrice: 25999,
    quantity: 1,
    variant: 'blue-128gb',
    subtotal: 23999,
    image: '/assets/images/iphone-15.jpg',
    url: '/elektronika/iphone-15/',
    category: 'elektronika',
    addedAt: '2024-01-15T10:30:00Z',
    options: {
      giftWrap: true,
      note: 'Vánoční dárek'
    }
  },
  // ... další položky
]
```

### `GitCart.cart.getItem(sku, options)`

Vrátí konkrétní položku z košíku.

```javascript
// Získání položky
const item = GitCart.cart.getItem('IPHONE15-128');

// S variantou
const item = GitCart.cart.getItem('IPHONE15-128', { variant: 'blue-128gb' });

if (item) {
  console.log(`V košíku: ${item.quantity}x ${item.title}`);
}
```

### `GitCart.cart.hasItem(sku, options)`

Zkontroluje, zda je produkt v košíku.

```javascript
// Kontrola existence
if (GitCart.cart.hasItem('IPHONE15-128')) {
  console.log('iPhone je v košíku');
}

// S variantou
if (GitCart.cart.hasItem('IPHONE15-128', { variant: 'blue-128gb' })) {
  console.log('Modrý iPhone je v košíku');
}
```

### `GitCart.cart.getItemCount()`

Vrátí celkový počet položek (kusů) v košíku.

```javascript
const count = GitCart.cart.getItemCount();
console.log(`Košík obsahuje ${count} položek`);

// Pro různé účely
document.querySelector('.cart-badge').textContent = count;
```

### `GitCart.cart.getUniqueItemCount()`

Vrátí počet unikátních produktů (bez ohledu na množství).

```javascript
const uniqueCount = GitCart.cart.getUniqueItemCount();
console.log(`${uniqueCount} různých produktů v košíku`);
```

### `GitCart.cart.getSubtotal()`

Vrátí mezisoučet košíku (bez dopravy a DPH).

```javascript
const subtotal = GitCart.cart.getSubtotal();
console.log(`Mezisoučet: ${subtotal} Kč`);

// S formátováním
const formatted = GitCart.utils.formatPrice(subtotal, 'CZK');
console.log(`Mezisoučet: ${formatted}`);
```

### `GitCart.cart.getTotal(shipping, tax)`

Vrátí celkovou cenu včetně dopravy a DPH.

```javascript
// Bez dopravy a DPH
const total = GitCart.cart.getTotal();

// S dopravou
const totalWithShipping = GitCart.cart.getTotal(149);

// S dopravou a DPH
const totalWithTax = GitCart.cart.getTotal(149, 21);

console.log(`Celkem k platbě: ${totalWithTax} Kč`);
```

## 💳 Checkout API

Správa checkout procesu od košíku po dokončení objednávky.

### `GitCart.checkout.start(options)`

Zahájí checkout proces.

```javascript
// Základní checkout
GitCart.checkout.start();

// S možnostmi
GitCart.checkout.start({
  step: 'shipping',        // Začít na konkrétním kroku
  requireLogin: false,     // Vyžadovat přihlášení
  guestCheckout: true,     // Povolit checkout bez registrace
  redirectUrl: '/checkout/' // URL checkout stránky
});
```

### `GitCart.checkout.setCustomer(customerData)`

Nastaví zákaznická data.

```javascript
const customerData = {
  firstName: 'Jan',
  lastName: 'Novák',
  email: 'jan@example.com',
  phone: '+420 123 456 789',
  company: 'Moje firma s.r.o.', // Volitelné
  ic: '12345678',              // Volitelné
  dic: 'CZ12345678',           // Volitelné
  note: 'Zavolat před doručením'
};

GitCart.checkout.setCustomer(customerData);
```

### `GitCart.checkout.setShipping(method, address)`

Nastaví způsob dopravy a doručovací adresu.

```javascript
const shippingMethod = {
  id: 'ppl',
  name: 'PPL kurýr',
  price: 149,
  deliveryTime: '1-2 dny'
};

const address = {
  street: 'Václavské náměstí 1',
  city: 'Praha',
  zip: '110 00',
  country: 'CZ',
  note: 'Druhý vchod, 3. patro'
};

GitCart.checkout.setShipping(shippingMethod, address);
```

### `GitCart.checkout.setPayment(method)`

Nastaví způsob platby.

```javascript
const paymentMethod = {
  id: 'card',
  name: 'Platební karta',
  price: 0,
  description: 'Visa, Mastercard, Apple Pay'
};

GitCart.checkout.setPayment(paymentMethod);
```

### `GitCart.checkout.getOrderData()`

Vrátí kompletní data objednávky.

```javascript
const orderData = GitCart.checkout.getOrderData();
console.log(orderData);

// Výstup:
{
  id: 'order_1642234567890',
  timestamp: '2024-01-15T10:30:00.000Z',
  locale: 'cs-cz',
  currency: 'CZK',
  
  items: [/* cart items */],
  
  customer: {
    firstName: 'Jan',
    lastName: 'Novák',
    email: 'jan@example.com',
    phone: '+420 123 456 789'
  },
  
  shipping: {
    method: 'ppl',
    methodName: 'PPL kurýr',
    price: 149,
    address: {
      street: 'Václavské náměstí 1',
      city: 'Praha',
      zip: '110 00',
      country: 'CZ'
    }
  },
  
  payment: {
    method: 'card',
    methodName: 'Platební karta'
  },
  
  totals: {
    subtotal: 25999,
    shipping: 149,
    tax: 0,
    discount: 0,
    total: 26148
  }
}
```

### `GitCart.checkout.complete()`

Dokončí objednávku.

```javascript
// Dokončení objednávky
GitCart.checkout.complete()
  .then(orderData => {
    console.log('Objednávka dokončena:', orderData.id);
    // Redirect na thank you page
    window.location.href = '/order-success/?order=' + orderData.id;
  })
  .catch(error => {
    console.error('Chyba při dokončování:', error);
    alert('Objednávku se nepodařilo dokončit. Zkuste to znovu.');
  });
```

### `GitCart.checkout.getCurrentStep()`

Vrátí aktuální krok checkout procesu.

```javascript
const step = GitCart.checkout.getCurrentStep();
// Možné hodnoty: 'cart', 'customer', 'shipping', 'payment', 'review', 'complete'

switch (step) {
  case 'customer':
    showCustomerForm();
    break;
  case 'shipping':
    showShippingOptions();
    break;
  case 'payment':
    showPaymentOptions();
    break;
}
```

### `GitCart.checkout.validateStep(step)`

Validuje data pro konkrétní krok.

```javascript
// Validace zákaznických dat
const isValid = GitCart.checkout.validateStep('customer');
if (!isValid) {
  const errors = GitCart.checkout.getValidationErrors('customer');
  console.log('Chyby validace:', errors);
}

// Validace všech kroků
const isOrderReady = GitCart.checkout.validateStep('complete');
```

## 🔍 Search API

Client-side vyhledávání produktů s fuzzy search a filtrováním.

### `GitCart.search.query(searchTerm, filters)`

Vyhledá produkty podle zadaných kritérií.

```javascript
// Základní vyhledávání
const results = await GitCart.search.query('iphone');

// S filtry
const results = await GitCart.search.query('telefon', {
  category: 'elektronika',
  price: { min: 10000, max: 30000 },
  tags: ['smartphone'],
  inStock: true
});

console.log(`Nalezeno ${results.length} produktů`);
```

#### Dostupné filtry

```javascript
{
  category: 'string',           // Kategorie produktu
  categories: ['array'],        // Více kategorií
  price: {                     // Cenové rozpětí
    min: number,
    max: number
  },
  tags: ['array'],             // Tagy produktu
  brand: 'string',             // Značka
  featured: boolean,           // Doporučené produkty
  inStock: boolean,            // Skladem
  onSale: boolean,             // Ve slevě
  rating: {                    // Hodnocení
    min: number,
    max: number
  },
  availability: 'string'       // Dostupnost: 'available', 'preorder', 'discontinued'
}
```

### `GitCart.search.filter(filters)`

Filtruje produkty bez textového vyhledávání.

```javascript
// Pouze produkty ve slevě
const saleProducts = await GitCart.search.filter({
  onSale: true,
  inStock: true
});

// Produkty z konkrétní kategorie
const electronics = await GitCart.search.filter({
  category: 'elektronika',
  price: { min: 5000 }
});
```

### `GitCart.search.sort(field, direction)`

Setřídí výsledky vyhledávání.

```javascript
// Řazení podle ceny (vzestupně)
GitCart.search.sort('price', 'asc');

// Řazení podle názvu (sestupně)
GitCart.search.sort('title', 'desc');

// Řazení podle hodnocení
GitCart.search.sort('rating', 'desc');

// Řazení podle relevance (výchozí)
GitCart.search.sort('relevance', 'desc');
```

#### Dostupná pole pro řazení

- `title` - Název produktu
- `price` - Cena
- `rating` - Hodnocení
- `date` - Datum přidání
- `popularity` - Popularita
- `relevance` - Relevance (pro textové vyhledávání)

### `GitCart.search.getSuggestions(term)`

Vrátí návrhy pro automatické doplňování.

```javascript
const suggestions = await GitCart.search.getSuggestions('ipho');
console.log(suggestions);

// Výstup:
[
  'iphone',
  'iphone 15',
  'iphone 14',
  'iphone case',
  'iphone charger'
]
```

### `GitCart.search.getFilters()`

Vrátí dostupné filtry na základě aktuálních výsledků.

```javascript
const availableFilters = await GitCart.search.getFilters();
console.log(availableFilters);

// Výstup:
{
  categories: [
    { id: 'elektronika', name: 'Elektronika', count: 45 },
    { id: 'obleceni', name: 'Oblečení', count: 23 }
  ],
  brands: [
    { id: 'apple', name: 'Apple', count: 12 },
    { id: 'samsung', name: 'Samsung', count: 8 }
  ],
  priceRange: { min: 99, max: 50000 },
  tags: [
    { id: 'smartphone', name: 'Smartphone', count: 25 },
    { id: '5g', name: '5G', count: 18 }
  ]
}
```

### Konfigurace Search

```javascript
// Konfigurace search enginu
GitCart.search.configure({
  // Fuse.js možnosti
  fuzzy: {
    threshold: 0.3,        // Práh fuzzy vyhledávání (0-1)
    distance: 100,         // Maximální vzdálenost pro match
    keys: [                // Pole pro vyhledávání
      { name: 'title', weight: 0.7 },
      { name: 'description', weight: 0.2 },
      { name: 'tags', weight: 0.1 }
    ]
  },
  
  // Cache nastavení
  cache: {
    enabled: true,
    duration: 300000       // 5 minut v ms
  },
  
  // Počet výsledků
  maxResults: 100,
  
  // Highlighting
  highlight: true
});
```

## 🔔 Event systém

GitCart používá event-driven architekturu pro komunikaci mezi komponentami.

### `GitCart.on(eventName, callback)`

Přidá posluchač události.

```javascript
// Poslouchání přidání do košíku
GitCart.on('gitcart:cart:item:added', function(data) {
  console.log('Přidáno do košíku:', data.title);
  
  // Update cart widget
  updateCartWidget();
  
  // Google Analytics tracking
  gtag('event', 'add_to_cart', {
    currency: data.currency,
    value: data.price,
    items: [{
      item_id: data.sku,
      item_name: data.title,
      category: data.category,
      quantity: data.quantity,
      price: data.price
    }]
  });
});

// Poslouchání více událostí najednou
GitCart.on('gitcart:cart:item:added gitcart:cart:item:removed', function(data) {
  updateCartTotal();
});

// Wildcard poslouchání
GitCart.on('gitcart:cart:*', function(data, eventName) {
  console.log('Cart event:', eventName, data);
});
```

### `GitCart.emit(eventName, data)`

Vyšle vlastní událost.

```javascript
// Vlastní událost
GitCart.emit('myshop:product:favorited', {
  sku: 'IPHONE15-128',
  title: 'iPhone 15',
  userId: 'user123'
});

// Plugin může poslouchat
GitCart.on('myshop:product:favorited', function(data) {
  // Uložit do wishlistu
  saveToWishlist(data);
});
```

### `GitCart.off(eventName, callback)`

Odstraní posluchač události.

```javascript
function myCallback(data) {
  console.log('Callback called');
}

// Přidání
GitCart.on('gitcart:cart:updated', myCallback);

// Odstranění
GitCart.off('gitcart:cart:updated', myCallback);

// Odstranění všech posluchačů pro událost
GitCart.off('gitcart:cart:updated');
```

### `GitCart.once(eventName, callback)`

Přidá posluchač, který se spustí pouze jednou.

```javascript
// Spustí se pouze při prvním přidání do košíku
GitCart.once('gitcart:cart:item:added', function(data) {
  console.log('První produkt přidán do košíku!');
  showWelcomeTooltip();
});
```

### Základní události GitCart

#### Cart události

```javascript
// Přidání produktu do košíku
'gitcart:cart:item:added': {
  sku: 'string',
  title: 'string',
  price: number,
  quantity: number,
  variant: 'string',
  subtotal: number
}

// Odebrání z košíku
'gitcart:cart:item:removed': {
  sku: 'string',
  title: 'string',
  quantity: number
}

// Změna množství
'gitcart:cart:quantity:changed': {
  sku: 'string',
  oldQuantity: number,
  newQuantity: number,
  variant: 'string'
}

// Aktualizace košíku (po jakékoliv změně)
'gitcart:cart:updated': {
  items: Array,
  itemCount: number,
  subtotal: number,
  total: number
}

// Vymazání košíku
'gitcart:cart:cleared': {}
```

#### Product události

```javascript
// Zobrazení produktu
'gitcart:product:viewed': {
  sku: 'string',
  title: 'string',
  price: number,
  category: 'string',
  url: 'string'
}

// Quick view produktu
'gitcart:product:quickview': {
  sku: 'string',
  title: 'string'
}
```

#### Search události

```javascript
// Vyhledávání
'gitcart:search:query': {
  term: 'string',
  filters: Object,
  resultsCount: number
}

// Aplikování filtrů
'gitcart:products:filtered': {
  filters: Object,
  resultsCount: number
}

// Řazení produktů
'gitcart:products:sorted': {
  field: 'string',
  direction: 'string'
}
```

#### Checkout události

```javascript
// Zahájení checkout
'gitcart:checkout:started': {
  items: Array,
  total: number
}

// Krok checkout
'gitcart:checkout:step': {
  step: 'string',
  data: Object
}

// Výběr dopravy
'gitcart:shipping:selected': {
  method: 'string',
  methodName: 'string',
  price: number
}

// Výběr platby  
'gitcart:payment:selected': {
  method: 'string',
  methodName: 'string'
}

// Dokončení objednávky
'gitcart:checkout:completed': {
  id: 'string',
  total: number,
  items: Array,
  customer: Object
}
```

## 🛠️ Utility funkce

Pomocné funkce pro běžné úkoly.

### `GitCart.utils.formatPrice(amount, currency, options)`

Formátuje cenu podle lokálních zvyklostí.

```javascript
// Základní formátování
const formatted = GitCart.utils.formatPrice(25999, 'CZK');
console.log(formatted); // "25 999 Kč"

// S možnostmi
const formatted = GitCart.utils.formatPrice(29.99, 'USD', {
  decimals: 2,
  thousandsSeparator: ',',
  decimalSeparator: '.',
  currencyPosition: 'before' // 'before' | 'after'
});
console.log(formatted); // "$29.99"

// Různé měny
console.log(GitCart.utils.formatPrice(999, 'EUR'));  // "999 €"
console.log(GitCart.utils.formatPrice(1299, 'GBP')); // "£1,299"
```

### `GitCart.utils.formatDate(date, format, locale)`

Formátuje datum podle locale.

```javascript
const date = new Date('2024-01-15');

// Výchozí formátování
console.log(GitCart.utils.formatDate(date)); // "15. 1. 2024" (cs-cz)

// Vlastní formát
console.log(GitCart.utils.formatDate(date, 'dd.mm.yyyy')); // "15.01.2024"
console.log(GitCart.utils.formatDate(date, 'mm/dd/yyyy', 'en-us')); // "01/15/2024"

// Relative formátování
console.log(GitCart.utils.formatDate(date, 'relative')); // "před 3 dny"
```

### `GitCart.utils.slugify(text)`

Převede text na URL-friendly slug.

```javascript
const slug = GitCart.utils.slugify('iPhone 15 Pro Max - Nejlepší telefon!');
console.log(slug); // "iphone-15-pro-max-nejlepsi-telefon"

// S možnostmi
const slug = GitCart.utils.slugify('Příliš žluťoučký kůň', {
  maxLength: 20,
  separator: '_'
});
console.log(slug); // "prilis_zlutoucky_kun"
```

### `GitCart.utils.debounce(func, wait)`

Debounce funkce pro optimalizaci výkonu.

```javascript
// Debounced search
const debouncedSearch = GitCart.utils.debounce(function(term) {
  GitCart.search.query(term);
}, 300);

// Použití na input
document.getElementById('search').addEventListener('input', function(e) {
  debouncedSearch(e.target.value);
});
```

### `GitCart.utils.throttle(func, limit)`

Throttle funkce pro omezení frekvence volání.

```javascript
// Throttled scroll handler
const throttledScroll = GitCart.utils.throttle(function() {
  console.log('Scrolling...');
}, 100);

window.addEventListener('scroll', throttledScroll);
```

### `GitCart.utils.storage`

Wrapper pro localStorage s error handling.

```javascript
// Uložení dat
GitCart.utils.storage.set('user_preferences', {
  theme: 'dark',
  language: 'cs-cz'
});

// Načtení dat
const prefs = GitCart.utils.storage.get('user_preferences');

// S fallback hodnotou
const theme = GitCart.utils.storage.get('theme', 'light');

// Odstranění
GitCart.utils.storage.remove('temp_data');

// Vyčištění všeho
GitCart.utils.storage.clear();

// Kontrola podpory
if (GitCart.utils.storage.isSupported()) {
  console.log('localStorage je podporován');
}
```

### `GitCart.utils.ajax(url, options)`

Simplifikované AJAX volání.

```javascript
// GET request
const data = await GitCart.utils.ajax('/api/products');

// POST request
const result = await GitCart.utils.ajax('/api/contact', {
  method: 'POST',
  data: {
    name: 'Jan Novák',
    email: 'jan@example.com'
  },
  headers: {
    'Content-Type': 'application/json'
  }
});

// S error handling
try {
  const data = await GitCart.utils.ajax('/api/data');
  console.log('Success:', data);
} catch (error) {
  console.error('Error:', error);
}
```

## 🔌 Plugin API

Systém pro rozšiřování GitCart funkcionality.

### `GitCart.plugin(name, definition)`

Registruje nový plugin.

```javascript
GitCart.plugin('my-plugin', {
  // Konfigurace pluginu
  config: {
    name: 'My Plugin',
    version: '1.0.0',
    enabled: true
  },

  // Inicializace pluginu
  init: function() {
    console.log('My Plugin initialized');
    this.setupEventListeners();
  },

  // Event listenery
  events: {
    'gitcart:cart:item:added': function(data) {
      this.trackAddToCart(data);
    },
    
    'gitcart:checkout:completed': function(data) {
      this.trackPurchase(data);
    }
  },

  // Render funkce pro HTML injection
  render: {
    'product-detail-after-images': function(context) {
      return '<div class="my-plugin-content">Custom content</div>';
    }
  },

  // Plugin metody
  methods: {
    trackAddToCart: function(data) {
      console.log('Tracking add to cart:', data);
    },
    
    trackPurchase: function(data) {
      console.log('Tracking purchase:', data);
    }
  }
});
```

### Plugin lifecycle

```javascript
GitCart.plugin('lifecycle-example', {
  
  // 1. Inicializace - volá se při registraci pluginu
  init: function() {
    console.log('Plugin initializing...');
    this.setupUI();
    this.loadSettings();
  },

  // 2. Setup - volá se po načtení DOM
  setup: function() {
    console.log('Plugin setup complete');
    this.bindEvents();
  },

  // 3. Destroy - volá se při odstranění pluginu
  destroy: function() {
    console.log('Plugin cleanup');
    this.removeEventListeners();
    this.clearData();
  }
});
```

### Plugin komunikace

```javascript
// Plugin A
GitCart.plugin('plugin-a', {
  init: function() {
    // Odeslání zprávy jinému pluginu
    GitCart.emit('plugin-a:ready', {
      version: '1.0.0'
    });
  }
});

// Plugin B
GitCart.plugin('plugin-b', {
  events: {
    'plugin-a:ready': function(data) {
      console.log('Plugin A is ready:', data.version);
    }
  }
});
```

## 📱 Responsive & Mobile API

Utility pro práci s responzivním designem a mobilními zařízeními.

### `GitCart.device`

Informace o zařízení a viewport.

```javascript
// Detekce zařízení
console.log(GitCart.device.isMobile());    // true/false
console.log(GitCart.device.isTablet());    // true/false
console.log(GitCart.device.isDesktop());   // true/false

// Detekce OS
console.log(GitCart.device.isIOS());       // true/false
console.log(GitCart.device.isAndroid());   // true/false

// Viewport informace
console.log(GitCart.device.getViewport()); 
// { width: 1920, height: 1080, ratio: 1.78 }

// Breakpoint detekce
console.log(GitCart.device.getBreakpoint()); // 'mobile', 'tablet', 'desktop'

// Touch podpora
console.log(GitCart.device.hasTouch());    // true/false
```

### `GitCart.responsive`

Responsive utility funkce.

```javascript
// Reagování na změny breakpointu
GitCart.responsive.onBreakpointChange(function(breakpoint) {
  console.log('New breakpoint:', breakpoint);
  
  if (breakpoint === 'mobile') {
    // Mobilní specifická funkcionalita
    enableSwipeNavigation();
  } else {
    // Desktop funkcionalita  
    enableHoverEffects();
  }
});

// Media query matching
if (GitCart.responsive.matches('(max-width: 768px)')) {
  // Mobilní layout
}

// Responsive images
GitCart.responsive.updateImages(); // Aktualizuje srcset obrázků
```

## 🎨 UI komponenty

Built-in UI komponenty pro běžné e-commerce prvky.

### Cart widget

```javascript
// Inicializace cart widgetu
GitCart.ui.cartWidget.init({
  selector: '.cart-widget',
  template: 'mini', // 'mini', 'full', 'sidebar'
  showTotal: true,
  showItemCount: true,
  autoUpdate: true
});

// Refresh widgetu
GitCart.ui.cartWidget.refresh();

// Toggle sidebar
GitCart.ui.cartWidget.toggle();
```

### Product quick view

```javascript
// Otevření quick view
GitCart.ui.quickView.open('IPHONE15-128', {
  showAddToCart: true,
  showImages: true,
  showDescription: false
});

// Zavření
GitCart.ui.quickView.close();

// Event listenery
GitCart.on('gitcart:quickview:opened', function(product) {
  // Analytics tracking
});
```

### Toast notifikace

```javascript
// Zobrazení notifikace
GitCart.ui.toast.show('Produkt přidán do košíku', {
  type: 'success',        // 'success', 'error', 'warning', 'info'
  duration: 3000,         // ms
  position: 'top-right',  // 'top-left', 'top-right', 'bottom-left', 'bottom-right'
  closable: true
});

// Error notifikace
GitCart.ui.toast.error('Chyba při přidávání do košíku');

// Success s akcí
GitCart.ui.toast.success('Produkt přidán', {
  actions: [{
    text: 'Zobrazit košík',
    callback: () => window.location.href = '/kosik/'
  }]
});
```

## 🔧 Konfigurace a customizace

### Globální konfigurace

```javascript
// Nastavení před inicializací
window.GitCartConfig = {
  // Základní nastavení
  currency: 'CZK',
  locale: 'cs-cz',
  debug: false,
  
  // API endpointy
  endpoints: {
    products: '/api/products.json',
    search: '/api/search.json'
  },
  
  // Cart nastavení
  cart: {
    persistent: true,
    storageKey: 'gitcart_cart',
    maxItems: 100,
    allowDuplicates: false
  },
  
  // Search konfigurace
  search: {
    minQueryLength: 2,
    maxResults: 50,
    highlightResults: true,
    fuzzy: {
      threshold: 0.3,
      distance: 100
    }
  },
  
  // UI nastavení
  ui: {
    animations: true,
    toastDuration: 3000,
    loadingIndicator: true
  }
};
```

### Runtime konfigurace

```javascript
// Změna konfigurace za běhu
GitCart.configure({
  currency: 'EUR',
  debug: true
});

// Získání aktuální konfigurace
const config = GitCart.getConfig();
console.log(config.currency); // 'EUR'

// Změna konkrétního nastavení
GitCart.setConfig('ui.animations', false);
```

## 📊 Analytics integrace

Vestavěná podpora pro analytics služby.

### Google Analytics 4

```javascript
// Automatické tracking základních událostí
GitCart.analytics.ga4.configure({
  measurementId: 'G-XXXXXXXXXX',
  enableEcommerce: true,
  enhancedEcommerce: true,
  trackPageViews: true
});

// Manuální tracking
GitCart.analytics.ga4.track('add_to_cart', {
  currency: 'CZK',
  value: 25999,
  items: [{
    item_id: 'IPHONE15-128',
    item_name: 'iPhone 15',
    category: 'Elektronika',
    price: 25999,
    quantity: 1
  }]
});
```

### Facebook Pixel

```javascript
GitCart.analytics.facebook.configure({
  pixelId: 'your-pixel-id',
  trackPageViews: true
});

// Custom events
GitCart.analytics.facebook.track('AddToCart', {
  content_ids: ['IPHONE15-128'],
  content_type: 'product',
  value: 25999,
  currency: 'CZK'
});
```

## 🚨 Error handling

Robustní error handling a debugging.

### Error handling

```javascript
// Globální error handler
GitCart.onError(function(error, context) {
  console.error('GitCart error:', error);
  
  // Odeslání do error tracking služby
  if (window.Sentry) {
    Sentry.captureException(error, { extra: context });
  }
});

// Specifické error handling
try {
  await GitCart.cart.add('INVALID-SKU');
} catch (error) {
  if (error.type === 'PRODUCT_NOT_FOUND') {
    GitCart.ui.toast.error('Produkt nebyl nalezen');
  }
}
```

### Debug režim

```javascript
// Zapnutí debug režimu
GitCart.debug(true);

// Debug informace
console.log(GitCart.getDebugInfo());
// {
//   version: '1.0.0',
//   config: {...},
//   cart: {...},
//   plugins: [...],
//   performance: {...}
// }

// Performance monitoring
const metrics = GitCart.getPerformanceMetrics();
console.log('API call duration:', metrics.avgApiTime);
```

## 🎯 Příklady použití

### Kompletní e-shop implementace

```html
<!DOCTYPE html>
<html>
<head>
  <script src="/assets/js/gitcart.js"></script>
</head>
<body>
  <!-- Product card -->
  <div class="product-card" data-sku="IPHONE15-128">
    <h3>iPhone 15 128GB</h3>
    <div class="price">23.999 Kč</div>
    <button class="add-to-cart">Přidat do košíku</button>
  </div>

  <!-- Cart widget -->
  <div class="cart-widget">
    <span class="cart-count">0</span>
    <span class="cart-total">0 Kč</span>
  </div>

  <script>
    document.addEventListener('DOMContentLoaded', function() {
      
      // Add to cart buttons
      document.querySelectorAll('.add-to-cart').forEach(button => {
        button.addEventListener('click', function() {
          const card = this.closest('.product-card');
          const sku = card.dataset.sku;
          const title = card.querySelector('h3').textContent;
          const price = 23999;
          
          GitCart.cart.add(sku, 1, {
            title: title,
            price: price
          });
        });
      });
      
      // Update cart widget on changes
      GitCart.on('gitcart:cart:updated', function(data) {
        document.querySelector('.cart-count').textContent = data.itemCount;
        document.querySelector('.cart-total').textContent = 
          GitCart.utils.formatPrice(data.total, 'CZK');
      });
      
      // Analytics tracking
      GitCart.on('gitcart:cart:item:added', function(data) {
        gtag('event', 'add_to_cart', {
          currency: 'CZK',
          value: data.price * data.quantity,
          items: [{
            item_id: data.sku,
            item_name: data.title,
            category: data.category,
            price: data.price,
            quantity: data.quantity
          }]
        });
      });
      
    });
  </script>
</body>
</html>
```

### Search & Filter komponenta

```html
<div class="search-container">
  <input type="text" id="search-input" placeholder="Vyhledat produkty...">
  
  <div class="filters">
    <select id="category-filter">
      <option value="">Všechny kategorie</option>
      <option value="elektronika">Elektronika</option>
      <option value="obleceni">Oblečení</option>
    </select>
    
    <div class="price-filter">
      <input type="range" id="price-min" min="0" max="50000">
      <input type="range" id="price-max" min="0" max="50000" value="50000">
    </div>
  </div>
  
  <div id="search-results"></div>
</div>

<script>
const searchInput = document.getElementById('search-input');
const categoryFilter = document.getElementById('category-filter');
const priceMin = document.getElementById('price-min');
const priceMax = document.getElementById('price-max');
const resultsContainer = document.getElementById('search-results');

// Debounced search
const debouncedSearch = GitCart.utils.debounce(performSearch, 300);

searchInput.addEventListener('input', debouncedSearch);
categoryFilter.addEventListener('change', performSearch);
priceMin.addEventListener('input', performSearch);
priceMax.addEventListener('input', performSearch);

async function performSearch() {
  const term = searchInput.value;
  const filters = {
    category: categoryFilter.value || undefined,
    price: {
      min: parseInt(priceMin.value),
      max: parseInt(priceMax.value)
    }
  };
  
  try {
    const results = await GitCart.search.query(term, filters);
    renderResults(results);
  } catch (error) {
    console.error('Search error:', error);
    GitCart.ui.toast.error('Chyba při vyhledávání');
  }
}

function renderResults(results) {
  resultsContainer.innerHTML = results.map(product => `
    <div class="product-result">
      <h4>${product.title}</h4>
      <div class="price">${GitCart.utils.formatPrice(product.price, 'CZK')}</div>
      <button onclick="addToCart('${product.sku}')">Přidat do košíku</button>
    </div>
  `).join('');
}

function addToCart(sku) {
  GitCart.cart.add(sku);
  GitCart.ui.toast.success('Produkt přidán do košíku');
}
</script>
```

GitCart.js poskytuje kompletní API pro vytváření moderních e-commerce zážitků s minimální komplexitou a maximální flexibilitou.