# Product Requirements Document
## Building Jerry — Multi-Agent Personal Assistant

| Field | Detail |
|-------|--------|
| Project Name | Building Jerry — Multi-Agent Personal Assistant |
| Document Type | Product Requirements Document (PRD) |
| Version | 1.0 |
| Date | March 2026 |
| Program | Applied Agentic AI |
| Institution | Interview Kickstart |
| Week | Week 1 — Multi-Agent Fundamentals |
| Model | Google Gemini (all agents) |
| Integrations | Gmail · Google Calendar · Fireflies.ai |
| Status | Production — Deployed |

---

## 1. Executive Summary

Jerry is a multi-agent personal assistant built in n8n that handles email, calendar, and meeting intelligence through a Master Agent orchestrating three specialist agents — all powered by Google Gemini. Users interact through a single chat interface; the Master Agent classifies intent and routes to the appropriate specialist agent.

The system integrates with Gmail (read, compose, send, reply, draft, delete), Google Calendar (create, read, check availability, update, delete events), and Fireflies.ai (list transcripts, retrieve full speaker-level meeting intelligence via GraphQL). Each specialist agent maintains its own memory buffer and tool suite.

---

## 2. Problem Statement

Personal productivity requires managing three high-friction domains simultaneously: email (high volume, requires contextual action), calendar (time-sensitive, error-prone), and meetings (knowledge trapped in recordings). Existing tools address each domain in isolation, requiring constant context switching.

Jerry solves this by deploying three specialist agents under a single Master Agent, enabling unified natural language control across all three domains in one conversation.

---

## 3. Objectives

- Route user queries to the correct specialist agent (Email, Calendar, or Meetings) via natural language intent classification
- Support multi-domain queries — single requests spanning email and calendar dispatch to both agents simultaneously
- Provide full Gmail CRUD operations: read, compose, send, reply, draft, and delete
- Provide full Google Calendar operations: create, read, check availability, update, and delete events
- Provide Fireflies.ai meeting intelligence: list transcripts, retrieve full speaker-level transcript details
- Enforce temporal awareness in the Calendar Agent — always fetch current date before any calendar action

---

## 4. Agent Architecture

| Agent | Model | Tools | Key Design Notes |
|-------|-------|-------|-----------------|
| Master Agent | Gemini | Email Agent Tool, Calendar Agent Tool, Meeting Agent Tool | Orchestrator — classifies intent and routes. Supports multi-domain dispatch. 10-turn context window. |
| Email Agent | Gemini | Get many messages, Send message, Reply to message, Create draft, Delete message | Reads emails before replying or deleting to extract correct Message ID. Full Gmail CRUD. |
| Calendar Agent | Gemini | Get many events, Check availability, Create event, Update event, Delete event, Date & Time | Always fetches current date first before any calendar action. Reads events before update/delete to extract Event ID. |
| Meeting Agent | Gemini | List of Transcripts (Fireflies GraphQL), Transcript (Fireflies GraphQL) | Two-step retrieval: list transcripts → extract ID → construct GraphQL body → retrieve full speaker-level transcript. |

---

## 5. Tool Integrations

### 5.1 Gmail Tools (5)
- **Get many messages** — read and summarize inbox; used before reply/delete to extract Message ID
- **Send a message** — compose and send new email
- **Reply to a message** — reply to existing thread using extracted Message ID
- **Create a draft** — save unsent draft
- **Delete a message** — permanently delete using extracted Message ID

### 5.2 Google Calendar Tools (5 + Date & Time)
- **Get many events** — read events within a time range
- **Get availability** — check free/busy status for a calendar
- **Create an event** — schedule new event with title, start, end, description
- **Update an event** — modify existing event using extracted Event ID
- **Delete an event** — remove event using extracted Event ID
- **Date & Time tool** — always called first by Calendar Agent to establish current date before any action

### 5.3 Fireflies.ai Tools (2 — GraphQL)
- **List of Transcripts** — POST to Fireflies.ai GraphQL API; returns transcript list with IDs and titles
- **Transcript** — POST with dynamically constructed GraphQL body; returns full transcript including speaker names, text, timestamps, attendees, and participants

---

## 6. User Stories

| ID | User Story | Acceptance Criteria | Priority |
|----|-----------|-------------------|----------|
| US-01 | As a user, I want to ask Jerry to send an email so I don't have to open Gmail. | Email Agent composes and sends email from natural language instruction. | Must Have |
| US-02 | As a user, I want to check my calendar availability for next week. | Calendar Agent fetches current date first, then retrieves events for the specified range. | Must Have |
| US-03 | As a user, I want to get a summary of what was discussed in yesterday's meeting. | Meeting Agent lists transcripts, identifies correct one, retrieves full speaker-level content. | Must Have |
| US-04 | As a user, I want to reply to the email from John about the project update. | Email Agent reads inbox first, extracts Message ID for John's email, sends reply. | Must Have |
| US-05 | As a user, I want to schedule a meeting for Tuesday at 2pm. | Calendar Agent fetches current date, creates event with correct date/time. | Must Have |
| US-06 | As a user, I want to ask about both my emails and my calendar in one message. | Master Agent dispatches to both Email and Calendar agents; synthesizes unified response. | Should Have |

---

## 7. Security Notes

- All Gmail and Google Calendar credentials managed via n8n OAuth2 credential store
- Fireflies.ai API key stored as a header auth credential in n8n — not hardcoded in workflow nodes
- No personal email content, calendar events, or meeting transcripts stored in this repository
- All integrations operate on the authenticated user's own accounts only

---

## 8. Academic Context

| Field | Detail |
|-------|--------|
| Program | Applied Agentic AI |
| Institution | Interview Kickstart |
| Week | Week 1 — Multi-Agent Fundamentals |
| Semester | Spring 2026 |
| Author | Lamonte Smith |
| GitHub | github.com/LSmithPMP/jerry-multi-agent-assistant |

---

*Lamonte Smith · Interview Kickstart — Applied Agentic AI · March 2026 · Confidential*
