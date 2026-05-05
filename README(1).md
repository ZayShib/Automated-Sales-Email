# 📧 Agentnomics.ai — Automated Email Outreach Script

> AI-powered cold outreach that reads your Google Sheet, writes a personalized email for every business using Llama 3, and sends it through your Gmail — automatically.

---

## What It Does

This Google Apps Script connects your Google Sheet of business leads to Groq's free Llama 3 AI model to generate and send personalized cold outreach emails at scale. Every email is uniquely written based on the business's name, category, location, website, Instagram handle, and any custom notes you've added. Once sent, each row is automatically marked so no one gets emailed twice.

---

## How It Works

1. The script reads each row from your leads sheet
2. It sends the business details to Llama 3 via Groq's API
3. Llama 3 writes a unique subject line and email body for that business
4. The email is sent through your Gmail account
5. The row is marked "Sent" in column K

---

## Requirements

- A Google account with Gmail
- A Google Sheet of business leads (see Sheet Setup below)
- A free Groq API key — get one at [console.groq.com](https://console.groq.com)

---

## Sheet Setup

Your Google Sheet must have the following column headers in this exact order:

| Column | Header | Description |
|--------|--------|-------------|
| A | `name` | Business name |
| B | `website` | Business website URL |
| C | `category` | Business type / industry |
| D | `square_signal` | Platform signal info |
| E | `email` | Email address to send to |
| F | `phone` | Phone number |
| G | `city` | City |
| H | `state` | State |
| I | `instagram` | Instagram handle |
| J | `notes` | Any custom notes for personalization |
| K | `status` | Leave blank — script writes "Sent" here automatically |

> **Important:** The sheet tab name at the bottom must match the `SHEET_NAME` value in the script. Default is set to `square_pos_retailer_leads`.

---

## Installation

1. Open your Google Sheet
2. Click **Extensions → Apps Script**
3. Delete any existing code in the editor
4. Paste the full script into the editor
5. Update the configuration values at the top of the script (see Configuration below)
6. Hit `Ctrl+S` to save
7. Refresh your Google Sheet — the **📧 Outreach** menu will appear in the menu bar

---

## Configuration

At the top of the script, update these four values before running:

```javascript
const SHEET_NAME   = "square_pos_retailer_leads"; // your sheet tab name
const GROQ_API_KEY = "your-groq-api-key-here";    // from console.groq.com
const YOUR_NAME    = "Your Name";                  // your name for email sign-off
const YOUR_COMPANY = "Your Company";               // your company name
```

---

## Running the Script

1. Go to your Google Sheet
2. Click **📧 Outreach** in the top menu bar
3. Click **Send Emails**
4. On first run, a permissions popup will appear — click through and allow Gmail and Sheets access
5. The script will run through every row that doesn't have "Sent" in column K

> **Tip:** Before running on your full list, temporarily replace one email in column E with your own address and run it first. This lets you preview exactly what gets generated before it goes out to real businesses.

---

## Personalization

The AI uses the following fields from each row to write a unique email:

- **Business name** — addressed directly in the email
- **Category** — used to reference their specific industry naturally
- **City & State** — makes the email feel locally aware
- **Website** — gives the AI context about the business
- **Instagram** — mentioned naturally if available (feels very personal)
- **Notes** — any custom context you add here is fed directly into the prompt

The richer your Notes column, the more specific and human each email will be.

---

## Limits & Pricing

| Service | Free Tier Limit |
|---------|----------------|
| Groq API (Llama 3.3 70B) | 14,400 requests/day |
| Gmail (personal) | 100 emails/day |
| Gmail (Google Workspace) | 500 emails/day |

Both services used in this script are **free**. There is no cost to run this.

---

## Security Notes

- **Never share your Groq API key publicly.** If exposed, regenerate it immediately at console.groq.com
- Business data (names, cities, Instagram handles, notes) is sent to Groq's servers to generate emails — avoid putting sensitive personal information in the Notes column
- The script only has permission to **send** Gmail — it cannot read your inbox
- Only share your Google Sheet in **View mode** unless someone truly needs editor access, as editors can view the script and your API key

---

## Troubleshooting

**📧 Outreach menu doesn't appear**
Make sure the script is saved (`Ctrl+S`) and you've refreshed the Sheet. Verify you're editing the script linked to your Sheet via Extensions → Apps Script, not a standalone project.

**`Cannot call SpreadsheetApp.getUi()` error**
Don't run `onOpen` manually from the Apps Script editor. Just refresh your Google Sheet and it fires automatically.

**`Model decommissioned` error**
Groq occasionally retires models. Update the model name in `generateEmail` to Groq's latest — check [console.groq.com/docs](https://console.groq.com/docs) for current model names.

**Emails sending but column K not updating**
Make sure column K exists and has `status` as the header. The script writes to the 11th column (index 10).

---

## Model Used

**Llama 3.3 70B Versatile** via Groq  
`model: "llama-3.3-70b-versatile"`  
Meta's Llama 3.3 70B is highly capable at natural, human-sounding writing — ideal for cold outreach that doesn't read like a template.

---

*Built for Agentnomics.ai — automated with Google Apps Script + Groq + Llama 3*
