# GitCart - CLI Reference

Kompletní referenční příručka pro všechny CLI příkazy GitCart.

## 🚀 Instalace a základní použití

### Instalace

```bash
# Globální instalace
npm install -g gitcart-cli

# Lokální instalace pro projekt
npm install gitcart-cli

# Ověření instalace
gitcart --version
gitcart --help
```

### Globální parametry

Všechny příkazy podporují tyto globální parametry:

```bash
--help, -h        # Zobrazí nápovědu
--version, -v     # Zobrazí verzi
--verbose         # Detailní výstup
--quiet, -q       # Tichý režim (pouze chyby)
--config <path>   # Vlastní cesta k site.json
--no-color        # Vypnout barevný výstup
```

## 📁 Správa projektu

### `gitcart init`

Vytvoří nový GitCart projekt s kompletní strukturou.

```bash
gitcart init <název-projektu> [options]
```

#### Parametry

```bash
--template <name>     # Template: default, minimal, blog (výchozí: default)
--language <locale>   # Výchozí jazyk: cs-cz, en-us, de-de (výchozí: cs-cz)
--force, -f          # Přepsat existující složku
--no-git             # Nevytvářet git repozitář
--no-install         # Neinstalovat dependencies
```

#### Příklady

```bash
# Základní inicializace
gitcart init muj-eshop

# S vlastním template
gitcart init muj-eshop --template minimal

# Vícejazyčný projekt
gitcart init international-shop --language en-us

# Bez git a npm install
gitcart init quick-project --no-git --no-install
```

#### Výstup

```
✅ Creating GitCart project: muj-eshop
📁 Creating project structure...
📄 Generating site.json configuration...
📦 Installing dependencies...
🔧 Initializing git repository...
🎉 Project created successfully!

Next steps:
  cd muj-eshop
  gitcart product add    # Add your first product
  gitcart dev           # Start development server
```

### `gitcart build`

Vybuiluje statické soubory pro produkci.

```bash
gitcart build [options]
```

#### Parametry

```bash
--output, -o <dir>   # Výstupní složka (výchozí: dist)
--clean             # Vyčistit výstupní složku před buildem
--watch, -w         # Watch mód - rebuild při změnách
--minify            # Minifikace CSS/JS (výchozí: true)
--sourcemap         # Generovat source maps
--analyze           # Analýza bundle velikosti
```

#### Příklady

```bash
# Základní build
gitcart build

# Build s čištěním a minifikací
gitcart build --clean --minify

# Watch mód pro development
gitcart build --watch

# Build do vlastní složky
gitcart build --output public

# Build s analýzou
gitcart build --analyze
```

#### Výstup

```
🏗️  Building GitCart site...

📊 Build Statistics:
  - Languages: 2 (cs-cz, en-us)
  - Categories: 5 
  - Products: 127
  - Pages: 8
  - Blog posts: 23

📝 Generated files:
  - HTML pages: 189
  - CSS files: 3 (minified)
  - JS files: 7 (minified)
  - Images: 245 (optimized)
  - Sitemap: ✅
  - Robots.txt: ✅

⚡ Build completed in 3.2s
📦 Total size: 12.4 MB (compressed: 3.1 MB)
```

### `gitcart dev`

Spustí development server s hot reload.

```bash
gitcart dev [options]
```

#### Parametry

```bash
--port, -p <number>  # Port serveru (výchozí: 3000)
--host <hostname>    # Hostname (výchozí: localhost)
--open, -o          # Otevřít browser automaticky
--https             # Používat HTTPS
--cert <path>       # Cesta k SSL certifikátu
--key <path>        # Cesta k SSL klíči
```

#### Příklady

```bash
# Základní dev server
gitcart dev

# Na vlastním portu
gitcart dev --port 8080

# S automatickým otevřením browseru
gitcart dev --open

# HTTPS server
gitcart dev --https --cert ssl/cert.pem --key ssl/key.pem

# Server dostupný v síti
gitcart dev --host 0.0.0.0
```

#### Výstup

```
🚀 Starting GitCart development server...

📡 Server running at:
  ➜ Local:   http://localhost:3000/
  ➜ Network: http://192.168.1.100:3000/

🔄 Watching for changes:
  - Content files (*.md, *.json)
  - Templates (*.liquid, *.njk)
  - Assets (*.css, *.js, images)
  - Plugins (plugins/*.js)

✅ Ready! Press Ctrl+C to stop.
```

