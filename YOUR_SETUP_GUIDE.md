# Your Airtable Setup Guide

Custom configuration for your specific Airtable fields.

---

## Your Field Mapping

### ✅ Fields We Auto-Fill

| Your Airtable Field | Our Data Source | Example |
|---------------------|-----------------|---------|
| **Email** | Website / Hunter.io | contact@harvey.ai |
| **Phone** | Google Maps / Website | +1 415-555-0100 |
| **address** | Google Maps | 405 Howard St, San Francisco |
| **category** | Google Maps | Software Company |
| **Rating** | Google Maps | 4.8 |
| **reviewCount** | Google Maps | 127 |
| **description** | Website | AI-powered legal assistant... |
| **techStack** | Website | React, Node.js, AWS, OpenAI |
| **Industry** | Website + AI | AI/ML |
| **Team Size** | Website | 25000+ |
| **LinkedIn** | Hunter.io / Website | https://linkedin.com/company/harvey |
| **Facebook** | Hunter.io / Website | https://facebook.com/harvey |
| **Twitter** | Hunter.io / Website | @harvey_ai |
| **Business Hours** | Google Maps | Monday: 9AM-6PM... |
| **leadScore** | AI | 85 (0-100) |
| **icpScore** | AI | 38 (0-40) |
| **icpReasoning** | AI | Strong match: Enterprise AI... |
| **summary** | AI | Harvey is an AI-powered legal... |
| **Outreach Angle** | AI | Personalized talking points |
| **dataConfidence** | System | 0.85 (0-1) |
| **enrichedAt** | System | 2025-12-19 |
| **enrichmentSources** | System | google_maps, website, hunter, llm |

### 🟡 Fields We Don't Generate (Yet)

| Your Field | Why Not Available | Workaround |
|------------|-------------------|------------|
| **Est. Monthly LLM Spend** | Not publicly available | Add formula based on team size + industry |
| **LLM Providers Used** | Not explicitly tracked | Check `techStack` field for OpenAI, Anthropic, etc. |
| **Pain Point** | Not currently generated | Use AI summary to infer, or add manual research |
| **Contact Email** | Duplicate of Email field | Map both to same `email` output |
| **Priority** | Business logic field | Add formula: HIGH if leadScore > 80, MEDIUM if > 50, else LOW |
| **HQ Location** | Input only | We use this for Google Maps search, but don't overwrite it |

---

## Complete Configuration

### Step 1: Replace Placeholders

Open [MY_AIRTABLE_CONFIG.json](MY_AIRTABLE_CONFIG.json) and replace:

1. `YOUR_AIRTABLE_PAT_HERE` → Your Personal Access Token from https://airtable.com/create/tokens
2. `YOUR_BASE_ID_HERE` → From URL: `https://airtable.com/appXXXXX/...` (the `appXXXXX` part)
3. `YOUR_TABLE_ID_HERE` → Click ... menu on table → "Copy table ID"
4. `YOUR_HUNTER_API_KEY_HERE` → From https://hunter.io/api (optional but recommended)
5. `YOUR_OPENAI_API_KEY_HERE` → From https://platform.openai.com/api-keys

### Step 2: Configure Your ICP

Update the `icpCriteria` in the config:

```json
"scoring": {
  "icpCriteria": "YOUR ICP DESCRIPTION HERE"
}
```

Example for targeting AI companies:
```json
"icpCriteria": "B2B SaaS companies with 50-1000 employees, actively using LLM providers (OpenAI, Anthropic, Google), building AI-powered products, enterprise customers, raised Series A+ funding"
```

### Step 3: Airtable Button Webhook Body

For the button automation, use this exact body:

```json
{
  "mode": "single",
  "recordIds": ["{{RECORD_ID_FROM_TRIGGER}}"],
  "airtable": {
    "apiKey": "YOUR_AIRTABLE_PAT",
    "baseId": "YOUR_BASE_ID",
    "tableId": "YOUR_TABLE_ID",
    "inputFields": {
      "companyName": "Company Name",
      "website": "Website",
      "location": "HQ Location"
    },
    "outputFields": {
      "email": "Email",
      "phone": "Phone",
      "address": "address",
      "category": "category",
      "rating": "Rating",
      "reviewCount": "reviewCount",
      "description": "description",
      "techStack": "techStack",
      "industry": "Industry",
      "employeeCount": "Team Size",
      "linkedinUrl": "LinkedIn",
      "facebookUrl": "Facebook",
      "twitterHandle": "Twitter",
      "businessHours": "Business Hours",
      "leadScore": "leadScore",
      "icpScore": "icpScore",
      "icpReasoning": "icpReasoning",
      "summary": "summary",
      "outreachAngles": "Outreach Angle",
      "dataConfidence": "dataConfidence",
      "enrichedAt": "enrichedAt",
      "enrichmentSources": "enrichmentSources"
    }
  },
  "enrichment": {
    "sources": ["google_maps", "website", "hunter"]
  },
  "llm": {
    "enabled": true,
    "provider": "openai",
    "apiKey": "YOUR_OPENAI_KEY"
  },
  "scoring": {
    "icpCriteria": "B2B SaaS companies, 50-500 employees, AI/LLM integration needs"
  }
}
```

**Important:** Replace `{{RECORD_ID_FROM_TRIGGER}}` with the dynamic record ID that Airtable provides in the automation builder.

---

## Handling Missing Fields

### Option 1: Add Formulas

For fields we don't generate, add Airtable formulas:

**Priority Formula:**
```
IF({leadScore} > 80, "High", IF({leadScore} > 50, "Medium", "Low"))
```

