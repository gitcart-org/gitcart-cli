# GitCart - CLI Reference

Complete reference guide for all GitCart CLI commands.

## 🚀 Installation and Basic Usage

### Installation

```bash
# Global installation (recommended)
npm install -g gitcart-cli

# Local installation for project
npm install gitcart-cli

# Verify installation
gitcart --version          # with global installation only
npx gitcart --version      # with local installation
gitcart --help             # with global installation only
npx gitcart --help         # with local installation
```

### Global Parameters

All commands support these global parameters:

```bash
--help, -h        # Show help
--version, -v     # Show version
--verbose         # Detailed output
--quiet, -q       # Quiet mode (errors only)
--config <path>   # Custom path to site.json
--no-color        # Disable colored output
```

## 📁 Project Management

### `gitcart init`

Creates a new GitCart project with complete structure.

```bash
gitcart init <project-name> [options]
```

#### Parameters

```bash
--template <name>     # Template: default, minimal, blog (default: default)
--language <locale>   # Default language: cs-cz, en-us, de-de (default: en-us)
--force, -f          # Overwrite existing folder
--no-git             # Don't create git repository
--no-install         # Don't install dependencies
```

#### Examples

```bash
# Basic initialization
gitcart init my-shop

# With custom template
gitcart init my-shop --template minimal

# Multilingual project
gitcart init international-shop --language en-us

# Without git and npm install
gitcart init quick-project --no-git --no-install
```

#### Output

```
✅ Creating GitCart project: my-shop
📁 Creating project structure...
📄 Generating site.json configuration...
📦 Installing dependencies...
🔧 Initializing git repository...
🎉 Project created successfully!

Next steps:
  cd my-shop
  gitcart product add    # Add your first product
  gitcart dev           # Start development server
```

### `gitcart build`

Builds static files for production.

```bash
gitcart build [options]
```

#### Parameters

```bash
--output, -o <dir>   # Output directory (default: dist)
--clean             # Clean output directory before build
--watch, -w         # Watch mode - rebuild on changes
--minify            # Minify CSS/JS (default: true)
--sourcemap         # Generate source maps
--analyze           # Bundle size analysis
```

#### Examples

```bash
# Basic build
gitcart build

# Build with cleaning and minification
gitcart build --clean --minify

# Watch mode for development
gitcart build --watch

# Build to custom directory
gitcart build --output public

# Build with analysis
gitcart build --analyze
```

#### Output

```
🏗️  Building GitCart site...

📊 Build Statistics:
  - Languages: 2 (en-us, es-es)
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

Starts development server with hot reload.

```bash
gitcart dev [options]
```

#### Parameters

```bash
--port, -p <number>  # Server port (default: 3000)
--host <hostname>    # Hostname (default: localhost)
--open, -o          # Open browser automatically
--https             # Use HTTPS
--cert <path>       # Path to SSL certificate
--key <path>        # Path to SSL key
```

#### Examples

```bash
# Basic dev server
gitcart dev

# On custom port
gitcart dev --port 8080

# With automatic browser opening
gitcart dev --open

# HTTPS server
gitcart dev --https --cert ssl/cert.pem --key ssl/key.pem

# Server accessible on network
gitcart dev --host 0.0.0.0
```

#### Output

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

✨ Ready! Press Ctrl+C to stop
```

## 🛒 Content Management

### `gitcart product add`

Adds a new product with interactive wizard.

```bash
gitcart product add [options]
```

#### Parameters

```bash
--title <string>     # Product title
--price <number>     # Product price
--sku <string>       # Product SKU
--category <string>  # Category path
--language <locale>  # Target language (default: default language)
--template <path>    # Use product template file
--images <count>     # Number of image placeholders
--no-images         # Don't create image placeholders
```

#### Examples

```bash
# Interactive product creation
gitcart product add

# Quick product creation
gitcart product add --title "iPhone 15" --price 999 --sku "IPHONE15"

# Product with category
gitcart product add --category "electronics/phones"

# Product for specific language
gitcart product add --language "es-es"

# Product from template
gitcart product add --template "templates/product-template.md"
```

#### Interactive Flow

```
🛒 Adding new product...

? Product title: iPhone 15 128GB Blue
? Product SKU: IPHONE15-128-BLUE
? Price: 999
? Currency: USD
? Category: electronics > phones
? Language: en-us
? Number of images: 3
? Featured product? Yes
? Tags (comma-separated): smartphone, apple, 5g, premium

✅ Product created: en-us/categories/electronics/phones/.products/iphone-15.md
📁 Image placeholders created in: .images/
  - iphone-15-front.jpg
  - iphone-15-back.jpg
  - iphone-15-gallery-1.jpg

💡 Next steps:
  1. Add product images to en-us/categories/electronics/phones/.images/
  2. Edit product description in the markdown file
  3. Run 'gitcart dev' to preview changes
```