## 🛍️ Správa obsahu

### `gitcart product add`

Přidá nový produkt do e-shopu.

```bash
gitcart product add [options]
```

#### Parametry

```bash
--name <title>       # Název produktu
--sku <sku>         # SKU kód
--price <amount>    # Cena
--category <slug>   # Kategorie
--language <locale> # Jazyk (výchozí: default language)
--featured          # Označit jako doporučený
--images <count>    # Počet placeholder obrázků
--template <path>   # Vlastní template produktu
```

#### Interaktivní režim

Bez parametrů se spustí interaktivní průvodce:

```bash
gitcart product add

? Product name: iPhone 15
? SKU: IPHONE15-128
? Price (CZK): 25999
? Sale price (optional): 23999
? Category: elektronika
? Language: cs-cz
? Stock quantity: 15
? Variant (optional): blue-128gb
? Tags (comma separated): smartphone,apple,5g
? Number of placeholder images: 3
? Featured product: Yes
? Generate sample content: Yes

✅ Product created: cs-cz/categories/elektronika/.products/iphone-15.md
📁 Created image placeholders in .images/
🎯 Added to search index
```

#### Příklady

```bash
# Interaktivní vytvoření
gitcart product add

# S parametry
gitcart product add --name "iPhone 15" --sku "IPHONE15" --price 25999 --category elektronika

# Pro konkrétní jazyk
gitcart product add --language en-us --category electronics

# S označením jako featured
gitcart product add --featured --images 5
```

### `gitcart category add`

Přidá novou kategorii produktů.

```bash
gitcart category add [options]
```

#### Parametry

```bash
--name <title>      # Název kategorie
--slug <slug>       # URL slug
--parent <slug>     # Nadřazená kategorie
--language <locale> # Jazyk
--featured          # Zobrazit v navigaci
--description <text> # Krátký popis
```

#### Interaktivní režim

```bash
gitcart category add

? Category name: Elektronika
? URL slug: elektronika
? Description: Nejnovější elektronické zařízení
? Parent category: none
? Language: cs-cz
? Featured in navigation: Yes
? Sort order: 1
? Create sample product: Yes
? Banner image name: elektronika-banner.jpg

✅ Category created: cs-cz/categories/elektronika/.category.md
📁 Created directory structure
📄 Generated sample product: samsung-galaxy.md
```

#### Příklady

```bash
# Základní kategorie
gitcart category add

# Podkategorie
gitcart category add --parent elektronika --name "Mobilní telefony"

# Pro více jazyků
gitcart category add --name "Electronics" --language en-us
```

### `gitcart page add`

Vytvoří novou statickou stránku.

```bash
gitcart page add [options]
```

#### Parametry

```bash
--name <title>       # Název stránky
--slug <slug>        # URL slug
--template <name>    # Template: page, contact, about
--language <locale>  # Jazyk
--nav-header        # Přidat do hlavní navigace
--nav-footer        # Přidat do footer navigace
--icon <name>       # Ikona pro navigaci
```

#### Interaktivní režim

```bash
gitcart page add

? Page name: Kontakt
? URL slug: kontakt
? Template: contact
? Language: cs-cz
? Add to header navigation: No
? Add to footer navigation: Yes
? Navigation order: 1
? Icon for navigation: phone
? Generate sample content: Yes

✅ Page created: cs-cz/pages/kontakt.md
🔗 Updated navigation in site.json
```

### `gitcart blog post add`

Přidá nový blog článek.

```bash
gitcart blog post add [options]
```

#### Parametry

```bash
--title <title>      # Název článku
--slug <slug>        # URL slug
--category <name>    # Blog kategorie
--author <name>      # Autor
--language <locale>  # Jazyk
--date <date>        # Datum publikace (YYYY-MM-DD)
--featured           # Doporučený článek
```

#### Interaktivní režim

```bash
gitcart blog post add

? Post title: Jak vybrat telefon v roce 2024
? URL slug: jak-vybrat-telefon
? Category: tipy
? Author: Jan Novák
? Language: cs-cz  
? Publication date: 2024-01-15
? Featured post: Yes
? Tags: telefon,výběr,2024
? Generate sample content: Yes

✅ Blog post created: cs-cz/blog/2024/jak-vybrat-telefon.md
📝 Added to blog index
🏷️ Created new category: tipy
```

