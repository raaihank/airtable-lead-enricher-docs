# Screenshot Guide for Extension Documentation

This guide explains how to capture professional screenshots for the extension documentation.

## Required Screenshots

### 1. Installation Flow (Optional)
**Filename:** `screenshots/00-installation.png`
- Show the browser prompt when running `npm start`
- Include the Airtable base selection screen

### 2. Welcome Screen
**Filename:** `screenshots/01-welcome-screen.png`

**Steps:**
1. Open extension in Airtable
2. Make sure it's showing the welcome screen
3. Capture full extension panel
4. Include:
   - Logo (100x100px)
   - "Lead Enricher" heading
   - Feature list with checkmarks
   - "Get Started" button
   - Footer links (Documentation, Terms, Privacy)

**Window size:** 400px width (standard extension panel)

### 3. Step 1 - API Keys Configuration
**Filename:** `screenshots/02-step1-api-keys.png`

**Steps:**
1. Click "Get Started" button
2. Fill in sample API keys (use placeholder text like `apify_api_XXXXXX`)
3. Show the "Show Keys" toggle in off position
4. Include:
   - Step indicator (1. API Keys highlighted)
   - Security notice with lock icon
   - All API key fields (Apify, OpenAI, Hunter.io)
   - ICP Criteria field
   - "Continue" button

**Important:** Blur or mask actual API keys!

### 4. Step 2 - Field Mapping
**Filename:** `screenshots/03-step2-field-mapping.png`

**Steps:**
1. Click "Continue" from Step 1
2. Click "Auto-fill" button to populate field mappings
3. Show several mapped fields (5-8 fields visible)
4. Include:
   - Step indicator (2. Field Mapping highlighted)
   - INPUT FIELDS section with dropdowns
   - OUTPUT FIELDS section with mappings
   - "Auto-fill" button
   - Field remove buttons (X)
   - "+ Add field mapping..." dropdown
   - "Back" and "Continue" buttons

### 5. Step 3 - Select & Enrich
**Filename:** `screenshots/04-step3-select-enrich.png`

**Steps:**
1. Click "Continue" from Step 2
2. Select a table and view
3. Show filter options
4. Set max records to 10-20
5. Include:
   - Step indicator (3. Select & Enrich highlighted)
   - Table/View dropdowns
   - Filter dropdown
   - Max Records input (10-20)
   - Summary section showing:
     - "X records ready to enrich"
     - "AI scoring enabled/disabled"
     - "Social profiles enabled/disabled"
   - "Enrich X Records" button

### 6. Enrichment in Progress
**Filename:** `screenshots/05-enrichment-progress.png`

**Steps:**
1. Click "Enrich X Records"
2. Wait for enrichment to start
3. Capture during processing (40-60% progress)
4. Include:
   - Progress box header "⏳ Enrichment In Progress"
   - Progress bar showing 40-60%
   - Status text (e.g., "Running")
   - Processed records count
   - Run ID
   - "View in Apify Console →" link
   - "← Enrich More Leads" button (disabled)

**Note:** May need to use developer tools to slow down or pause progress

### 7. Enrichment Complete
**Filename:** `screenshots/06-enrichment-complete.png`

**Steps:**
1. Wait for enrichment to complete
2. Capture completion screen
3. Include:
   - Progress box header "✅ Enrichment Complete"
   - 100% progress bar
   - "Completed" status
   - Records queued count
   - Run ID
   - "View in Apify Console →" link
   - "← Enrich More Leads" button (enabled)

## Screenshot Best Practices

### Preparation
1. **Use a clean Airtable base** with professional company names
   - Example companies: Stripe, Airbnb, Notion, Figma, etc.
   - Avoid sensitive or real customer data

2. **Configure field names** to be clear and professional
   - "Company Name", "Website", "Email", "Lead Score", etc.
   - Avoid test names like "field1", "test", etc.

3. **Use realistic data** for demonstrations
   - Fill in 15-20 sample companies
   - Use well-known companies for credibility

### Capture Settings

**Recommended Tools:**
- **macOS:** Cmd+Shift+4, then press Space to capture window
- **Windows:** Snipping Tool or Windows+Shift+S
- **Browser Extension:** Awesome Screenshot, Nimbus Screenshot

**Settings:**
- **Format:** PNG (not JPEG)
- **Size:** Natural size (don't resize, docs will handle it)
- **Quality:** Maximum quality, no compression
- **DPI:** 144 (Retina) if possible, 72 minimum

### Editing Guidelines

1. **Blur sensitive data:**
   - Full API keys (show only first few characters)
   - Record IDs
   - Personal information

2. **Add annotations (optional):**
   - Red arrows pointing to key features
   - Numbered circles for step-by-step guides
   - Highlight boxes around important elements

3. **Maintain consistency:**
   - Same window size for all screenshots
   - Same theme/appearance
   - Same data set across screenshots

### File Naming Convention

```
screenshots/
├── 00-installation.png          (Optional)
├── 01-welcome-screen.png         (Required)
├── 02-step1-api-keys.png        (Required)
├── 03-step2-field-mapping.png   (Required)
├── 04-step3-select-enrich.png   (Required)
├── 05-enrichment-progress.png   (Required)
└── 06-enrichment-complete.png   (Required)
```

## After Capturing Screenshots

1. **Optimize images** (reduce file size without quality loss):
   ```bash
   # Using imageoptim (macOS)
   imageoptim screenshots/*.png

   # Using pngquant (all platforms)
   pngquant --quality=80-100 screenshots/*.png
   ```

2. **Commit to repository:**
   ```bash
   git add docs/screenshots/*.png
   git commit -m "Add extension screenshots"
   git push
   ```

3. **Update documentation:**
   - Replace placeholder divs in `extension.html`
   - Use `<img src="screenshots/XX-name.png" class="screenshot" alt="Description">`

## Example Screenshot HTML

Replace the placeholder divs in `docs/extension.html` with:

```html
<!-- Welcome Screen -->
<img
  src="screenshots/01-welcome-screen.png"
  alt="Extension Welcome Screen showing features and Get Started button"
  class="screenshot"
>

<!-- Step 1 -->
<img
  src="screenshots/02-step1-api-keys.png"
  alt="Step 1: API Keys configuration with security features"
  class="screenshot"
>

<!-- And so on... -->
```

## Testing

After adding screenshots:

1. Open `docs/extension.html` in browser
2. Verify all images load correctly
3. Check image quality on both normal and retina displays
4. Ensure images are properly sized and aligned
5. Test on mobile devices (responsive design)

## Troubleshooting

**Images too large:**
- Resize to max width 1400px
- Compress with tools like TinyPNG or ImageOptim

**Images blurry:**
- Capture at higher resolution
- Use 144 DPI for retina displays
- Don't upscale, always downscale

**Inconsistent sizes:**
- Use same browser zoom level (100%)
- Capture full extension panel
- Don't crop unevenly

## Questions?

If you need help with screenshots, please open an issue or contact the team.
