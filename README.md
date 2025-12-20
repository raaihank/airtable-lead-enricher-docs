# Documentation Site

This directory contains the GitHub Pages documentation site for the Airtable Lead Enricher.

**Live Site:** https://raaihank.github.io/airtable-lead-enricher-docs/

## Structure

```
docs/
├── index.html                  # Main landing page
├── extension.html              # Extension guide with screenshots (NEW!)
├── configuration.html          # Configuration reference
├── integrations.html          # API integration guide
├── terms.html                 # Terms of Service
├── privacy.html               # Privacy Policy
├── screenshots/               # Extension screenshots (NEW!)
│   ├── README.md             # Screenshot placeholders guide
│   ├── 01-welcome-screen.png
│   ├── 02-step1-api-keys.png
│   ├── 03-step2-field-mapping.png
│   ├── 04-step3-select-enrich.png
│   ├── 05-enrichment-progress.png
│   └── 06-enrichment-complete.png
├── SCREENSHOT_GUIDE.md        # How to capture screenshots (NEW!)
├── AIRTABLE_BUTTON_SETUP.md  # Airtable button setup guide
├── DEMO_CHECKLIST.md         # Demo preparation checklist
├── YOUR_SETUP_GUIDE.md       # Setup guide
└── _config.yml               # GitHub Pages config

```

## Pages

### 📱 Extension Guide (NEW!)
**File:** `extension.html`
**URL:** https://raaihank.github.io/airtable-lead-enricher-docs/extension.html

Complete visual guide for the Airtable extension including:
- Installation steps
- Step-by-step walkthrough with screenshots
- Field mapping guide
- Troubleshooting
- Best practices
- Version history

**Features:**
- 6 screenshot placeholders ready for images
- Comprehensive documentation with tables
- Consistent styling with other pages
- Linked from main index page

### 🏠 Main Landing Page
**File:** `index.html`
**URL:** https://raaihank.github.io/airtable-lead-enricher-docs/

Overview of the project with links to all documentation sections.

### ⚙️ Configuration Guide
**File:** `configuration.html`

Detailed reference for all configuration options, input schema, and examples.

### 🔌 Integrations Guide
**File:** `integrations.html`

How to use the actor via API mode without Airtable (Lambda, Airflow, custom workflows).

### 📜 Legal Pages
- **Terms of Service:** `terms.html`
- **Privacy Policy:** `privacy.html`

Both linked from extension footer and welcome screen.

## Adding Screenshots

### Quick Start

1. **Capture screenshots** following `SCREENSHOT_GUIDE.md`
2. **Save to** `docs/screenshots/` with correct naming:
   - `01-welcome-screen.png`
   - `02-step1-api-keys.png`
   - etc.
3. **Replace placeholders** in `extension.html`:
   ```html
   <!-- Replace -->
   <div class="screenshot-placeholder">
       Welcome Screen
   </div>

   <!-- With -->
   <img src="screenshots/01-welcome-screen.png" class="screenshot" alt="Welcome Screen">
   ```
4. **Commit and push** to GitHub
5. **GitHub Pages** will automatically update

### Screenshot Specifications

- **Format:** PNG
- **Max Width:** 1400px
- **Quality:** High (144 DPI preferred)
- **Blur:** Sensitive data (API keys, personal info)

See `SCREENSHOT_GUIDE.md` for detailed instructions.

## Styling

All pages use consistent styling:
- **Colors:**
  - Primary: `#667eea` (purple-blue)
  - Secondary: `#764ba2` (purple)
  - Gradient background on most pages
- **Typography:** System fonts (-apple-system, BlinkMacSystemFont, etc.)
- **Layout:** Centered container, max-width 900px
- **Components:** Cards, tables, info boxes, code blocks

## Development

### Local Testing

```bash
# Serve locally (Python 3)
cd docs
python3 -m http.server 8000

# Then open http://localhost:8000
```

### Updating Content

1. Edit HTML files directly
2. Test locally
3. Commit changes
4. Push to GitHub
5. GitHub Pages auto-deploys in ~1 minute

### Adding New Pages

1. Create new `.html` file in `docs/`
2. Copy header/footer from existing page
3. Add link to `index.html` in docs-grid
4. Commit and push

## GitHub Pages Configuration

**File:** `_config.yml`

```yaml
theme: jekyll-theme-minimal
```

**Settings:**
- **Source:** `main` branch, `/docs` folder
- **Custom domain:** (Optional) Can be configured in GitHub repo settings
- **HTTPS:** Enforced

## Maintenance

### Regular Updates

- Update version numbers when releasing new versions
- Add new features to extension guide
- Update screenshots when UI changes
- Keep changelog current in extension.html

### Broken Links

Check for broken links:
```bash
# Install linkchecker
brew install linkchecker  # macOS
# or
pip install linkchecker

# Check links
linkchecker https://raaihank.github.io/airtable-lead-enricher-docs/
```

## Contributing

To contribute to documentation:

1. Fork the repository
2. Create a feature branch
3. Make your changes to files in `docs/`
4. Test locally
5. Submit a pull request

## Support

For documentation issues:
- Open an issue: https://github.com/raaihank/apify-airtable-lead-enricher/issues
- Tag with `documentation` label

## License

Same as main project license.