### `gitcart category add`

Creates a new product category.

```bash
gitcart category add [options]
```

#### Parameters

```bash
--title <string>     # Category title
--slug <string>      # URL slug
--parent <string>    # Parent category path
--language <locale>  # Target language
--description <text> # Category description
--featured          # Mark as featured category
```

#### Examples

```bash
# Interactive category creation
gitcart category add

# Quick category creation
gitcart category add --title "Electronics" --slug "electronics"

# Subcategory creation
gitcart category add --title "Smartphones" --parent "electronics"

# Category for specific language
gitcart category add --language "es-es"
```

### `gitcart page add`

Creates a new static page.

```bash
gitcart page add [options]
```

#### Parameters

```bash
--title <string>     # Page title
--slug <string>      # URL slug
--template <string>  # Template name (page, contact, about)
--language <locale>  # Target language
--nav-header        # Add to header navigation
--nav-footer        # Add to footer navigation
```

#### Examples

```bash
# Interactive page creation
gitcart page add

# Quick page creation
gitcart page add --title "About Us" --slug "about-us"

# Contact page with template
gitcart page add --title "Contact" --template "contact" --nav-footer

# Page for specific language
gitcart page add --language "es-es"
```

### `gitcart blog post add`

Creates a new blog post.

```bash
gitcart blog post add [options]
```

#### Parameters

```bash
--title <string>     # Post title
--slug <string>      # URL slug
--author <string>    # Post author
--category <string>  # Post category
--language <locale>  # Target language
--published         # Mark as published (default: draft)
--featured          # Mark as featured post
```

#### Examples

```bash
# Interactive blog post creation
gitcart blog post add

# Quick post creation
gitcart blog post add --title "How to Choose a Phone" --author "John Doe"

# Featured published post
gitcart blog post add --featured --published

# Post for specific language
gitcart blog post add --language "es-es"
```

## 🔌 Plugin Management

### `gitcart plugin list`

Lists all installed and available plugins.

```bash
gitcart plugin list [options]
```

#### Parameters

```bash
--installed         # Show only installed plugins
--available         # Show only available plugins
--detailed          # Show detailed information
```

#### Examples

```bash
# List all plugins
gitcart plugin list

# Only installed plugins
gitcart plugin list --installed

# Detailed information
gitcart plugin list --detailed
```

#### Output

```
📦 GitCart Plugins

✅ Installed (3):
  - google-analytics@1.2.0  (Analytics tracking)
  - email-notifications@1.0.0  (Order email notifications)
  - chat-widget@0.5.0  (Customer chat support)

📋 Available (8):
  - social-share@1.1.0  (Social media sharing)
  - product-reviews@2.0.0  (Customer reviews)
  - wishlist@1.0.0  (Product wishlist)
  - currency-converter@0.8.0  (Multi-currency support)
  - seo-optimizer@1.5.0  (Advanced SEO features)
```

### `gitcart plugin add`

Installs and configures a plugin.

```bash
gitcart plugin add <plugin-name> [options]
```

#### Parameters

```bash
--config <path>     # Custom configuration file
--no-config        # Skip configuration setup
--dev              # Install as development dependency
```

#### Examples

```bash
# Install plugin from npm
gitcart plugin add google-analytics

# Install with custom config
gitcart plugin add email-notifications --config config/email.json

# Install local plugin
gitcart plugin add ./plugins/my-custom-plugin.js

# Install development plugin
gitcart plugin add my-dev-plugin --dev
```

### `gitcart plugin remove`

Removes an installed plugin.

```bash
gitcart plugin remove <plugin-name> [options]
```

#### Parameters

```bash
--keep-config      # Keep plugin configuration
--force           # Force removal without confirmation
```

### `gitcart plugin create`

Creates a new plugin template.

```bash
gitcart plugin create <plugin-name> [options]
```

#### Parameters

```bash
--type <type>      # Plugin type: analytics, payment, shipping, content
--events <list>    # Comma-separated list of events to handle
--zones <list>     # Comma-separated list of rendering zones
--template <path>  # Use custom plugin template
```

#### Examples

