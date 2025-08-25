# Integrace Tailwind CSS v4 do GitCart

Tento dokument popisuje integraci Tailwind CSS v4 implementovanou v GitCart pro moderní, utility-first stylování.

## Přehled

GitCart nyní obsahuje vestavěnou podporu pro Tailwind CSS v4 s následujícími funkcemi:

- **Automatická detekce** Tailwind CSS souborů
- **Inteligentní cachování** pro rychlejší rebuildy
- **Produkční optimalizace** s Lightning CSS
- **Podpora watch režimu** pro vývoj
- **E-commerce specifické utility** a komponenty
- **Podpora tmavého režimu** s proper tématy
- **Podpora RTL jazyků** pro arabštinu/hebrejštinu
- **Kompatibilita s plugin systémem**

## Struktura souborů

```
projekt/
├── tailwind.config.js          # Tailwind konfigurace
├── templates/
│   └── default/
│       └── assets/
│           └── css/
│               └── style.css    # Hlavní Tailwind CSS soubor
└── dist/
    └── assets/
        └── css/
            └── style.css        # Zkompilovaný výstup
```

## Konfigurace

### tailwind.config.js

Konfigurace je optimalizována pro e-commerce použití GitCart:

```javascript
export default {
  content: [
    './templates/**/*.{liquid,njk,hbs,mustache,html,js}',
    './src/**/*.js',
    './plugins/**/*.js',
    './dist/**/*.html'
  ],
  darkMode: 'class',
  theme: {
    extend: {
      // E-commerce specifická rozšíření
      aspectRatio: {
        'product': '4 / 3',
        'category': '16 / 9'
      },
      colors: {
        primary: { /* ... */ },
        success: { /* ... */ },
        error: { /* ... */ }
      }
    }
  }
}
```

### Závislosti

Vyžadovány jsou následující balíčky:

```json
{
  "dependencies": {
    "@tailwindcss/cli": "^4.1.12",
    "lightningcss": "^1.30.1"
  }
}
```

## Struktura Template CSS

### Hlavní CSS soubor (style.css)

```css
/* Import Tailwind CSS v4 */
@import "tailwindcss";

/* Custom utility třídy pro GitCart e-commerce */
@layer utilities {
  .aspect-product {
    aspect-ratio: 4 / 3;
  }
  
  .line-clamp-2 {
    overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
  }
}

/* E-commerce komponenty */
@layer components {
  .products-grid {
    @apply grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6;
  }
  
  .btn-primary {
    @apply bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded-lg transition-colors;
  }
  
  .product-card {
    @apply bg-white dark:bg-gray-800 rounded-lg shadow-md hover:shadow-xl transition-all;
  }
}
```

## Build proces

### Automatická detekce

AssetProcessor automaticky detekuje Tailwind CSS soubory kontrolou:
- `@import "tailwindcss"` 
- `@tailwind` direktiv

### Build pipeline

1. **Vykreslení templates** - Generuje HTML z Liquid templates
2. **Skenování obsahu** - Skenuje templates pro Tailwind třídy
3. **CSS kompilace** - Sestavuje CSS s Tailwind CLI
4. **Optimalizace** - Optimalizuje s Lightning CSS (pouze produkce)
5. **Cachování** - Cachuje výsledky pro rychlejší rebuildy

### Použití CLI

```bash
# Build s Tailwind CSS
gitcart build

# Vývoj s watch režimem
gitcart dev --watch

# Produkční build s minifikací
gitcart build --minify
```

## Funkce

### Podpora tmavého režimu

Všechny templates obsahují varianty pro tmavý režim:

```html
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  <h1 class="text-blue-600 dark:text-blue-400">Vítejte</h1>
</div>
```

### E-commerce komponenty

Připravené komponenty pro e-commerce:

```html
<!-- Product Grid -->
<div class="products-grid">
  <!-- Product Card -->
  <div class="product-card">
    <div class="aspect-product overflow-hidden">
      <img class="w-full h-full object-cover group-hover:scale-105 transition-transform">
    </div>
    <div class="p-4">
      <h3 class="font-semibold mb-2">Název produktu</h3>
      <div class="price-regular">999 Kč</div>
      <button class="btn-primary w-full mt-3">Přidat do košíku</button>
    </div>
  </div>
</div>
```

