# Security Policy

## Credentials & Secrets

All credentials in this workflow are managed through n8n's built-in credential store:

- **Google Gemini API key** — stored as named n8n credential, never hardcoded
- **Gmail OAuth2** — stored as named n8n credential
- **Google Calendar OAuth2** — stored as named n8n credential
- **Fireflies.ai API key** — stored as header auth credential in n8n

No API keys, tokens, or secrets appear in workflow node logic or this repository.

## Data Handling

- This workflow operates on the authenticated user's own Gmail, Google Calendar, and Fireflies.ai accounts only
- No email content, calendar events, or meeting transcripts are stored in this repository
- All data remains within the user's own Google and Fireflies.ai accounts

## Reporting a Vulnerability

1. **Do not** open a public GitHub issue
2. Email: lamontesmithpmp@gmail.com with subject line `[SECURITY] jerry-multi-agent-assistant`
3. Expected response time: 72 hours

## Disclaimer

This workflow was built for academic demonstration purposes as part of the Interview Kickstart Applied Agentic AI program. Before deploying with real personal accounts, review OAuth2 scopes and ensure they align with your privacy requirements.