**Est. Monthly LLM Spend Formula:**
```
IF(
  FIND("AI", {Industry}),
  IF(
    OR(FIND("25000", {Team Size}), FIND("10000", {Team Size})),
    "$50,000+",
    IF(FIND("500", {Team Size}), "$10,000-$50,000", "$1,000-$10,000")
  ),
  "N/A"
)
```

**LLM Providers Used (from techStack):**
```
CONCATENATE(
  IF(FIND("OpenAI", {techStack}), "OpenAI, ", ""),
  IF(FIND("Anthropic", {techStack}), "Anthropic, ", ""),
  IF(FIND("Google", {techStack}), "Google, ", ""),
  IF(FIND("Cohere", {techStack}), "Cohere, ", "")
)
```

### Option 2: Custom Field Mapping

If you want different field names, update the `outputFields` mapping:

```json
"outputFields": {
  "email": "Contact Email",  // Use "Contact Email" instead of "Email"
  "employeeCount": "Company Size",  // Use "Company Size" instead of "Team Size"
  ...
}
```

---

## Field Format Notes

### ⚠️ Important Format Requirements

1. **enrichedAt**: Your field format is `yyyy-mm-dd hh:mm`
   - Our output format: ISO 8601 `2025-12-19T23:30:00.000Z`
   - Airtable will auto-convert this ✅

2. **leadScore**: Your format is Integer
   - Our output: Integer 0-100 ✅

3. **icpScore**: Your format is Decimal
   - Our output: Integer 0-40
   - Will display as `38.00` in Airtable ✅

4. **Rating**: Your format is Decimal
   - Our output: Decimal 0-5 (e.g., 4.7) ✅

5. **reviewCount**: Your format is Integer
   - Our output: Integer ✅

6. **Outreach Angle**: Single long text field
   - Our output: Array of 3-4 angles
   - Will be joined with newlines:
   ```
   - Reference their 25,000+ employee scale and LLM cost optimization needs
   - Lead with multi-provider AI usage (OpenAI, Anthropic, Google) as pain point
   - Mention compliance-ready infrastructure needs for legal tech market
   - Ask about intelligent routing challenges across different LLM providers
   ```

---

## Example Enrichment Result

### Input (What you have):
```
Company Name: Harvey AI
Website: https://harvey.ai
HQ Location: San Francisco, CA
```

### Output (What you'll get):

```
Email: contact@harvey.ai
Phone: +1 415-555-0100
address: 405 Howard St, San Francisco, CA
category: Software Company
Rating: 4.8
reviewCount: 127
description: Harvey is an AI-powered legal assistant platform...
techStack: React, Node.js, AWS, OpenAI API, Anthropic Claude
Industry: AI/ML
Team Size: 25000+
LinkedIn: https://linkedin.com/company/harvey
Facebook: https://facebook.com/harveyai
Twitter: @harvey_ai
Business Hours: Monday: 9:00 AM - 6:00 PM...
leadScore: 85
icpScore: 38
icpReasoning: Strong ICP match: Enterprise AI company with 25000+ employees, actively using multiple LLM providers, serving legal tech market
summary: Harvey is an AI-powered legal assistant platform that helps law firms and legal departments automate document review, research, and analysis. The company uses multiple LLM providers including OpenAI and Anthropic to deliver accurate legal insights.
Outreach Angle:
  - Reference their 25,000+ employee scale and LLM cost optimization needs
  - Lead with multi-provider AI usage (OpenAI, Anthropic, Google) as pain point
  - Mention compliance-ready infrastructure needs for legal tech market
  - Ask about intelligent routing challenges across different LLM providers
dataConfidence: 0.85
enrichedAt: 2025-12-19T23:30:00.000Z
enrichmentSources: google_maps, website, hunter, llm
```

---

## Testing Your Setup

### Quick Test (Single Record)

1. Open your Harvey AI record
2. Make sure `Company Name` and `Website` are filled
3. Click the "Enrich" button
4. Wait 60 seconds
5. Refresh the record
6. Check all fields populated ✅

### Batch Test (Multiple Records)

Add these test companies:

| Company Name | Website |
|--------------|---------|
| Anthropic | https://anthropic.com |
| OpenAI | https://openai.com |
| Cohere | https://cohere.com |

Run enrichment on all three to compare results.

---

## Troubleshooting

### Field Names Don't Match

✅ Field names are **case-sensitive**
- `"Email"` ≠ `"email"`
- Check exact spelling in your config

### Data Not Appearing

✅ Check field types match:
- Number fields need Number type in Airtable
- Date fields need Date type
- Long text fields for arrays (outreachAngles)

### Hunter.io Social Links Not Found

✅ Hunter.io required for LinkedIn/Facebook/Twitter
- Add Hunter API key to config
- Free tier: 25 searches/month
- Paid: $49/month for 1,000 searches

### Outreach Angles Empty

✅ Requires both:
- LLM enabled (OpenAI key)
- ICP criteria defined in config

---

## Cost Breakdown

Per enrichment:
- **Apify compute**: $0.002-0.005
- **OpenAI (gpt-4o-mini)**: $0.001
- **Hunter.io**: $0.049 (if using)
- **Total without Hunter**: ~$0.003
- **Total with Hunter**: ~$0.052

For 100 leads:
- Without Hunter: ~$0.30
- With Hunter: ~$5.20

---

## Next Steps

1. ✅ Test with Harvey AI first
2. ✅ Verify all fields populate correctly
3. ✅ Add formulas for Priority and Est. LLM Spend
4. ✅ Set up batch enrichment for historical records
5. ✅ Train team on using outreach angles

---

Need help? Check [AIRTABLE_BUTTON_SETUP.md](AIRTABLE_BUTTON_SETUP.md) for detailed setup instructions.
