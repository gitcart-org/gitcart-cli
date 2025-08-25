# GitCart - Konfigurace

Kompletní referenční příručka pro konfiguraci GitCart projektů.

## 📝 site.json - Hlavní konfigurace

Soubor `site.json` je srdcem každého GitCart projektu. Definuje všechna základní nastavení, jazyky, URL strategie a vlastní proměnné.

### 🏗️ Základní struktura

```json
{
  "name": "string (required)",
  "url": "string (required)", 
  "description": "string (optional)",
  "template_engine": "liquid",
  "url_strategy": "default_clean|always_full|always_short|default_clean_full",
  "url_locale_format": "short|full",
  "languages": { /* language objects */ },
  "pagination": { /* pagination settings */ },
  "variables": { /* custom variables */ },
  "seo": { /* SEO settings */ },
  "build": { /* build options */ }
}
```

## 🌍 Konfigurace jazyků

### languages

Definuje všechny jazyky podporované vaším e-shopem.

```json
{
  "languages": {
    "cs-cz": {
      "name": "Čeština",
      "default": true,
      "short": "cs",
      "currency": "CZK",
      "date_format": "d.m.Y",
      "number_format": {
        "decimal_separator": ",",
        "thousands_separator": " "
      }
    },
    "en-us": {
      "name": "English",
      "short": "en", 
      "currency": "USD",
      "date_format": "m/d/Y",
      "number_format": {
        "decimal_separator": ".",
        "thousands_separator": ","
      }
    },
    "de-de": {
      "name": "Deutsch",
      "short": "de",
      "currency": "EUR", 
      "date_format": "d.m.Y",
      "number_format": {
        "decimal_separator": ",",
        "thousands_separator": "."
      }
    }
  }
}
```

#### Parametry jazyka

| Parametr | Typ | Povinný | Popis |
|----------|-----|---------|-------|
| `name` | string | ✅ | Lidsky čitelný název jazyka |
| `default` | boolean | - | Výchozí jazyk (pouze jeden) |
| `short` | string | ✅ | Krátký kód pro URL (cs, en, de) |
| `currency` | string | ✅ | Měna (CZK, EUR, USD) |
| `date_format` | string | - | Formát data (výchozí: d.m.Y) |
| `number_format` | object | - | Formátování čísel |

## 🔗 URL strategie

### url_strategy

Určuje, jak budou generovány URL adresy pro různé jazyky.

#### "default_clean" (doporučeno)
```
Výchozí jazyk: /produkty/iphone-15/
Ostatní jazyky: /en/products/iphone-15/
```

#### "always_full" 
```
Všechny jazyky: /cs-cz/produkty/iphone-15/
                /en-us/products/iphone-15/
```

#### "always_short"
```
Všechny jazyky: /cs/produkty/iphone-15/
                /en/products/iphone-15/
```

#### "default_clean_full"
```
Výchozí jazyk: /produkty/iphone-15/
Ostatní jazyky: /en-us/products/iphone-15/
```

### url_locale_format

Určuje formát locale kódů v URL:
- `"short"` - cs, en, de
- `"full"` - cs-cz, en-us, de-de

## 📄 Paginace

### pagination

```json
{
  "pagination": {
    "products_per_page": 12,
    "max_pagination_pages": 10,
    "show_first_last": true,
    "show_prev_next": true,
    "show_numbers": true
  }
}
```

| Parametr | Typ | Výchozí | Popis |
|----------|-----|---------|-------|
| `products_per_page` | number | 12 | Produktů na stránku |
| `max_pagination_pages` | number | 10 | Max. počet stránek v paginaci |
| `show_first_last` | boolean | true | Zobrazit "První"/"Poslední" |
| `show_prev_next` | boolean | true | Zobrazit "Předchozí"/"Další" |
| `show_numbers` | boolean | true | Zobrazit čísla stránek |

## 🔧 Vlastní proměnné

### variables

Systém variables umožňuje definovat vlastní data přístupná v templates.

