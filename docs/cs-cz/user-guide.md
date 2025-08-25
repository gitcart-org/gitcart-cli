# GitCart - Uživatelská příručka

Kompletní návod pro použití GitCart generátoru statických e-shopů.

## 🚀 Začínáme

### Systémové požadavky

- Node.js 18+ 
- npm 8+
- Git (doporučeno)
- Textový editor (VS Code, Sublime Text, atd.)

### První projekt krok za krokem

#### 1. Instalace GitCart CLI

```bash
# Globální instalace (doporučeno)
npm install -g gitcart-cli

# Ověření instalace
gitcart --version
```

#### 2. Vytvoření nového projektu

```bash
# Interaktivní vytvoření
gitcart init muj-eshop

# Nebo s parametry
gitcart init muj-eshop --template default --language cs-cz
```

#### 3. Základní konfigurace

Editujte soubor `site.json`:

```json
{
  "name": "Můj E-shop",
  "url": "https://muj-eshop.cz",
  "description": "Kvalitní produkty za skvělé ceny",
  "template_engine": "liquid",
  "url_strategy": "default_clean",
  "url_locale_format": "short",
  
  "languages": {
    "cs-cz": {
      "name": "Čeština", 
      "default": true, 
      "short": "cs", 
      "currency": "CZK"
    }
  },
  
  "pagination": {
    "products_per_page": 12,
    "max_pagination_pages": 10
  },
  
  "variables": {
    "contact": {
      "email": "info@muj-eshop.cz",
      "phone": "+420 123 456 789",
      "address": "Václavské náměstí 1, Praha"
    },
    "social": {
      "facebook": "https://facebook.com/muj-eshop",
      "instagram": "@muj_eshop"
    },
    "navigation": {
      "header": [
        {"page": "o-nas"},
        {"page": "kontakt"}
      ],
      "footer": {
        "left": [
          {"page": "kontakt"},
          {"page": "o-nas"}
        ],
        "right": [
          {"page": "doprava-a-platba"},
          {"page": "obchodni-podminky"}
        ]
      }
    }
  }
}
```

## 📁 Správa obsahu

### Vytváření kategorií

#### CLI způsob (doporučeno)
```bash
gitcart category add
```

Interaktivní průvodce se zeptá na:
- Název kategorie
- URL slug 
- Popis kategorie
- Nadřazenou kategorii
- Jazyk
- Zda vytvořit ukázkový produkt

#### Ruční vytvoření

1. Vytvořte složku: `cs-cz/categories/elektronika/`
2. Vytvořte soubor `.category.md`:

```markdown
---
title: Elektronika
description: Nejnovější elektronické zařízení
sort_order: 1
featured: true
banner_image: elektronika-banner.jpg
seo_title: "Elektronika - nejlepší ceny v ČR"
seo_description: "Kupte si kvalitní elektroniku za skvělé ceny. Telefony, tablety, počítače a další."
---

# Elektronika

Vítejte v naší sekci elektroniky! Najdete zde nejnovější:

## 📱 Mobilní telefony
- iPhone, Samsung, Xiaomi
- Nejnovější modely za skvělé ceny
- Záruka a servis

## 💻 Počítače a tablety
- Notebooky pro práci i hraní
- Tablety pro každodenní použití
- Příslušenství a doplňky

## 🎧 Audio technika
- Sluchátka a reproduktory
- Hi-Fi zařízení
- Domácí kina
```

### Vytváření produktů

#### CLI způsob
```bash
gitcart product add
```

#### Ruční vytvoření

1. Vytvořte soubor v kategorii: `cs-cz/categories/elektronika/.products/iphone-15.md`