```bash
# Interactive plugin creation
gitcart plugin create my-plugin

# Analytics plugin
gitcart plugin create ga-tracking --type analytics

# Payment plugin with events
gitcart plugin create stripe-payment --type payment --events "order:completed,payment:selected"

# Content plugin with zones
gitcart plugin create product-features --type content --zones "product-detail-after-images"
```

## 🌍 Multilingual Management

### `gitcart multilingual check`

Checks for missing translations and content inconsistencies.

```bash
gitcart multilingual check [options]
```

#### Parameters

```bash
--language <locale> # Check specific language
--fix              # Auto-fix simple issues
--report <format>  # Generate report (json, html, csv)
```

### `gitcart multilingual scaffold`

Creates translation templates for missing content.

```bash
gitcart multilingual scaffold [options]
```

#### Parameters

```bash
--language <locale> # Target language for scaffolding
--source <locale>   # Source language (default: default language)
--missing          # Only scaffold missing translations
--overwrite        # Overwrite existing files
```

### `gitcart multilingual sync`

Synchronizes content structure between languages.

```bash
gitcart multilingual sync [options]
```

## 🧪 Development Tools

### `gitcart validate`

Validates project configuration and content.

```bash
gitcart validate [options]
```

#### Parameters

```bash
--config           # Validate only configuration
--content          # Validate only content
--links            # Check for broken links
--images           # Check for missing images
--seo             # Run SEO validation
```

### `gitcart clean`

Cleans build artifacts and temporary files.

```bash
gitcart clean [options]
```

#### Parameters

```bash
--cache            # Clear build cache
--dist             # Remove dist directory
--logs             # Clear log files
--all              # Clean everything
```

### `gitcart info`

Shows project information and diagnostics.

```bash
gitcart info [options]
```

#### Parameters

```bash
--system           # Show system information
--dependencies     # Show dependency versions
--config           # Show resolved configuration
```

#### Output

```
📊 GitCart Project Info

🏷️  Project:
  Name: My E-shop
  Version: 1.0.0
  Languages: en-us (default), es-es
  URL: https://myshop.com

📈 Content Statistics:
  Categories: 5
  Products: 127
  Pages: 8
  Blog posts: 23
  Images: 245

🔧 Configuration:
  Template engine: liquid
  URL strategy: default_clean
  Build output: dist/
  Dev server: localhost:3000

🔌 Plugins (3):
  ✅ google-analytics@1.2.0
  ✅ email-notifications@1.0.0
  ✅ chat-widget@0.5.0

💻 System:
  Node.js: v18.17.0
  GitCart CLI: v1.5.2
  OS: macOS 13.4.1
```

## 🚀 Deployment Commands

### `gitcart deploy`

Deploys the site to various hosting platforms.

```bash
gitcart deploy <platform> [options]
```

#### Supported Platforms

```bash
# GitHub Pages
gitcart deploy github --repo username/repo

# Netlify
gitcart deploy netlify --site-id abc123

# Vercel
gitcart deploy vercel --project my-project

# FTP
gitcart deploy ftp --host ftp.myhost.com --user myuser

# Custom
gitcart deploy custom --script deploy.sh
```

### `gitcart export`

Exports project data in various formats.

```bash
gitcart export <format> [options]
```

#### Formats

```bash
# JSON export
gitcart export json --output data.json

# CSV export (products)
gitcart export csv --type products --output products.csv

# Backup
gitcart export backup --output backup.tar.gz
```

## ⚙️ Configuration Commands

### `gitcart config get`

Gets configuration values.

```bash
gitcart config get [key]
```

### `gitcart config set`

Sets configuration values.

```bash
gitcart config set <key> <value>
```

### `gitcart config list`

Lists all configuration values.

```bash
gitcart config list
```

## 🔧 Troubleshooting

### Common Exit Codes

```bash
0   # Success
1   # General error
2   # Configuration error
3   # Content validation error
4   # Build error
5   # Plugin error
10  # Network error
```

### Debug Mode

```bash
# Enable debug logging
DEBUG=gitcart:* gitcart build

# Debug specific modules
DEBUG=gitcart:build,gitcart:plugins gitcart dev

# Full verbose output
gitcart build --verbose
```

### Common Issues

#### Port Already in Use
```bash
# Use different port
gitcart dev --port 8080

# Kill existing process
lsof -ti:3000 | xargs kill -9
```

#### Permission Issues
```bash
# Fix permissions
chmod -R 755 dist/

# Use sudo for global install
sudo npm install -g gitcart-cli
```

#### Memory Issues
```bash
# Increase Node.js memory limit
NODE_OPTIONS="--max-old-space-size=4096" gitcart build
```