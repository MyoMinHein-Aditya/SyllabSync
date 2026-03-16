# 📚 SyllabSync

An n8n automation that sends AI-generated weekly study prep emails and creates Google Calendar study events — every Monday, automatically.

Built with **Groq (LLaMA 3.3)**, **Gmail**, and **Google Calendar** — completely free to run.

---

## What It Does

Every Monday at 8AM, SyllabSync:
1. Uses **Groq AI** to write a personalized weekly study email for the student
2. Uses **Groq AI** to generate 2 Google Calendar study events for the week
3. **Sends the email** directly to the student via Gmail
4. **Creates the calendar events** automatically in Google Calendar

No approval step. No manual intervention. Fully automated.

---

## Demo

```
📧 Subject: 📚 Week 11 Study Prep — CS401 Advance Engineering

Hi there!
We're now in Week 11 of CS401 Advance Engineering...

1. Review of system design patterns
2. Practice problems on algorithm optimization
3. Study of case studies on engineering projects

📅 Calendar Events Created:
  - CS401 Study Session 1 → Wed 6:00 PM (90 min)
  - CS401 Study Session 2 → Fri 7:00 PM (90 min)
```

---

## Tech Stack

| Tool | Purpose | Cost |
|------|---------|------|
| [n8n](https://n8n.io) | Workflow automation | Free (self-hosted) / Cloud |
| [Groq](https://console.groq.com) | AI (LLaMA 3.3 70B) | Free tier — 14,400 req/day |
| Gmail OAuth2 | Send emails | Free |
| Google Calendar OAuth2 | Create events | Free |

---

## Workflow Structure

```
Every Monday 8AM
      ↓
   Setup
      ↓
Groq — Write Email
      ↓
  Store Email
      ↓
Groq — Generate Cal Events
      ↓
  Merge Results
      ↓
  ┌───┴───┐
Gmail   Split Cal Events
Send        ↓
      Google Calendar
        Create Event
```

---

## Prerequisites

- [n8n](https://n8n.io) instance (self-hosted or cloud)
- [Groq API key](https://console.groq.com/keys) — free, no credit card needed
- Gmail account connected via OAuth2 in n8n
- Google Calendar account connected via OAuth2 in n8n

---

## Setup

### 1. Import the Workflow

In n8n:
- Go to **Workflows → Import from File**
- Upload `SyllabSync_workflow.json`

### 2. Configure the Setup Node

Open the **Setup** node and fill in your values:

```javascript
const studentEmail  = 'YOUR_STUDENT_EMAIL@gmail.com';
const courseName    = 'YOUR_COURSE_NAME';
const groqApiKey    = 'YOUR_GROQ_API_KEY';
```

### 3. Connect Gmail Credentials

1. In n8n go to **Credentials → New → Gmail OAuth2**
2. Connect your Google account
3. Assign to the **Gmail — Send to Student** node

### 4. Connect Google Calendar Credentials

1. In n8n go to **Credentials → New → Google Calendar OAuth2**
2. Connect your Google account
3. Assign to the **Google Calendar — Create Event** node

### 5. Fix the Split Cal Events Connection

Make sure **Merge Results** connects to both:
- **Gmail — Send to Student**
- **Split Cal Events**

(Draw the second connection manually in the canvas if missing)

### 6. Publish and Run

- Click **Publish** (top right)
- Click **Execute workflow** to test immediately
- The schedule trigger fires every **Monday at 8AM** automatically

---

## Configuration

All config lives in the **Setup** node:

| Variable | Description |
|----------|-------------|
| `studentEmail` | Email address to send the weekly prep to |
| `courseName` | Name of the course (used in email + calendar events) |
| `groqApiKey` | Your Groq API key from console.groq.com |

The timezone for calendar events defaults to `Asia/Kolkata`. Change it in the **Merge Results** node:
```javascript
const TZ = 'Asia/Kolkata'; // Change to your timezone
```

---

## Getting a Groq API Key

1. Go to [https://console.groq.com](https://console.groq.com)
2. Sign up with your Google account (free)
3. Go to **API Keys → Create API Key**
4. Copy and paste into the Setup node

Groq's free tier gives **14,400 requests/day** — more than enough for weekly emails.

---

## Customization

**Change the schedule** — Edit the trigger node to fire on a different day/time.

**Change the AI model** — In both Groq nodes, replace `llama-3.3-70b-versatile` with any model from [Groq's model list](https://console.groq.com/docs/models).

**Change number of calendar events** — Edit the prompt in **Groq — Generate Cal Events** node, change `exactly 2` to your preferred number.

**Change timezone** — Edit `const TZ = 'Asia/Kolkata'` in the **Merge Results** node.

---

## Files

| File | Description |
|------|-------------|
| `SyllabSync_workflow.json` | n8n workflow — import this |
| `README.md` | This file |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| `Unauthorized` on Groq node | Check your API key in the Setup node |
| Calendar events not created | Make sure Merge Results connects to Split Cal Events |
| Gmail credential error | Re-authenticate Gmail OAuth2 in n8n Credentials |
| Workflow fires every minute | Change the trigger from `minutes` to `weeks` in the schedule node |

---

## License

MIT — free to use, modify, and distribute.