```markdown
---
title: iPhone 15
price: 25999
sale_price: 23999
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
seo_title: "iPhone 15 128GB Blue - nejlepší cena v ČR"
seo_description: "iPhone 15 s 128GB úložištěm v modré barvě. A17 Bionic čip, USB-C, 48MP fotoaparát. Doprava zdarma!"
---

# iPhone 15 - Nová éra inovací

iPhone 15 představuje významný pokrok ve světě smartphonů. S revolučním USB-C konektorem, výkonným A17 Bionic čipem a pokročilým kamerovým systémem získáte zařízení, které redefinuje mobilní technologie.

## 🌟 Klíčové vlastnosti

### A17 Bionic čip
- Nejpokročilejší čip v iPhone historii
- 6jádrový CPU s 2 performance a 4 efficiency jádry
- 5jádrová GPU pro skvělé hraní
- 16jádrový Neural Engine pro AI funkce

### Pokročilý kamerový systém
- **48MP Main kamera** s f/1.78 aperturou
- **12MP Ultra Wide** s f/2.4 aperturou a 120° zorným polem
- **12MP TrueDepth** přední kamera
- Portrétní režim s rozmazáním pozadí
- 4K Dolby Vision HDR nahrávání

### USB-C konektivita
- Univerzální USB-C port
- Rychlé nabíjení a přenos dat
- Kompatibilita s širokou škálou příslušenství

### Vydrž baterie
- Až 20 hodin přehrávání videa
- Až 16 hodin streamování videa
- Rychlé nabíjení - 50% za 30 minut s 20W adaptérem

## 📱 Technické specifikace

| Vlastnost | Hodnota |
|-----------|---------|
| **Displej** | 6.1" Super Retina XDR OLED |
| **Rozlišení** | 2556 × 1179 px, 460 ppi |
| **Procesor** | A17 Bionic |
| **Úložiště** | 128GB |
| **Paměť** | 6GB RAM |
| **Hlavní kamera** | 48MP f/1.78 + 12MP f/2.4 |
| **Přední kamera** | 12MP f/1.9 |
| **Video** | 4K Dolby Vision HDR |
| **Konektivita** | 5G, Wi-Fi 6E, Bluetooth 5.3 |
| **Odolnost** | IP68 |
| **Rozměry** | 147.6 × 71.6 × 7.80 mm |
| **Hmotnost** | 171 g |

## 🎨 Dostupné barvy

- **Modrá** (aktuální výběr)
- Růžová
- Žlutá
- Zelená
- Černá

## 📦 Obsah balení

- iPhone 15
- USB-C to USB-C kabel
- Dokumentace

*Napájecí adaptér se prodává zvlášť*

## 🛡️ Záruka a servis

- **2 roky záruky** na hardware
- **90 dní podpory** zdarma
- Autorizovaný Apple servis v ČR
- AppleCare+ dostupné pro rozšířené pokrytí
```

2. Přidejte obrázky do složky: `cs-cz/categories/elektronika/.images/`

### Vytváření statických stránek

#### CLI způsob
```bash
gitcart page add
```

#### Ruční vytvoření

Vytvořte soubor `cs-cz/pages/kontakt.md`:

```markdown
---
title: Kontaktujte nás
description: Naše kontaktní údaje a informace
nav_footer: true
nav_order: 1
icon: phone
show_map: true
custom_class: contact-page
seo_title: "Kontakt - Můj E-shop"
seo_description: "Kontaktní údaje, otevírací doba a mapa k našemu obchodu."
---

# Kontaktujte nás

## 📍 Kde nás najdete

**Obchodní adresa:**
Václavské náměstí 1
110 00 Praha 1
Česká republika

## 📞 Kontaktní údaje

- **Telefon:** +420 123 456 789
- **Email:** info@muj-eshop.cz
- **Zákaznická linka:** 800 123 456 (zdarma)

## 🕐 Otevírací doba

| Den | Otevírací doba |
|-----|----------------|
| Pondělí - Pátek | 9:00 - 18:00 |
| Sobota | 9:00 - 15:00 |
| Neděle | Zavřeno |

## 💬 Máte dotaz?

Napište nám a my se vám ozveme do 24 hodin!
```

## 🛠 Development a Build

### Vývojový server
```bash
# Spuštění s live reload
gitcart dev --watch

# Specifický port
gitcart dev --port 3000 --watch
```

### Build pro produkci
```bash
# Základní build
gitcart build

# Build s detailními informacemi
gitcart build --verbose

# Build s optimalizacemi
gitcart build --minify --optimize-images
```

### Struktura výstupu
```
dist/
├── index.html                 # Homepage
├── elektronika/               # Category pages
│   ├── index.html
│   ├── 2/index.html          # Pagination
│   └── iphone-15/
│       └── index.html
├── kontakt/
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

## 🌐 Vícejazyčná podpora

### Konfigurace jazyků

V `site.json`:

```json
{
  "languages": {
    "cs-cz": {
      "name": "Čeština",
      "default": true,
      "short": "cs",
      "currency": "CZK"
    },
    "en-us": {
      "name": "English",
      "short": "en", 
      "currency": "USD"
    },
    "de-de": {
      "name": "Deutsch",
      "short": "de",
      "currency": "EUR"
    }
  },
  "url_strategy": "default_clean",
  "url_locale_format": "short"
}
```

### URL strategie

- `"always_full"`: `/cs-cz/elektronika/iphone-15/`
- `"always_short"`: `/cs/elektronika/iphone-15/`
- `"default_clean"`: `/elektronika/iphone-15/` (výchozí jazyk), `/en/electronics/iphone-15/` (ostatní)
- `"default_clean_full"`: `/elektronika/iphone-15/` (výchozí), `/en-us/electronics/iphone-15/` (ostatní)

### Vytváření vícejazyčného obsahu

1. **Složky podle locales:**
```
cs-cz/                     # Čeština (výchozí)
├── categories/
└── pages/