```json
{
  "variables": {
    "contact": {
      "email": "info@myshop.com",
      "phone": "+420 123 456 789",
      "address": "Václavské náměstí 1, Praha",
      "business_hours": "Po-Pá 9-17h"
    },
    
    "social": {
      "facebook": "https://facebook.com/myshop",
      "instagram": "@myshop",
      "youtube": "https://youtube.com/@myshop",
      "twitter": "@myshop"
    },
    
    "navigation": {
      "header": [
        {"page": "o-nas"},
        {"page": "kontakt"},
        {"section": "blog"}
      ],
      "footer": [
        {"page": "doprava-a-platba"},
        {"page": "obchodni-podminky"},
        {"page": "reklamace"},
        {"page": "o-nas"}
      ]
    }
  }
}
```

### Automatická resoluce stránek

Při použití `{"page": "kontakt"}` GitCart automaticky:
1. Najde odpovídající markdown soubor
2. Načte frontmatter metadata
3. Vygeneruje URL
4. Rozšíří objekt o všechna pole z frontmatteru

Výsledek v template:
```liquid
{% for link in navigation.header %}
  <a href="{{ link.url }}" class="{{ link.custom_class }}">
    {% if link.icon %}<i class="icon-{{ link.icon }}"></i>{% endif %}
    {{ link.title }}
  </a>
{% endfor %}
```

## 🔍 SEO nastavení

### seo

```json
{
  "seo": {
    "default_title_suffix": " | Můj E-shop",
    "default_description": "Kvalitní produkty za skvělé ceny",
    "og_image": "assets/images/og-default.jpg",
    "twitter_site": "@myshop",
    "schema_org": {
      "organization": {
        "name": "Můj E-shop s.r.o.",
        "logo": "https://myshop.com/logo.png",
        "url": "https://myshop.com",
        "sameAs": [
          "https://facebook.com/myshop",
          "https://instagram.com/myshop"
        ]
      }
    }
  }
}
```

## 🏗️ Build nastavení

### build

```json
{
  "build": {
    "minify_css": true,
    "minify_js": true,
    "optimize_images": true,
    "generate_sitemap": true,
    "generate_robots": true,
    "clean_urls": true,
    "output_dir": "dist"
  }
}
```


## 🔌 Plugin konfigurace

Pluginy mohou mít svoji sekci v site.json:

```json
{
  "plugins": {
    "google-analytics": {
      "tracking_id": "G-XXXXXXXXXX",
      "enable_ecommerce": true,
      "anonymize_ip": true
    },
    
    "email-notifications": {
      "provider": "mailgun",
      "api_key": "key-xyz",
      "domain": "myshop.com",
      "owner_email": "orders@myshop.com",
      "enable_customer_confirmation": true
    },
    
    "chat-widget": {
      "provider": "tawk",
      "widget_id": "xxx",
      "position": "bottom-right",
      "show_on_mobile": true
    }
  }
}
```

## 🧪 Development nastavení

```json
{
  "dev": {
    "port": 3000,
    "host": "localhost",
    "open_browser": true,
    "live_reload": true,
    "watch_files": [
      "**/*.md",
      "**/*.json", 
      "templates/**/*",
      "plugins/**/*"
    ]
  }
}
```

## ✅ Validace konfigurace

GitCart automaticky validuje site.json při buildu:

### Povinné pole
- `name` - název e-shopu
- `url` - produkční URL
- `languages` - minimálně jeden jazyk

### Automatické kontroly
- ✅ Validní URL formát
- ✅ Unikátní language shorty
- ✅ Právě jeden výchozí jazyk
- ✅ Existující page/section reference v navigation
- ✅ Validní currency kódy
- ✅ Logické paginace hodnoty

### Příklad chybové zprávy
```
❌ Errors found (3):
  site.json:15 - Missing required field 'name'
  site.json:32 - Invalid URL format in languages.cs-cz.url
  site.json:45 - Page reference 'kontakt' not found in cs-cz/pages/

⚠️  Warnings (1):
  site.json:25 - currencies should use ISO 4217 codes (CZK not Czech Republic Koruna)
```

## 🎯 Příklady použití

### Minimální konfigurace
```json
{
  "name": "Můj E-shop",
  "url": "https://myshop.com",
  "languages": {
    "cs-cz": {
      "name": "Čeština",
      "default": true,
      "short": "cs",
      "currency": "CZK"
    }
  }
}
```