### Responzivní design

Mobile-first přístup s responzivními utility:

```html
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
  <div class="p-4 lg:p-6">
    <h2 class="text-lg lg:text-xl font-semibold">Nadpis</h2>
  </div>
</div>
```

### Podpora RTL jazyků

Vestavěná RTL podpora pro arabštinu/hebrejštinu:

```html
<body class="{% if current_language_data.rtl %}rtl{% endif %}">
  <div class="text-left rtl:text-right">
    <p class="ml-4 rtl:ml-0 rtl:mr-4">Obsah</p>
  </div>
</body>
```

## Výkon

### Cachování systém

- **File-based cachování** zabraňuje zbytečným rebuildům
- **Content-aware hashing** detekuje změny přesně
- **Produkční optimalizace** s Lightning CSS

### Optimalizace velikosti bundle

- **Purging** odstraňuje nepoužívané CSS třídy
- **Minifikace** redukuje velikost souborů
- **Moderní CSS** cílí pouze na současné prohlížeče

## Vývojářský workflow

### Watch režim

Tailwind CSS se automaticky rebuilds při změně templates:

```bash
gitcart dev --watch
# Spustí vývojový server s Tailwind watch režimem
```

### Hot Reload

Změny v templates spustí:
1. Znovu vykreslení template
2. Tailwind CSS rebuild  
3. Reload prohlížeče

### Debugging

Použijte verbose režim pro detailní výstup:

```bash
gitcart build --verbose
# Zobrazuje detaily Tailwind kompilace
```

## Integrace s pluginy

### CSS Injection

Pluginy mohou přidávat custom CSS:

```javascript
GitCart.plugin('my-plugin', {
  styles: './custom-styles.css',
  tailwindClasses: ['bg-custom-500', 'text-plugin-primary']
});
```

### Dynamické třídy

Safelist chrání dynamicky generované třídy:

```javascript
// V tailwind.config.js
safelist: [
  'badge-sale',
  'order-pending',
  { pattern: /^(bg|text)-primary-(50|100|500|900)$/ }
]
```

## Řešení problémů

### Běžné problémy

1. **Build selhává**: Zkontrolujte, zda je nainstalován `@tailwindcss/cli`
2. **Třídy se neobjevují**: Ověřte content paths v konfiguraci
3. **Pomalé buildy**: Vymažte cache pomocí `assetProcessor.clearTailwindCache()`

### Testování

Použijte vestavěný test script:

```bash
node __tests__/test-tailwind-build.js
```

Validuje:
- Konfigurační soubory
- Závislosti
- Build proces
- Generování výstupu
- Funkcionalitu cache

## Migrace z vlastního CSS

### Před (Vlastní CSS)
```css
.product-card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
```

### Po (Tailwind CSS)
```html
<div class="bg-white dark:bg-gray-800 rounded-lg shadow-md hover:shadow-xl hover:-translate-y-1 transition-all">
  <!-- Obsah -->
</div>
```

## Nejlepší praktiky

### 1. Používejte sémantické komponenty
```css
@layer components {
  .shop-button {
    @apply bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded-lg transition-colors;
  }
}
```

### 2. Udržujte konzistenci tmavého režimu
```html
<!-- Vždy poskytněte varianty pro tmavý režim -->
<div class="bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100">
```

### 3. Používejte responzivní design
```html
<!-- Mobile-first responzivní design -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3">
```

### 4. Využívajte E-commerce utility
```html
<!-- Používejte předdefinované aspect ratios -->
<div class="aspect-product">
  <img class="w-full h-full object-cover">
</div>
```

## Závěr

Integrace Tailwind CSS v4 poskytuje moderní, udržitelné a výkonné řešení pro stylování GitCart e-commerce stránek. Automatický build proces, inteligentní cachování a e-commerce specifické utility usnadňují vytváření krásných, responzivních online obchodů.