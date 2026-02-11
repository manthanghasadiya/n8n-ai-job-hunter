# n8n AI Job Hunter

An automated job search workflow that scrapes LinkedIn, scores job listings against your resume using AI, and delivers personalized matches to Discord daily.

![n8n](https://img.shields.io/badge/n8n-workflow-orange?logo=n8n)
![Gemini](https://img.shields.io/badge/Google-Gemini%202.5-blue?logo=google)
![License](https://img.shields.io/badge/license-MIT-green)

## Overview

Stop manually browsing job boards. This workflow:

1. **Parses your resume** using AI to extract skills, experience, and target roles
2. **Scrapes LinkedIn** for fresh job postings (last 24 hours)
3. **Fetches full job descriptions** for each listing
4. **Scores and ranks jobs** against your profile using Gemini 2.5 Flash
5. **Delivers top 10 matches** to Discord with match scores, skill gaps, and application tips

Runs daily at 9 AM automatically.

## How It Works

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Resume    │────▶│  AI Parse   │────▶│  LinkedIn   │
│    PDF      │     │  (Gemini)   │     │   Scrape    │
└─────────────┘     └─────────────┘     └─────────────┘
                                              │
                                              ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Discord   │◀────│  Score &    │◀────│  Fetch Job  │
│  Delivery   │     │   Rank      │     │   Details   │
└─────────────┘     └─────────────┘     └─────────────┘
```

## Sample Output

```
🎯 Daily Job Match Report
📅 Monday, February 10, 2026
👤 John Doe
━━━━━━━━━━━━━━━━━━━━

✨ Found 10 excellent opportunities
🔥 3 exceptional matches (95%+)

━━━━━━━━━━━━━━━━━━━━
📋 JOB 1 of 10
━━━━━━━━━━━━━━━━━━━━

Senior Security Engineer
🏢 Acme Corp
📍 Remote, US
🔥 Match Score: 97%

✅ Matched Skills:
Python, AWS, Kubernetes, Penetration Testing

📚 Consider adding: Terraform

💡 Why It's a Match:
Strong alignment with your cloud security experience...

🔗 Apply Here:
https://linkedin.com/jobs/view/...
```

## Prerequisites

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- [Google Gemini API key](https://makersuite.google.com/app/apikey) (free tier works)
- [Discord webhook URL](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks)
- Resume hosted as accessible PDF URL

## Installation

### 1. Import Workflow

- Download `job-hunter-workflow.json`
- In n8n, go to **Workflows → Import from File**
- Select the downloaded JSON

### 2. Configure Credentials

**Google Gemini:**
- Go to **Credentials → Add Credential → Google Gemini (PaLM) API**
- Paste your API key

### 3. Update Workflow Variables

Open the workflow and update these nodes:

| Node | Field | Value |
|------|-------|-------|
| `HTTP Request` | URL | Your hosted resume PDF URL |
| `HTTP Request1` | URL | Your Discord webhook URL |

### 4. Customize Search (Optional)

In the `LinkedIn Search URL` node, modify:
- `location` - Default: `United States`
- `f_TPR=r86400` - Time filter (r86400 = last 24 hours)

### 5. Activate

Toggle the workflow to **Active**. It will run daily at 9 AM.

## Configuration Options

### Change Schedule

Edit the `Schedule Trigger` node:
- Default: Daily at 9:00 AM
- Modify `triggerAtHour` for different time
- Add `triggerAtMinute` for specific minutes

### Adjust Scoring Threshold

In `Parse AI Results` node:
- `strongMatches`: 85+ score (default)
- `goodMatches`: 75+ score (default)

### Modify Job Count

- Jobs fetched: 30 (3 batches × 10)
- Jobs delivered: Top 10 after scoring

## Workflow Nodes

| Node | Purpose |
|------|---------|
| Schedule Trigger | Runs workflow daily at 9 AM |
| HTTP Request | Fetches your resume PDF |
| Extract from File | Extracts text from PDF |
| Message a Model | AI parses resume into structured profile |
| LinkedIn Search URL | Generates LinkedIn API URLs |
| Fetch LinkedIn Jobs | Scrapes job listings |
| Parse Jobs | Extracts job data from HTML |
| Fetch Job Description | Gets full JD for each job |
| Extract JD | Parses job descriptions |
| Analyze Jobs with AI | Scores jobs against your profile |
| Parse AI Results | Processes AI scoring output |
| Format Telegram Messages | Formats Discord messages |
| HTTP Request1 | Sends to Discord webhook |

## Rate Limiting

Built-in protections:
- 3-second delay between job description fetches
- 2-second delay between Discord messages
- Batched LinkedIn requests

## Troubleshooting

**No jobs found:**
- Check if LinkedIn HTML structure changed
- Verify search terms in `LinkedIn Search URL` node
- Try running manually to see errors

**AI parsing fails:**
- Ensure Gemini API key is valid
- Check if resume PDF is accessible
- Verify PDF isn't password protected

**Discord not receiving:**
- Test webhook URL directly with curl
- Check n8n execution logs for errors

## Customization Ideas

- Add more job boards (Indeed, Greenhouse, Lever)
- Send to Slack/Telegram instead of Discord
- Store results in Google Sheets
- Add email notifications for 95%+ matches
- Filter by salary range or company size

## Contributing

Contributions welcome! Feel free to:
- Add support for more job boards
- Improve job description parsing
- Add new notification channels
- Enhance AI scoring prompts

## License

MIT License - feel free to use, modify, and distribute.

## Author

**Manthan Ghasadiya**
- LinkedIn: [linkedin.com/in/manthanghasadiya](https://linkedin.com/in/man-ghasadiya)
- GitHub: [github.com/yourusername](https://github.com/manthanghasadiya)

---

⭐ Star this repo if it helped your job search!
