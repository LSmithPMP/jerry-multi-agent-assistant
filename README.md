# Building Jerry — Multi-Agent Personal Assistant

> **Applied Agentic AI Program · Week 1 · Interview Kickstart · Spring 2026**  
> Lamonte Smith

![Jerry — Multi-Agent Workflow](%20jerry-workflow-diagram.png)

---

A conversational multi-agent personal assistant built in n8n that handles **email**, **calendar**, and **meeting intelligence** through a Master Agent orchestrating three specialist agents — all powered by Google Gemini.

---

## System Architecture

```
User (Chat Interface)
        │
        ▼
┌─────────────────────────────────────┐
│           Master Agent              │
│         (Google Gemini)             │
│  • Intent classification            │
│  • Multi-domain routing             │
│  • 10-turn context window           │
└────┬──────────────┬─────────────────┘
     │              │              │
     ▼              ▼              ▼
┌─────────┐  ┌───────────┐  ┌───────────┐
│  Email  │  │ Calendar  │  │ Meeting   │
│  Agent  │  │   Agent   │  │   Agent   │
│ (Gemini)│  │ (Gemini)  │  │ (Gemini)  │
└────┬────┘  └─────┬─────┘  └─────┬─────┘
     │             │              │
  Gmail (5)   Google Cal (6)  Fireflies.ai
  tools       + Date/Time     GraphQL API
```

---

## Agents

### Master Agent
- **Model:** Google Gemini
- **Role:** Orchestrator — classifies intent and routes to correct specialist
- **Memory:** Simple Memory Buffer — 10-turn context window
- **Capability:** Multi-domain dispatch — single query can trigger multiple agents simultaneously

### Email Agent
- **Model:** Google Gemini
- **Tools:** Get many messages · Send message · Reply to message · Create draft · Delete message
- **Design note:** Always reads emails before replying or deleting to extract the correct Message ID

### Calendar Agent
- **Model:** Google Gemini
- **Tools:** Get many events · Check availability · Create event · Update event · Delete event · Date & Time
- **Design note:** Always fetches current date first before any calendar action to prevent scheduling errors

### Meeting Agent
- **Model:** Google Gemini
- **Tools:** List of Transcripts (Fireflies GraphQL) · Transcript (Fireflies GraphQL)
- **Design note:** Two-step retrieval — list transcripts → extract ID → construct GraphQL body → retrieve full speaker-level transcript

---

## Tech Stack

![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat&logo=n8n&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google_Gemini-4285F4?style=flat&logo=google&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-D14836?style=flat&logo=gmail&logoColor=white)
![Google Calendar](https://img.shields.io/badge/Google_Calendar-4285F4?style=flat&logo=googlecalendar&logoColor=white)

**Model:** Google Gemini (all agents)  
**Integrations:** Gmail · Google Calendar · Fireflies.ai (GraphQL)  
**Patterns:** Master-Worker · Multi-Domain Routing · Tool-as-Agent · Window Buffer Memory

---

## Sample Queries

```
"Send an email to John saying I'll be late to the meeting"
"What's on my calendar this week?"
"What did we discuss in yesterday's standup?"
"Reply to Sarah's email about the Q2 report"
"Schedule a 1-on-1 with Mike for Tuesday at 3pm"
"Check if I'm free Thursday afternoon"
```

---

## Academic Context

| Field | Detail |
|-------|--------|
| Program | Applied Agentic AI |
| Institution | Interview Kickstart |
| Week | Week 1 — Multi-Agent Fundamentals |
| Semester | Spring 2026 |

---

## Security Notes

- All credentials managed via n8n credential store — never hardcoded
- Gmail and Google Calendar use OAuth2
- Fireflies.ai API key stored as n8n header auth credential
- See [SECURITY.md](SECURITY.md) for full policy

---

## Repository Structure

```
jerry-multi-agent-assistant/
├── workflow/
│   └── jerry-multi-agent-assistant.json
├── docs/
│   └── PRD.docx
├── SECURITY.md
└── README.md
```

---

## License

MIT License — see [LICENSE](LICENSE)

---

<div align="center">
<sub>Built by <a href="https://github.com/LSmithPMP">Lamonte Smith</a> · Applied Agentic AI · Interview Kickstart · Spring 2026</sub>
</div>
