# 🔍 LinkedIn BI Jobs H1B Scraper — Complete Setup Guide

> **Automated daily LinkedIn job search for Business Intelligence roles with H1B visa sponsorship filtering.**
> Built with Claude.ai + Apify. Setup takes ~10 minutes. Each daily run takes 2–3 minutes.

---

## Table of Contents

1. [What This Pipeline Does](#1-what-this-pipeline-does)
2. [Tools You'll Need](#2-tools-youll-need)
3. [Connect Apify to Claude](#3-connect-apify-to-claude)
4. [The Exact Prompt to Run Every Day](#4-the-exact-prompt-to-run-every-day)
5. [How the H1B Filter Works](#5-how-the-h1b-filter-works)
6. [Understanding Your Output Table](#6-understanding-your-output-table)
7. [Tips to Maximize Results](#7-tips-to-maximize-results)
8. [Customizing for Other Job Searches](#8-customizing-for-other-job-searches)
9. [Troubleshooting Common Issues](#9-troubleshooting-common-issues)
10. [Quick Reference Card](#10-quick-reference-card)

---

## 1. What This Pipeline Does

Every day, hundreds of Business Intelligence jobs are posted on LinkedIn. This pipeline scrapes all of them posted in the last 24 hours, filters for H1B-friendly companies, and delivers a searchable table — so you only spend time on jobs that can actually sponsor you.

**The pipeline runs in 4 steps:**

**Step 1 — Scrape LinkedIn**
Uses Apify's LinkedIn Job Scraper to pull all full-time Business Intelligence jobs posted in the US within the last 24 hours. Typically returns 300–400 jobs per run.

**Step 2 — Keyword scan for explicit H1B mentions**
Reads every job description and flags postings that mention: H1B, H-1B, visa sponsorship, work authorization, OPT, EAD, green card, immigration support, LCA, labor condition, visa transfer, portability, F-1, STEM OPT, or employment eligibility.

**Step 3 — Cross-reference known H1B sponsors**
Matches company names against a curated list of 100+ companies with documented USCIS H1B petition history — including Amazon, Microsoft, Google, Deloitte, PwC, Wells Fargo, Capital One, TCS, Infosys, and more.

**Step 4 — Deliver interactive table**
Outputs a filterable, sortable HTML table with 9 columns: company, role, location, salary, applicants, posted time, visa signal tag, recruiter/hiring manager search links, and direct apply link.

---

## 2. Tools You'll Need

### Claude.ai (by Anthropic)
The AI that orchestrates everything — runs the scraper, processes the data, applies the H1B filter, and builds the output table.

- **Sign up:** https://claude.ai
- Free plan works. Pro plan ($20/month) gives higher message limits.

### Apify
A cloud scraping platform that runs the LinkedIn job scraper. Claude connects to it automatically via MCP (Model Context Protocol).

- **Sign up:** https://apify.com
- Free tier includes enough monthly credits for daily runs (~$5/month in credits, enough for 100+ runs).

> **Cost note:** The LinkedIn scraper used in this guide (`cheap_scraper/linkedin-job-scraper`) costs approximately $0.01–$0.05 per run. Apify's free plan covers daily usage comfortably.

---

## 3. Connect Apify to Claude

> **Do this once — it takes about 3 minutes.** This connection allows Claude to automatically run scrapers on your behalf. Once set up, you never need to touch Apify manually again.

### Step-by-step:

**1. Create your Apify account**
Go to https://apify.com → click Sign Up → use Google or email → confirm your email.

**2. Get your Apify API token**
In Apify, go to:
`Settings → Integrations → API tokens → Create token`
Name it "Claude" → copy the token. Keep this safe like a password.

**3. Open Claude.ai settings**
Go to https://claude.ai → click your profile icon (top right) → Settings → Integrations (or Connectors).

**4. Add Apify as an integration**
Click Add integration or Browse connectors → search for Apify → click Connect → paste your API token when prompted → click Save.

**5. Verify the connection**
Start a new Claude chat and type: *"List my Apify actors"*
If Claude responds with Apify tools, you're connected. If not, retry the token step.

> **Important:** Always start a **fresh Claude conversation** each time you run the pipeline. Claude's tool availability can drift in long conversations. The whole pipeline completes in 2–3 minutes from a fresh chat.

---

## 4. The Exact Prompt to Run Every Day

Copy the prompt below exactly and paste it into a **new** Claude.ai conversation. Claude will handle everything automatically — scraping, filtering, and building the table — in about 2–3 minutes.

---

```
Use Apify to scrape LinkedIn Jobs for Business Intelligence roles that are full-time and posted within the last 24 hours in the United States.

After scraping, apply an H1B visa filter and return only jobs that meet ONE OR BOTH of these criteria:

CRITERION 1 — EXPLICIT MENTION: The job description explicitly mentions any of:
- H1B, H-1B, H1-B, H 1B
- visa sponsorship, will sponsor, sponsorship
- work authorization, work auth
- OPT, STEM OPT, EAD, F-1, F1 student
- immigration support, immigration assistance
- green card, labor condition, LCA
- visa transfer, visa portability
- authorized to work, employment eligibility

CRITERION 2 — KNOWN H1B SPONSOR: The company has documented H1B petition history with USCIS. This includes but is not limited to: Amazon, Google, Microsoft, Meta, Apple, Salesforce, IBM, Oracle, Intel, Walmart, Deloitte, PwC, KPMG, EY, Accenture, Cognizant, Infosys, TCS, Wipro, Capgemini, HCLTech, JPMorgan, Wells Fargo, Goldman Sachs, Capital One, Citi, Morgan Stanley, Bank of America, Netflix, Airbnb, Uber, DoorDash, Stripe, Snowflake, Databricks, Palantir, NVIDIA, Qualcomm, Adobe, ServiceNow, Workday, Medtronic, Johnson & Johnson, Pfizer, Sanofi, Bristol Myers Squibb, Booz Allen Hamilton, Northrop Grumman, Leidos, T-Mobile, AT&T, Comcast, Target, Walmart, and similar large enterprises.

For each qualifying job, return a structured interactive HTML table with these 9 columns:
1. Company name
2. Role title
3. Location
4. Salary range (show "Not listed" if absent)
5. Number of applicants
6. Date/time posted
7. Visa signal tag: 🟣 Both (explicit + known sponsor) | 🟢 Explicit mention | 🔵 Known sponsor
8. Poster/Contacts: show "Not disclosed by LinkedIn" for poster name, then two clickable links: "🔍 Find Recruiter" and "👤 Find Hiring Mgr" (pre-built LinkedIn People Search URLs for that company)
9. Direct Apply link

Make the table interactive with:
- Search bar (filter by company, title, or location)
- Dropdown to filter by visa signal type
- Dropdown to filter by salary availability
- Dropdown to filter by applicant volume (low competition vs high demand)
- Sortable columns
- Pagination (20-25 rows per page)

Save and present the result as a downloadable HTML file.
```

---

> **Pro tip:** Add *"Focus on remote jobs and jobs in [your city, state]"* at the end of the prompt to prioritize roles in your area. For example: *"Focus on remote jobs and jobs in San Diego, CA."*

---

## 5. How the H1B Filter Works

Every job goes through two independent checks. A job only needs to pass **one** to be included. Jobs that pass both get the highest-priority 🟣 Both tag.

### Visa Signal Tags

| Tag | What it means | Confidence | Action |
|-----|--------------|------------|--------|
| 🟣 **Both** | Job posting explicitly mentions H1B/visa AND company has documented USCIS H1B filing history | Very High | Apply immediately. Best bets. Find the recruiter and reach out directly on LinkedIn. |
| 🟢 **Explicit** | Job description directly states visa sponsorship, H1B, work authorization assistance, or similar keywords | High | Strong signal. Read the full job description carefully — some say "will sponsor" while others say "not able to sponsor." |
| 🔵 **Known Sponsor** | Company has filed H1B petitions with USCIS historically, even if this specific posting doesn't mention it | Medium-High | Apply and ask about sponsorship during the recruiter screen. Most large companies with H1B history continue to sponsor. |

### H1B Keywords the filter scans for

```
h1b · h-1b · h 1b · h1-b · visa sponsor · will sponsor · sponsorship · immigration ·
work authorization · work auth · opt · ead · green card · visa transfer · visa support ·
visa portability · visa filing · lca · labor condition · international candidate · f-1 ·
f1 student · stem opt · authorized to work · employment eligibility · sponsoring · sponsor visa
```

> **Watch for negative signals:** The filter automatically excludes postings that say "not able to sponsor," "unable to sponsor," "cannot sponsor," or "we do not provide visa sponsorship." However, always read the full job description before applying.

---

## 6. Understanding Your Output Table

### 🏢 Company + Location
The hiring company name and city/state. Many companies post the same role across multiple locations — each appears as a separate row so you can target your preferred city.

### 💼 Role Title
The exact job title from LinkedIn. Click the Apply link to read the full description. Job titles vary widely — "BI Developer," "Analytics Engineer," "Data Intelligence Specialist" can all be the same type of role.

### 💰 Salary Range
Pulled directly from the LinkedIn posting. Many companies don't list salary — those show "Not listed." Use the "With salary only" filter to focus on transparent postings. Ranges are annual unless shown as /hr.

### 👥 Applicants
LinkedIn shows approximate counts:
- **"Be among the first 25 applicants"** → Low competition 🟢 — apply immediately
- **"Over 200 applicants"** → High demand 🔴 — still worth applying but move fast
- **Blank** → Unknown

Being early dramatically improves your chances. Apply to low-competition roles first.

### 🕐 Posted Time
How long ago the job was posted — e.g., "2 hours ago," "15 hours ago," "1 day ago." Sort by this column (ascending) to see the freshest postings first.

### 🏷️ Visa Signal
The H1B confidence tag: 🟣 Both, 🟢 Explicit, or 🔵 Known Sponsor. Use the visa signal dropdown to filter to only "Both" for highest-confidence opportunities.

### 👤 Poster / Contacts
LinkedIn does not expose recruiter names via scraping (0% disclosure rate across all postings). Instead you get two pre-built LinkedIn search links:
- **🔍 Find Recruiter** — searches for "recruiter [Company Name]" on LinkedIn People Search
- **👤 Find Hiring Mgr** — searches for "hiring manager [Company Name]" on LinkedIn People Search

Click these to find the right person and message them directly.

### 📨 Apply Link
Opens the job directly on LinkedIn. Always apply through the official LinkedIn posting — it tracks your application status and lets you see if the recruiter viewed your profile.

---

## 7. Tips to Maximize Results

### 🏆 The "Both + Low Competition" combo is your golden target
Filter to 🟣 **Both** visa signal AND **"Low competition (<25 applicants)."** These are jobs from proven H1B sponsors with almost no competition. Apply to every single one of these immediately.

### ⏰ Timing matters — run it daily at the same time
LinkedIn's "last 24 hours" filter is rolling. If you run at 9am every morning, you capture all jobs posted the previous day. Running later means some jobs already have 50–100+ more applicants than they did earlier.

### 📧 Message the recruiter before applying
For 🟣 Both-tagged companies at big firms (Amazon, Microsoft, Capital One, etc.):
1. Click "🔍 Find Recruiter"
2. Filter by 1st/2nd degree connections
3. Send a short personalized note **before** submitting the application
4. Mention the specific role title and that you're interested in discussing sponsorship

This gets your application flagged as a warm lead.

### 🔍 Use the search bar to target your skills
Type your core skills in the search bar — "Snowflake," "dbt," "Power BI," "Tableau," "PySpark" — to instantly filter to roles that match your stack. The search scans company name, title, and location simultaneously.

### 💡 Sort salary descending for highest-paying first
Click the Salary column header twice to sort descending (highest salary first). Combine with "With salary only" filter to see only transparent, high-paying roles at the top.

### 📁 Save the HTML file for your records
The output is a self-contained HTML file. Open it in any browser — Chrome, Firefox, Safari. Save dated copies (e.g., `bi_h1b_jobs_aug12.html`) to track which jobs you've applied to over time.

---

## 8. Customizing for Other Job Searches

The pipeline works for any job type. Swap out the keyword in the prompt to adapt it.

### Different Job Types

| What you want | Replace "Business Intelligence" with |
|---|---|
| Data Engineering | `Data Engineer OR Analytics Engineer` |
| Power BI / Tableau | `Power BI Developer OR Tableau Developer` |
| Snowflake / dbt | `Snowflake Data Engineer OR dbt Analytics Engineer` |
| People Analytics | `People Analytics OR Workforce Analytics` |
| Data Architect | `Data Architect OR Data Platform Architect` |
| All analytics roles | `Business Intelligence OR Data Engineer OR Analytics Engineer` |

### Change the Time Window
Replace "last 24 hours" with "last 3 days" or "last week" to cast a wider net. Useful for less common roles or when you missed a day.

### Add Location Filters
Add "remote only" or "in [City, State]" to narrow geographically.
Example: *"full-time, remote or in New York, NY."*

### Run Multiple Searches in One Session
You can run the scraper multiple times in one conversation with different keywords. Run "Business Intelligence," then "Data Engineer," then "Analytics Engineer" — Claude will combine and deduplicate them if you ask.

---

## 9. Troubleshooting Common Issues

### ❌ "Apify tool not found" error
**Cause:** Claude's tool context cycled out Apify mid-conversation.
**Fix:** Start a brand new Claude conversation. Apify loads fresh at the start of every chat. This is the most common issue and is always resolved by starting fresh.

### ❌ Widget is loading forever (more than 5 minutes)
**Cause:** The live progress widget can stall if LinkedIn rate-limits the scraper.
**Fix:** Tell Claude: *"The widget is stuck — please check the run status and fetch whatever has been collected so far."* Claude will pull partial results immediately.

### ❌ Only a few jobs returned (under 50)
**Cause:** LinkedIn may have temporarily rate-limited the scraper, or the keyword returned fewer results than expected.
**Fix:** Wait 30 minutes and rerun. Or broaden the keyword — replace "Business Intelligence" with "BI OR Business Intelligence OR Analytics."

### ❌ "Tool result too large" error
**Cause:** The dataset is large (400+ jobs) and Claude is trying to load all descriptions at once.
**Fix:** Claude automatically handles this by fetching in batches. If it gets stuck, tell Claude: *"Please fetch in batches of 60 jobs and process each batch separately."*

### ❌ The HTML file doesn't open correctly
**Cause:** Some browsers block local HTML files with JavaScript.
**Fix:** Right-click the file → Open With → Google Chrome or Firefox. Avoid opening in Safari on some macOS versions. Alternatively, drag the file directly into a browser window.

### ❌ Recruiter search links open an empty LinkedIn page
**Cause:** You need to be logged into LinkedIn for People Search to work.
**Fix:** Log into LinkedIn first, then click the recruiter/hiring manager links. They will populate with real profiles.

### ❌ If all else fails
Close the Claude tab entirely, open a fresh tab, go to claude.ai, start a new conversation, and paste the full prompt from Section 4. The pipeline works correctly in every fresh conversation.

---

## 10. Quick Reference Card

### Daily Checklist
```
□ Open claude.ai → Start a NEW conversation
□ Paste the Section 4 prompt → Hit Enter
□ Wait 2–3 minutes for scrape + filter + build
□ Download the .html file
□ Filter to 🟣 Both + Low competition (<25 applicants)
□ Apply immediately to best matches
□ Message recruiters via 🔍 Find Recruiter links
```

### Typical Results Per Run
| Metric | Count |
|--------|-------|
| Total scraped | 300–400 jobs |
| After H1B filter | 150–230 jobs |
| 🟣 Both signal | 30–60 jobs |
| 🟣 Both + Low competition | 8–20 jobs ← **Apply to ALL of these** |

### Key Companies Always in Results
These companies consistently appear and are documented H1B sponsors:

**Big Tech:** Amazon AWS · Google · Microsoft · Salesforce · Netflix · Meta · Apple · Adobe · Snowflake · Databricks

**Finance:** Wells Fargo · JPMorgan · Capital One · Goldman Sachs · Citi · Deloitte · PwC · EY · KPMG

**Consulting & Staffing:** Accenture · TCS · Infosys · HCLTech · Capgemini · Wipro · Booz Allen Hamilton · Guidehouse

**Healthcare & Pharma:** Medtronic · Humana · Sanofi · Bristol Myers Squibb · HCA Healthcare · McKesson

**Telecom & Retail:** T-Mobile · AT&T · Comcast · Walmart · Target

### Recommended Daily Workflow
1. Run at **9:00 AM** every morning (captures all jobs from the previous 24 hours)
2. Filter to 🟣 **Both** + **Low competition** → apply within the hour
3. Filter to 🟢 **Explicit** + salary listed → apply same day
4. For big-name companies → click 🔍 Find Recruiter → send a personalized LinkedIn message before applying
5. Save the HTML file with today's date for tracking

---

## Important Notes

- **LinkedIn poster names are not exposed via scraping.** LinkedIn hides recruiter identity for ~99% of job postings. The "Find Recruiter" and "Find Hiring Mgr" links are pre-built LinkedIn People Search URLs that open relevant profiles when you're logged in.
- **Always verify visa sponsorship directly with the employer** before applying. The H1B filter increases your odds but is not a guarantee.
- **Delete your Claude API and Apify tokens** if you ever share them. Regenerate immediately if compromised.
- This pipeline uses publicly available LinkedIn data. Always comply with LinkedIn's terms of service.

---

*Built with Claude.ai + Apify · LinkedIn BI Jobs H1B Scraper Pipeline · Updated August 2026*
