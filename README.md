A production‑ready lead scoring and routing system that automatically categorizes prospects, triggers real‑time alerts, and sends personalized email responses.

📖 Table of Contents

Overview
Why This Exists
Workflow Architecture
Features
Tech Stack
How It Works (Step by Step)
Google Sheets Output
Future Enhancements
Demo
Author


🔮 Overview
Smart Lead Engine is an n8n‑based automation that captures incoming leads, evaluates them based on company size, and routes them to the appropriate sales track. It eliminates manual lead sorting, ensures immediate follow‑up, and logs everything to Google Sheets for tracking.

This is not a generic chatbot. It's a lead intelligence system that helps sales teams prioritize high‑value prospects and respond instantly.

🧠 Why This Exists
Businesses lose leads every day because:

They don't know which leads are high‑priority.

They respond too slowly (or not at all).

They waste time on manual data entry and routing.

Smart Lead Engine solves this by:

Automatically scoring leads as Gold, Silver, or Bronze.

Sending instant Discord alerts for Gold leads (so sales teams can call within seconds).

Sending personalized emails to every lead.

Logging everything to Google Sheets with a LeadGrade column.


🧩 Workflow Architecture
┌─────────────────────────────────────────────────────────────────┐
│                         LEAD CAPTURE                          │
│                  (Webhook / HTML Form / curl)                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        DATA POLISHER                          │
│          (Set Node – clean name, lowercase email)             │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         ROUTING (IF Nodes)                    │
├─────────────────────────────────────────────────────────────────┤
│  Bronze (1-10) │ Silver (11-50) │ Gold (51+) │ Fallback        │
└─────────────────────────────────────────────────────────────────┘
          │              │               │              │
          ▼              ▼               ▼              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      EMAIL DELIVERY                           │
│   Personalized Gmail – dynamic subject + custom body          │
└─────────────────────────────────────────────────────────────────┘
          │              │               │              │
          ▼              ▼               ▼              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      LOGGING & ALERTS                         │
│   Google Sheets (LeadGrade column) + Discord Alerts (Gold)    │
└─────────────────────────────────────────────────────────────────┘


🚀 Features
Automatic Lead Scoring – Categorizes leads as Gold, Silver, or Bronze based on company size.

Personalized Email Responses – Each branch sends a unique email with a dynamic subject line (e.g., "Priority Delivery: Ali's Guide for Tech").

Instant Discord Alerts – Gold leads trigger a real‑time notification so your sales team can act immediately.

Google Sheets Logging – Every lead is logged with a LeadGrade column for easy filtering and reporting.

Data Cleaning – Automatically formats names (proper case) and emails (lowercase) before logging or sending.

Anti‑Spam Delay – 5‑second wait before sending emails to avoid being flagged as spam.

Fallback Alert – Any lead that doesn't match a category triggers a Discord alert for manual review.

HTML Test Form – Includes a ready‑to‑use HTML form for testing without curl or technical tools.

Error Resilient – The workflow continues running even if an individual branch fails.


🛠️ Tech Stack
n8n – Workflow orchestration (self‑hosted).

Gmail API – Sending personalized emails.

Google Sheets API – Lead logging with LeadGrade column.

Discord Webhooks – Real‑time alerts for Gold and Fallback leads.

HTML + CSS – Test form for non‑technical demo.


⚙️ How It Works (Step by Step)

1. Lead Capture:

The workflow listens for incoming POST requests on a webhook.
You can send data via a HTML form, curl, or any tool that can POST JSON.

Example payload:

json
{
  "name": "Ali",
  "email": "ali@example.com",
  "CompanySize": "51+",
  "industry": "Tech"
}

2. Data Polishing:

A Set Node cleans the data:

name → Proper case (e.g., "ALI" → "Ali")
email → Lowercase (e.g., "ALI@GMAIL.COM" → "ali@gmail.com")

3. Routing (IF Nodes):

Three IF Nodes check the CompanySize field:

"1-10" → Bronze
"11-50" → Silver
"51+" → Gold

If none match → Fallback

4. Email Delivery:

Each branch has a Gmail node that sends a personalized email.

The email subject is dynamic, e.g.:

Bronze: "Here's your guide, Ali"
Silver: "Important: Ali's Guide for Tech"
Gold: "Priority Delivery: Ali's Guide for Tech"


5. Logging & Alerts:

Google Sheets appends a new row with all lead data + LeadGrade.
Discord sends an alert for Gold leads (and Fallback leads).

📊 Google Sheets Output:
When a lead is processed, the following columns are populated:

Name – Cleaned lead name (proper case).
CompanySize – Original company size (e.g., "51+").
Industry – Industry type.
Email – Cleaned email (lowercase).
EmailSubject – Dynamic subject line.
LeadGrade – Gold, Silver, Bronze, or Fallback.

🚀 Future Enhancements

SMS Alerts – Add Twilio node to send SMS for Gold leads.
HubSpot Integration – Push leads directly to HubSpot CRM.
Lead Enrichment – Use an API to enrich lead data (company website, LinkedIn, etc.).
Slack Support – Replace Discord with Slack webhooks.
Dashboard UI – Build a simple web dashboard to monitor lead flow.
Email Analytics – Track open rates and click‑throughs.

🎥 Demo
🔗 Loom Demo – [Smart Lead Engine](https://www.loom.com/share/5a9f1d5317c94b0d9c0e0c3fd341fc74)

👤 Author
Ali 

GitHub: @MxAli-56
Upwork: [Your Upwork Profile](https://www.upwork.com/freelancers/~01c07358643df0a9fb)

📄 License
This project is open‑source and available under the MIT License.

🙏 Acknowledgments

Built with n8n – the open‑source workflow automation tool.
Powered by Gmail, Google Sheets, and Discord APIs.

"Every lead deserves a response. Automate the scoring, prioritize the action."
