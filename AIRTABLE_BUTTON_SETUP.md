# Airtable "Enrich" Button Setup Guide

Add a one-click enrichment button to your Airtable base that enriches leads in real-time.

---

## Overview

This guide shows you how to:
1. Add an "Enrich" button to your Airtable table
2. Create an automation that triggers the actor
3. Test the real-time enrichment flow
4. View enriched data instantly

**Result:** Click "Enrich" → Wait 30-60 seconds → See enriched data appear automatically

---

## Prerequisites

- Airtable account (Free or paid)
- Apify account with this actor installed
- Your Airtable Personal Access Token (PAT)
- Your Apify API token

---

## Step 1: Prepare Your Airtable Table

### Required Fields

Your table needs these minimum fields:

| Field Name | Type | Required |
|------------|------|----------|
| Company Name | Single line text | ✅ Yes |
| Website | URL | ✅ Yes |
| Enrich Button | Button | ✅ Yes (we'll create this) |
| Status | Single select | 🟡 Recommended |

### Output Fields (Optional but Recommended)

Add these fields to receive enriched data:

| Field Name | Type | Example |
|------------|------|---------|
| Email | Email | contact@company.com |
| Phone | Phone number | +1 555-0100 |
| Address | Long text | 123 Main St, San Francisco |
| Category | Single line text | Software Company |
| Rating | Number | 4.5 |
| Review Count | Number | 127 |
| Description | Long text | AI-powered platform for... |
| Tech Stack | Long text | React, Node.js, AWS |
| Industry | Single select | SaaS, Fintech, AI/ML, etc. |
| Founding Year | Number | 2018 |
| Employee Count | Single line text | 50-100, 500+, 25000+ |
| Company Stage | Single select | Startup, Growth, Enterprise, Public |
| LinkedIn URL | URL | https://linkedin.com/company/... |
| Lead Score | Number (0-100) | 85 |
| ICP Score | Number (0-40) | 32 |
| Summary | Long text | AI-generated company summary |
| Target Customers | Long text | Who they serve |
| Value Proposition | Long text | Core value they provide |
| Key Products | Long text | Comma-separated products |
| Outreach Angles | Long text | Personalized talking points |
| Enriched At | Date & Time | 2025-12-19 11:30 PM |

---

## Step 2: Add the "Enrich" Button Field

1. Click **+** to add a new field
2. Choose **Button** field type
3. Name it: `Enrich` or `Enrich Lead`
4. Configure button:
   - **Label**: "Enrich Now" or "🔄 Enrich"
   - **Action**: "Run automation" (we'll create this next)
   - **Color**: Blue or Purple

---

## Step 3: Create the Automation

### 3.1 Create New Automation

1. Click **Automations** (top right in Airtable)
2. Click **Create automation**
3. Name it: "Real-Time Lead Enrichment"

### 3.2 Configure Trigger

1. **When**: Choose "When button clicked"
2. **Button**: Select your "Enrich" button field
3. **Table**: Select your leads table

### 3.3 Add Action - Update Status (Optional)

This sets status to "Enriching..." while processing:

1. Click **+ Add action**
2. Choose "Update record"
3. Configure:
   - **Record ID**: Use record ID from trigger
   - **Status** → Set to "Enriching..." (create this option if needed)

### 3.4 Add Action - Send Webhook to Apify

1. Click **+ Add action**
2. Choose "Send request"
3. Configure webhook:

**URL:**
```
https://api.apify.com/v2/acts/YOUR_USERNAME~airtable-lead-enricher/runs
```
Replace `YOUR_USERNAME` with your Apify username.

**Method:** `POST`

**Headers:**
```json
{
  "Content-Type": "application/json",
  "Authorization": "Bearer YOUR_APIFY_TOKEN"
}
```
Replace `YOUR_APIFY_TOKEN` with your Apify API token from https://console.apify.com/account/integrations

**Body:**
```json
{
  "mode": "single",
  "recordIds": ["{{RECORD_ID}}"],
  "airtable": {
    "apiKey": "YOUR_AIRTABLE_PAT",
    "baseId": "YOUR_BASE_ID",
    "tableId": "YOUR_TABLE_ID",
    "inputFields": {
      "companyName": "Company Name",
      "website": "Website"
    },
    "outputFields": {
      "email": "Email",
      "phone": "Phone",
      "address": "Address",
      "category": "Category",
      "rating": "Rating",
      "reviewCount": "Review Count",
      "description": "Description",
      "techStack": "Tech Stack",
      "industry": "Industry",
      "foundingYear": "Founding Year",
      "employeeCount": "Employee Count",
      "companyStage": "Company Stage",
      "linkedinUrl": "LinkedIn URL",
      "leadScore": "Lead Score",
      "icpScore": "ICP Score",
      "summary": "Summary",
      "targetCustomers": "Target Customers",
      "valueProposition": "Value Proposition",
      "keyProducts": "Key Products",
      "outreachAngles": "Outreach Angles",
      "enrichedAt": "Enriched At"
    },
    "updateMode": "append"
  },
  "llm": {
    "enabled": true,
    "provider": "openai",
    "apiKey": "YOUR_OPENAI_KEY"
  },
  "scoring": {
    "enabled": true,
    "icpCriteria": "B2B SaaS companies, 50-500 employees, uses modern tech stack"
  }
}
```

**Important:** In the Body, replace `{{RECORD_ID}}` with the dynamic record ID from the trigger step (Airtable will show you the option to insert it).

---

## Step 4: Get Your IDs

### Airtable Personal Access Token (PAT)

1. Go to https://airtable.com/create/tokens
2. Click "Create new token"
3. Name: "Lead Enricher"
4. Scopes needed:
   - `data.records:read`
   - `data.records:write`
5. Access: Select your base
6. Copy the token (starts with `pat...`)

### Base ID

From your Airtable URL: `https://airtable.com/appXXXXXXXXXXXXXX/...`
The Base ID is `appXXXXXXXXXXXXXX`

### Table ID

1. Click the **...** menu on your table
2. Select "Copy table ID"
3. It starts with `tbl...`

### Apify Token

1. Go to https://console.apify.com/account/integrations
2. Copy your API token

---

## Step 5: Test the Automation

1. **Turn on the automation** (toggle in top right)
2. **Open a record** with Company Name and Website filled
3. **Click the "Enrich" button**
4. **Watch the status** change to "Enriching..."
5. **Wait 30-60 seconds**
6. **Refresh the record** to see enriched data

### What Happens Behind the Scenes

```
Click Button
    ↓
Airtable Automation Triggered
    ↓
Webhook Sent to Apify
    ↓
Actor Runs (30-60 seconds)
    ├── Crawls Website
    ├── Searches Google Maps
    ├── Calls Hunter.io (if enabled)
    └── AI Scoring & Enrichment
    ↓
Data Written Back to Airtable
    ↓
Record Automatically Updated
```

---

## Step 6: Advanced Configuration

### Add Status Tracking

Create a "Status" single-select field with options:
- 🔵 Ready to Enrich
- 🟡 Enriching...
- 🟢 Enriched
- 🔴 Failed

Update your automation to set status at different stages.

### Add Error Handling

1. After webhook action, add "Conditional" action
2. Check if webhook failed (status code ≠ 200)
3. If failed, update Status to "Failed"

### Add Success Confirmation

1. Add delay of 60 seconds after webhook
2. Add "Update record" action
3. Set Status to "Enriched"
4. Add comment: "Enrichment completed at {timestamp}"

---

## Troubleshooting

### Button Does Nothing

✅ Check automation is turned ON
✅ Check webhook URL is correct
✅ Check Apify token is valid

### "Invalid API Key" Error

✅ PAT must have `data.records:read` and `data.records:write` scopes
✅ PAT must have access to your base
✅ Use PAT (starts with `pat`), not old API key

### Data Not Appearing

✅ Check field names match exactly (case-sensitive!)
✅ Refresh the record after 60 seconds
✅ Check Apify run logs: https://console.apify.com/actors/runs

### Webhook Timeout

✅ Actor runs take 30-60 seconds - this is normal
✅ Airtable webhook will timeout, but actor continues running
✅ Data will still be written back when complete

---

## Example Configurations

### Minimal (No AI, Just Contact Info)

```json
{
  "mode": "single",
  "recordIds": ["{{RECORD_ID}}"],
  "airtable": {
    "apiKey": "patXXX",
    "baseId": "appXXX",
    "tableId": "tblXXX",
    "inputFields": {
      "companyName": "Company Name",
      "website": "Website"
    },
    "outputFields": {
      "email": "Email",
      "phone": "Phone",
      "address": "Address",
      "rating": "Rating"
    }
  }
}
```

### With AI Scoring (Recommended)

```json
{
  "mode": "single",
  "recordIds": ["{{RECORD_ID}}"],
  "airtable": {
    "apiKey": "patXXX",
    "baseId": "appXXX",
    "tableId": "tblXXX",
    "inputFields": {
      "companyName": "Company Name",
      "website": "Website"
    },
    "outputFields": {
      "email": "Email",
      "phone": "Phone",
      "leadScore": "Lead Score",
      "outreachAngles": "Outreach Angles"
    }
  },
  "llm": {
    "enabled": true,
    "provider": "openai",
    "apiKey": "sk-proj-XXX"
  },
  "scoring": {
    "icpCriteria": "B2B SaaS, 50-500 employees, enterprise customers"
  }
}
```

---

## Video Demo

Coming soon: Watch a 2-minute video showing the complete setup and enrichment flow.

---

## Cost per Click

Each button click costs:
- **Apify compute**: $0.002-0.005 (30-60 seconds runtime)
- **OpenAI API** (if using AI): $0.001 per lead
- **Total**: ~$0.003-0.006 per enrichment

Compare to:
- Clearbit: $0.36 per lead
- ZoomInfo: $0.50+ per lead
- Apollo: $0.15 per lead

---

## Next Steps

1. ✅ Set up the button (Steps 1-5)
2. 📹 Record a demo for your team
3. 📊 Create a dashboard view showing enrichment status
4. 🔄 Set up batch enrichment for older records
5. 📈 Track ROI: leads enriched vs closed deals

---

## Support

Having trouble? Check:
1. Email: contact@datahq.pro
2. [Full Documentation](https://docs.airtable-lead-enricher.datahq.pro)
3. [Troubleshooting Guide](README.md#troubleshooting)
4. [Report Issue](https://github.com/raaihank/apify-airtable-lead-enricher/issues)

---

*Last updated: December 19, 2025*