## 🔌 Správa pluginů

### `gitcart plugin list`

Zobrazí seznam nainstalovaných pluginů.

```bash
gitcart plugin list [options]
```

#### Parametry

```bash
--enabled           # Pouze povolené pluginy
--disabled          # Pouze zakázané pluginy
--details, -d       # Detailní informace
```

#### Výstup

```bash
gitcart plugin list --details

📦 Installed Plugins (5):

✅ google-analytics (v1.2.0)
   📄 E-commerce tracking with GA4
   📁 plugins/google-analytics.js
   ⚡ Events: 4, Zones: 1
   🎛️ Config: trackingId, enableEcommerce

✅ email-notifications (v1.0.0)  
   📄 Order notifications via Mailgun
   📁 plugins/email-notifications.js
   ⚡ Events: 1, Zones: 1
   🎛️ Config: apiKey, domain, fromEmail

❌ chat-widget (v1.0.0) [DISABLED]
   📄 Live chat integration
   📁 plugins/chat-widget.js
   ⚡ Events: 2, Zones: 2
   🎛️ Config: provider, widgetId

📊 Plugin Statistics:
   - Total plugins: 5
   - Enabled: 4
   - Event listeners: 12
   - Render zones: 8
```

### `gitcart plugin add`

Nainstaluje nový plugin.

```bash
gitcart plugin add <source> [options]
```

#### Zdroje pluginů

```bash
# Z npm registry
gitcart plugin add gitcart-plugin-stripe

# Z GitHub
gitcart plugin add github:user/repo

# Z lokálního souboru
gitcart plugin add ./my-plugin.js

# Z URL
gitcart plugin add https://example.com/plugin.js
```

#### Parametry

```bash
--enable            # Povolit hned po instalaci (výchozí)
--disable           # Zakázat po instalaci
--config <json>     # Konfigurace jako JSON
--save-dev         # Přidat do devDependencies
```

#### Příklady

```bash
# Instalace z npm
gitcart plugin add gitcart-plugin-stripe

# Z GitHub s konfigurací
gitcart plugin add github:gitcart/plugin-paypal --config '{"clientId":"xxx"}'

# Lokální plugin
gitcart plugin add ./custom-plugin.js --disable
```

#### Výstup

```
📦 Installing plugin: gitcart-plugin-stripe
⬇️  Downloading from npm registry...
✅ Plugin installed successfully

🔧 Plugin configuration:
   Name: Stripe Payment Gateway
   Version: 2.1.0
   Events: gitcart:payment:selected
   Zones: payment-selection

💡 Next steps:
   1. Configure your Stripe keys in site.json
   2. Run 'gitcart build' to activate
   3. Test with 'gitcart dev'
```

### `gitcart plugin remove`

Odstraní plugin.

```bash
gitcart plugin remove <name> [options]
```

#### Parametry

```bash
--keep-config      # Zachovat konfiguraci v site.json
--force, -f        # Vynutit odstranění bez potvrzení
```

### `gitcart plugin create`

Vytvoří nový vlastní plugin.

```bash
gitcart plugin create <name> [options]
```

#### Parametry

```bash
--type <type>      # Typ: analytics, payment, shipping, content
--events <list>    # Seznam událostí (oddělené čárkou)
--zones <list>     # Render zones (oddělené čárkou)
--template <name>  # Template: basic, advanced, ecommerce
```

#### Interaktivní režim

```bash
gitcart plugin create my-shipping-plugin

? Plugin name: Custom Shipping Calculator
? Description: Calculate shipping costs based on weight and distance
? Plugin type: shipping
? Events to handle: gitcart:shipping:calculate,gitcart:checkout:shipping
? Render zones: shipping-selection,checkout-summary
? Include configuration: Yes
? Generate example code: Yes
? Add to .gitignore: No

✅ Plugin created: plugins/my-shipping-plugin.js
📄 Generated with example code and configuration
🎯 Ready for customization
```

### `gitcart plugin enable/disable`

Povolí nebo zakáže plugin.

```bash
gitcart plugin enable <name>
gitcart plugin disable <name>
```

## 🌍 Vícejazyčnost

### `gitcart multilingual check`

Zkontroluje chybějící překlady.

```bash
gitcart multilingual check [options]
```

#### Parametry

