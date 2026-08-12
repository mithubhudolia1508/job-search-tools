# 🔍 Job Search Tools

A collection of automated job search pipelines built with **Claude.ai + Apify**.

---

## 📄 LinkedIn BI Jobs H1B Scraper — End-to-End Guide

**File:** [`linkedin_bi_h1b_job_scraper_guide.html`](./linkedin_bi_h1b_job_scraper_guide.html)

### What it does
An automated daily pipeline that:
1. Scrapes LinkedIn for **Business Intelligence full-time jobs** posted in the last 24 hours
2. Applies an **H1B visa sponsorship filter** (explicit keyword scan + known USCIS filers)
3. Delivers a **searchable, sortable interactive HTML table** with 9 columns including salary, applicants, visa signal tag, recruiter search links, and direct apply links

### Visa signal tags
| Tag | Meaning |
|-----|---------|
| 🟣 Both | Job explicitly mentions H1B/visa AND company is a documented H1B filer |
| 🟢 Explicit | Job description directly mentions sponsorship, H1B, OPT, EAD, work auth |
| 🔵 Known Sponsor | Company has USCIS H1B petition history even if posting doesn't mention it |

### Tools required
- [Claude.ai](https://claude.ai) — free plan works
- [Apify](https://apify.com) — free tier (~$5/mo credits, enough for 100+ runs)

### Setup time
~10 minutes one-time setup, then 2–3 minutes per daily run.

### View the guide
Open `linkedin_bi_h1b_job_scraper_guide.html` in any browser for the full interactive setup guide with copy-paste prompts, troubleshooting, and customization tips.

---

*Built with Claude.ai · Updated August 2026*
