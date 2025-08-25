# GitCart - Configuration

Complete reference guide for GitCart project configuration.

## 📝 site.json - Main Configuration

The `site.json` file is the heart of every GitCart project. It defines all basic settings, languages, URL strategies, and custom variables.

### 🏗️ Basic Structure

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

## 🌍 Language Configuration

### languages

Defines all languages supported by your e-shop.

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

#### Language Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | ✅ | Human-readable language name |
| `default` | boolean | - | Default language (only one) |
| `short` | string | ✅ | Short code for URLs (cs, en, de) |
| `currency` | string | ✅ | Currency (CZK, EUR, USD) |
| `date_format` | string | - | Date format (default: d.m.Y) |
| `number_format` | object | - | Number formatting |

## 🔗 URL Strategies

### url_strategy

Determines how URL addresses will be generated for different languages.

#### "default_clean" (recommended)
```
Default language: /products/iphone-15/
Other languages: /en/products/iphone-15/
```

#### "always_full" 
```
All languages: /cs-cz/products/iphone-15/
                /en-us/products/iphone-15/
```

#### "always_short"
```
All languages: /cs/products/iphone-15/
                /en/products/iphone-15/
```

#### "default_clean_full"
```
Default language: /products/iphone-15/
Other languages: /en-us/products/iphone-15/
```

### url_locale_format

Determines the format of locale codes in URLs:
- `"short"` - cs, en, de
- `"full"` - cs-cz, en-us, de-de

## 📄 Pagination

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

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `products_per_page` | number | 12 | Products per page |
| `max_pagination_pages` | number | 10 | Max. number of pages in pagination |
| `show_first_last` | boolean | true | Show "First"/"Last" |
| `show_prev_next` | boolean | true | Show "Previous"/"Next" |
| `show_numbers` | boolean | true | Show page numbers |

## 🔧 Custom Variables

### variables

The variables system allows defining custom data accessible in templates.

```json
{
  "variables": {
    "contact": {
      "email": "info@myshop.com",
      "phone": "+420 123 456 789",
      "address": "Wenceslas Square 1, Prague",
      "business_hours": "Mon-Fri 9-5pm"
    },
    
    "social": {
      "facebook": "https://facebook.com/myshop",
      "instagram": "@myshop",
      "youtube": "https://youtube.com/@myshop",
      "twitter": "@myshop"
    },
    
    "navigation": {
      "header": [
        {"page": "about-us"},
        {"page": "contact"},
        {"section": "blog"}
      ],
      "footer": [
        {"page": "shipping-payment"},
        {"page": "terms-conditions"},
        {"page": "returns"},
        {"page": "about-us"}
      ]
    }
  }
}
```

### Automatic Page Resolution

When using `{"page": "contact"}` GitCart automatically:
1. Finds the corresponding markdown file
2. Loads frontmatter metadata
3. Generates URL
4. Expands the object with all frontmatter fields

Result in template:
```liquid
{% for link in navigation.header %}
  <a href="{{ link.url }}" class="{{ link.custom_class }}">
    {% if link.icon %}<i class="icon-{{ link.icon }}"></i>{% endif %}
    {{ link.title }}
  </a>
{% endfor %}
```

## 🔍 SEO Settings

### seo

```json
{
  "seo": {
    "default_title_suffix": " | My E-shop",
    "default_description": "Quality products at great prices",
    "og_image": "assets/images/og-default.jpg",
    "twitter_site": "@myshop",
    "schema_org": {
      "organization": {
        "name": "My E-shop Ltd.",
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

## 🏗️ Build Settings

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


## 🔌 Plugin Configuration

Plugins can have their own section in site.json:

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

## 🧪 Development Settings

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

## ✅ Configuration Validation

GitCart automatically validates site.json during build:

### Required Fields
- `name` - e-shop name
- `url` - production URL
- `languages` - at least one language

### Automatic Checks
- ✅ Valid URL format
- ✅ Unique language shorties
- ✅ Exactly one default language
- ✅ Existing page/section references in navigation
- ✅ Valid currency codes
- ✅ Logical pagination values

### Example Error Message
```
❌ Errors found (3):
  site.json:15 - Missing required field 'name'
  site.json:32 - Invalid URL format in languages.cs-cz.url
  site.json:45 - Page reference 'contact' not found in cs-cz/pages/

⚠️  Warnings (1):
  site.json:25 - currencies should use ISO 4217 codes (CZK not Czech Republic Koruna)
```

## 🎯 Usage Examples

### Minimal Configuration
```json
{
  "name": "My E-shop",
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