```bash
--language <locale> # Kontrola konkrétního jazyka
--content <type>    # Typ obsahu: products, categories, pages, blog
--fix               # Automaticky opravit jednoduché problémy
```

#### Výstup

```
🔍 Checking multilingual content...

❌ Missing translations found:

📦 Products (3 issues):
  - samsung-galaxy missing in: en-us, de-de
  - iphone-15 missing meta description in: en-us
  - xiaomi-redmi missing images in: de-de

📁 Categories (1 issue):
  - elektronika missing translation in: de-de

📄 Pages (2 issues):
  - doprava-a-platba missing in: en-us
  - reklamace missing seo_title in: all languages

💡 Suggestions:
  - Run 'gitcart multilingual scaffold --missing' to create templates
  - Use 'gitcart multilingual sync' to copy content between languages
```

### `gitcart multilingual scaffold`

Vytvoří šablony pro chybějící překlady.

```bash
gitcart multilingual scaffold [options]
```

#### Parametry

```bash
--missing          # Pouze chybějící překlady
--language <locale> # Pro konkrétní jazyk
--content <type>   # Typ obsahu
--template         # Vytvořit pouze template (bez obsahu)
```


## 🔍 Utility příkazy

### `gitcart validate`

Validuje obsah a konfiguraci.

```bash
gitcart validate [options]
```

#### Parametry

```bash
--content          # Validace markdown souborů
--config           # Validace site.json
--images           # Kontrola chybějících obrázků
--links            # Kontrola broken links
--seo              # SEO kontrola (meta tagy, titulky)
```

#### Výstup

```
🔍 Validating GitCart project...

✅ Configuration (site.json):
   - Valid JSON syntax
   - All required fields present
   - Language configuration valid

❌ Content validation (3 errors, 2 warnings):
   
   Errors:
   - cs-cz/categories/elektronika/.products/iphone-15.md:5
     Missing required field: price
   - cs-cz/pages/kontakt.md:3
     Invalid email format in frontmatter
   - en-us/categories/electronics/.category.md:1
     Invalid YAML syntax
   
   Warnings:
   - cs-cz/categories/elektronika/.products/samsung.md:8
     No images defined for product
   - cs-cz/pages/o-nas.md:2
     Meta description too short (< 150 chars)

📷 Image validation:
   - 23 images referenced, 21 found
   - Missing: iphone-15-gallery-3.jpg, samsung-box.jpg

🔗 Link validation:
   - 156 internal links checked
   - 2 broken links found:
     * /old-category/ (referenced in kontakt.md)
     * /blog/old-post/ (referenced in navigation)

📊 SEO validation:
   - 45 pages analyzed
   - 3 pages missing meta descriptions
   - 2 titles over 60 characters
   - All images have alt tags
```

### `gitcart clean`

Vyčistí dočasné soubory a cache.

```bash
gitcart clean [options]
```

#### Parametry

```bash
--dist             # Vyčistit dist/ složku
--cache            # Vyčistit build cache
--temp             # Vyčistit dočasné soubory
--node-modules     # Reinstalovat node_modules
--all              # Vyčistit vše
```

### `gitcart info`

Zobrazí informace o projektu.

```bash
gitcart info [options]
```

#### Parametry

```bash
--system           # Systémové informace
--dependencies     # Seznam závislostí
--content          # Statistiky obsahu
--config           # Přehled konfigurace
```

#### Výstup

```
📊 GitCart Project Information

🏷️  Project Details:
   Name: Můj E-shop
   Version: 0.1.1
   GitCart CLI: 1.0.0
   Node.js: 18.17.0

🌍 Languages (2):
   - cs-cz (Čeština) [DEFAULT]
   - en-us (English)

📦 Content Statistics:
   - Categories: 5
   - Products: 127 (cs: 127, en: 89)
   - Pages: 8 (cs: 8, en: 6)  
   - Blog posts: 23 (cs: 23, en: 12)
   - Images: 245

🔌 Plugins (4 active):
   - google-analytics (v1.2.0)
   - email-notifications (v1.0.0)
   - social-share (v1.0.0)
   - product-reviews (v1.0.0)

⚙️  Build Configuration:
   - Template engine: liquid
   - URL strategy: default_clean
   - Minification: enabled
   - Source maps: disabled
```

## 🧪 Testing a debugging

### `gitcart test`

Spustí testy projektu.

```bash
gitcart test [options]
```

#### Parametry