en-us/                     # Angličtina
├── categories/
│   └── electronics/       # Přeložené názvy kategorií
└── pages/

de-de/                     # Němčina
├── categories/
└── pages/
```

2. **Překlady v navigation:**
```json
{
  "variables": {
    "navigation": {
      "header": {
        "cs-cz": [
          {"page": "o-nas", "title": "O nás"},
          {"page": "kontakt", "title": "Kontakt"}
        ],
        "en-us": [
          {"page": "about-us", "title": "About Us"},
          {"page": "contact", "title": "Contact"}
        ]
      }
    }
  }
}
```

## 🔌 Plugin systém

### Instalace pluginů
```bash
# Z GitCart repozitáře
gitcart plugin add email-notifications
gitcart plugin add google-analytics
gitcart plugin add facebook-pixel

# Lokální plugin
gitcart plugin add ./plugins/custom-shipping.js
```

### Vytvoření vlastního pluginu
```bash
gitcart plugin create my-plugin
```

### Příklad pluginu - Email notifikace

`plugins/email-notifications.js`:

```javascript
(function() {
  'use strict';
  
  const CONFIG = {
    name: 'Email Notifications',
    version: '1.0.0',
    apiKey: 'your-mailgun-api-key',
    domain: 'your-mailgun-domain.com',
    toEmail: 'orders@muj-eshop.cz'
  };

  const EmailPlugin = {
    init() {
      console.log(`${CONFIG.name} plugin loaded`);
    },

    async sendOrderEmail(orderData) {
      const formData = new FormData();
      formData.append('from', `E-shop <noreply@${CONFIG.domain}>`);
      formData.append('to', CONFIG.toEmail);
      formData.append('subject', `Nová objednávka ${orderData.id}`);
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
Nová objednávka: ${orderData.id}
Zákazník: ${orderData.customer.firstName} ${orderData.customer.lastName}
Email: ${orderData.customer.email}
Telefon: ${orderData.customer.phone}

Celková částka: ${orderData.totals.total} ${orderData.currency}

Produkty:
${orderData.items.map(item => 
  `- ${item.title} x${item.quantity} = ${item.subtotal} ${orderData.currency}`
).join('\n')}

Doprava: ${orderData.shipping.methodName} (${orderData.shipping.price} ${orderData.currency})
Platba: ${orderData.payment.methodName}

Doručovací adresa:
${orderData.shipping.address.street}
${orderData.shipping.address.city}, ${orderData.shipping.address.zip}
${orderData.shipping.address.country}
      `;
    }
  };

  // Auto-registrace
  if (typeof GitCart !== 'undefined') {
    GitCart.plugin('email-notifications', {
      config: CONFIG,
      
      events: {
        'gitcart:checkout:completed': EmailPlugin.sendOrderEmail.bind(EmailPlugin)
      },

      render: {
        'order-success-info': function(context) {
          return '<div class="alert alert-success">Potvrzení objednávky bylo odesláno na váš email!</div>';
        }
      },

      init: EmailPlugin.init
    });
  }
})();
```

### Rendering zones

Dostupné zóny v templates:

**Globální zóny:**
- `head` - Meta tagy, analytics
- `body-start` - Tracking kódy
- `header-end` - Za hlavičkou
- `before-footer` - Před patičkou
- `footer-end` - Za patičkou  
- `body-end` - Skripty na konci

**Produktové zóny:**
- `product-images-start/end` - Kolem obrázků produktu
- `product-details-end` - Za detaily produktu
- `product-before-description` - Před popisem
- `product-after-description` - Za popisem
- `product-end` - Na konci produktu

**Kategorie zóny:**
- `category-start/end` - Začátek/konec kategorie
- `category-after-header` - Za hlavičkou kategorie
- `category-before-products` - Před seznamem produktů

## 🔍 Vyhledávání a filtry

### Konfigurace vyhledávání

GitCart používá Fuse.js pro client-side vyhledávání:

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
// Vyhledávání produktů
const results = await GitCart.search.query("iphone", {
  category: "elektronika",
  price: { min: 10000, max: 30000 },
  tags: ["smartphone"]
});

// Aplikování filtrů
GitCart.search.filter({
  category: "elektronika",
  price: { min: 5000, max: 25000 }
});

// Řazení výsledků  
GitCart.search.sort("price", "asc");
```

## 🛒 E-commerce funkce

### GitCart.js API

#### Košík
```javascript
// Přidání do košíku
GitCart.cart.add('IPHONE15-128', 1, {
  title: 'iPhone 15',
  price: 25999,
  image: 'iphone-front.jpg',
  variant: 'blue-128gb'
});

// Odebrání z košíku
GitCart.cart.remove('IPHONE15-128');

// Aktualizace množství
GitCart.cart.updateQuantity('IPHONE15-128', 2);

// Získání položek
const items = GitCart.cart.getItems();
const count = GitCart.cart.getItemCount();
const total = GitCart.cart.getTotal();

// Vyprázdnění košíku
GitCart.cart.clear();
```

#### Checkout proces
```javascript
// Zahájení objednávky
GitCart.checkout.start();

// Zadání zákaznických údajů
GitCart.checkout.setCustomer({
  firstName: 'Jan',
  lastName: 'Novák',
  email: 'jan@example.com',
  phone: '+420123456789'
});

// Volba dopravy
GitCart.checkout.setShipping('ppl', {
  street: 'Václavské náměstí 1',
  city: 'Praha',
  zip: '11000',
  country: 'CZ'
});

// Volba platby
GitCart.checkout.setPayment('card');

// Dokončení objednávky
const order = await GitCart.checkout.complete();
```

#### Event handling
```javascript
// Poslouchání událostí
GitCart.on('gitcart:cart:item:added', function(data) {
  console.log('Přidáno do košíku:', data);
  showNotification('Produkt přidán do košíku!');
});

GitCart.on('gitcart:checkout:completed', function(orderData) {
  console.log('Objednávka dokončena:', orderData);
  // Redirect na děkovnou stránku
  window.location.href = '/dekujeme/?order=' + orderData.id;
});
```

## 📊 SEO optimalizace

### Automatické SEO
GitCart automaticky generuje:
- Meta tagy na základě frontmatter
- Strukturovaná data (JSON-LD)
- Sitemap.xml
- Robots.txt
- Canonical URLs
- OpenGraph tagy

### Ruční optimalizace

V produktech a stránkách:
```markdown
---
title: iPhone 15 128GB Blue
seo_title: "iPhone 15 128GB Blue - nejlepší cena v ČR"
seo_description: "iPhone 15 s 128GB úložištěm v modré barvě. A17 Bionic čip, USB-C, 48MP fotoaparát. Doprava zdarma při objednávce nad 2000 Kč!"
seo_keywords: "iphone 15, apple, smartphone, 128gb, modrá"
og_image: "iphone-15-blue-og.jpg"
og_type: "product"
---
```

### Schema.org markup

Pro produkty se automaticky generuje:
```json
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "iPhone 15",
  "image": "/assets/images/iphone-15-blue-front.jpg",
  "description": "iPhone 15 s revolučním USB-C konektorem...",
  "sku": "IPHONE15-128-BLUE",
  "brand": {
    "@type": "Brand",
    "name": "Apple"
  },
  "offers": {
    "@type": "Offer",
    "price": "23999",
    "priceCurrency": "CZK",
    "availability": "https://schema.org/InStock"
  }
}
```

## 🚀 Deployment

### GitHub/GitLab Pages

1. **Push do repozitáře:**
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

1. **Připojte repozitář k Netlify**
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

## 🛠 Řešení problémů

### Časté chyby

#### "Template not found"
```bash
# Zkontrolujte template_engine v site.json
# Ověřte existenci template souborů
ls templates/default/layouts/
```

#### "SKU must be unique"
```bash
# Najděte duplicitní SKU
gitcart validate --check-sku
```

#### "Missing required field"
```bash
# Validace obsahu
gitcart validate --verbose
```

### Debug režim
```bash
# Detailní výpis při buildu
gitcart build --verbose --debug

# Kontrola před buildem
gitcart validate --fix-auto
```

### Performance optimalizace

#### Obrázky
```bash
# Optimalizace obrázků při buildu
gitcart build --optimize-images

# Generování WebP formátů
gitcart build --webp
```

#### CSS/JS
```bash
# Minifikace assets
gitcart build --minify

# Odstranění nepoužitých CSS tříd
gitcart build --purge-css
```

## 📞 Podpora

### Dokumentace
- [Oficiální dokumentace](https://gitcart.dev/docs)
- [Template reference](https://gitcart.dev/docs/templates)
- [Plugin API](https://gitcart.dev/docs/plugins)

### Community
- [GitHub Issues](https://github.com/gitcart/gitcart/issues)
- [Discord server](https://discord.gg/gitcart)
- [Fórum](https://forum.gitcart.dev)

### Podpora
- Email: support@gitcart.dev
- Dokumentace: docs.gitcart.dev
- Příklady: examples.gitcart.dev