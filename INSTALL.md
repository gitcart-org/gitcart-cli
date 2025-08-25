# GitCart CLI - Standalone Distribution

## Installation Options

### Option 1: Global Installation (Recommended)
```bash
npm install -g gitcart-cli
```

### Option 2: Local Installation with npx
```bash
npm install gitcart-cli
# Then use with npx:
npx gitcart init my-shop
```

### Option 3: Direct Binary Usage
Download the appropriate binary for your platform from GitHub releases:
- **Linux**: `gitcart-linux`
- **Windows**: `gitcart-win.exe`  
- **macOS**: `gitcart-macos`

Make it executable and add to PATH:
```bash
# Linux/macOS
chmod +x gitcart-*
sudo mv gitcart-* /usr/local/bin/gitcart

# Windows - Add gitcart-win.exe to your PATH as gitcart.exe
```

## Usage

### With Global Installation
```bash
gitcart init my-shop
gitcart build
gitcart dev
gitcart --help
```

### With Local Installation (npx)
```bash
npx gitcart init my-shop
npx gitcart build  
npx gitcart dev
npx gitcart --help
```

## Documentation

Full documentation is available in the `docs/` folder or online at:
- 🇺🇸 [English Documentation](docs/en-us/README.md)
- 🇨🇿 [Czech Documentation](docs/cs-cz/README.md)

## Templates

Default templates are included in the `templates/` folder.

---

**GitCart** - Universal static e-shop generator