```bash
--content          # Testovat markdown obsah
--links            # Test broken links
--performance      # Performance testy
--seo              # SEO testy
--plugins          # Testovat pluginy
--watch            # Watch mód
```

### `gitcart debug`

Debug informace a diagnostika.

```bash
gitcart debug [command] [options]
```

#### Subpříkazy

```bash
gitcart debug build     # Debug build procesu
gitcart debug plugins   # Debug pluginů
gitcart debug routes    # Debug URL generování
gitcart debug templates # Debug template renderingu
```

## ⚡ Performance a optimalizace

GitCart generátor automaticky optimalizuje výstupy během build procesu. Pro ruční analýzu výkonu použijte `gitcart audit` příkaz.

### `gitcart audit`

Audit výkonu a SEO.

```bash
gitcart audit [options]
```

#### Parametry

```bash
--performance      # Performance audit
--seo              # SEO audit
--accessibility    # Accessibility audit
--best-practices   # Best practices
--lighthouse       # Lighthouse audit (vyžaduje Chrome)
```

## 🚀 Deployment

### `gitcart deploy`

Deploy do produkce.

```bash
gitcart deploy <target> [options]
```

#### Podporované targets

```bash
gitcart deploy netlify    # Netlify deployment
gitcart deploy vercel     # Vercel deployment  
gitcart deploy github     # GitHub Pages
gitcart deploy ftp        # FTP server
gitcart deploy s3         # Amazon S3
```

#### Parametry

```bash
--build            # Build před deploymentem
--config <file>    # Deploy konfigurace
--dry-run          # Test bez skutečného deploye
--message <msg>    # Commit message pro git-based deploy
```

## 🔧 Konfigurace CLI

### `gitcart config`

Správa CLI konfigurace.

```bash
gitcart config <command> [options]
```

#### Subpříkazy

```bash
gitcart config list                    # Seznam všech nastavení
gitcart config get <key>              # Zobrazit hodnotu
gitcart config set <key> <value>      # Nastavit hodnotu
gitcart config delete <key>           # Smazat nastavení
gitcart config reset                  # Reset na výchozí
```

#### Příklady konfigurace

```bash
# Výchozí template
gitcart config set defaults.template minimal

# Výchozí jazyk
gitcart config set defaults.language en-us

# Editor pro scaffolding
gitcart config set editor.command "code"

# Dev server nastavení
gitcart config set dev.port 8080
gitcart config set dev.open true
```

### Konfigurační soubor

CLI ukládá konfiguraci do `~/.gitcart/config.json`:

```json
{
  "defaults": {
    "template": "default",
    "language": "cs-cz",
    "author": "Your Name"
  },
  "dev": {
    "port": 3000,
    "open": true,
    "host": "localhost"
  },
  "build": {
    "minify": true,
    "sourcemap": false
  },
  "editor": {
    "command": "code"
  }
}
```

## 📝 Příklady workflow

### Typický development workflow

```bash
# 1. Vytvoření projektu
gitcart init my-shop
cd my-shop

# 2. Přidání obsahu
gitcart category add
gitcart product add

# 3. Development
gitcart dev --open

# 4. Přidání dalšího obsahu
gitcart product add --category elektronika
gitcart page add --nav-footer

# 5. Build a test
gitcart build
gitcart validate

# 6. Deploy
gitcart deploy netlify
```

### Plugin development workflow

```bash
# 1. Vytvoření pluginu
gitcart plugin create payment-gateway

# 2. Vývoj a testování
gitcart dev --watch

# 3. Testování pluginu
gitcart test --plugins

# 4. Aktivace
gitcart plugin enable payment-gateway

# 5. Build s pluginem
gitcart build
```

### Vícejazyčný workflow

```bash
# 1. Kontrola chybějících překladů
gitcart multilingual check

# 2. Vytvoření šablon
gitcart multilingual scaffold --missing

# 3. Validace
gitcart validate --content

# 4. Build všech jazyků
gitcart build
```

## 🔗 Exit kódy

CLI vrací standardní exit kódy:

```bash
0   # Úspěch
1   # Obecná chyba
2   # Chybné použití (špatné parametry)
3   # Chyba konfigurace
4   # Chyba obsahu/validace
5   # Network/IO chyba
130 # Přerušeno uživatelem (Ctrl+C)
```

Tyto exit kódy můžete používat v CI/CD pipeline pro automatizaci.