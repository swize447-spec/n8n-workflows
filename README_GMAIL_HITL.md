# 📧 Gmail HITL (Human-in-the-Loop) – n8n Workflow

An AI-powered email assistant with human approval built with n8n, OpenAI, and Gmail.

This workflow monitors an inbox, automatically summarizes incoming emails, drafts a professional AI-generated reply, and sends it to a human reviewer for approval — before anything is sent to the original sender. If approved, the reply is sent automatically. If rejected, the AI regenerates a new draft.

---

## Features

- Automatic Gmail inbox monitoring (polling every minute)
- HTML to Markdown conversion for clean email processing
- AI email summarization using LangChain Summarization Chain
- Professional AI reply generation in HTML format
- Human approval gate using Gmail's sendAndWait feature
- Automatic reply on approval
- AI regeneration loop on rejection
- Fully automated end-to-end email handling

---

## Tech Stack

- n8n
- OpenAI GPT-4.1-mini
- LangChain Summarization Chain
- Gmail API (OAuth2)

---

## Workflow Overview

### 1. Gmail Trigger
The workflow polls the inbox every minute for new emails.  
Each new email is passed downstream for processing.

### 2. Markdown Conversion
The raw HTML email snippet is converted to clean Markdown format for better AI processing.

### 3. Summarization Chain
The email content is summarized using LangChain's Summarization Chain:
- condenses the email to a maximum of 100 words
- removes unnecessary formatting and noise
- prepares a clean input for the reply agent

### 4. Edit Fields
A pass-through configuration node used to manage data flow between the summarization and reply generation stages, and to handle the regeneration loop on rejection.

### 5. AI Agent (Reply Writer)
The summarized email is passed to an AI agent that:
- drafts a concise, professional business reply
- formats the output in HTML (using `<br>`, `<b>`, `<p>` tags)
- keeps the reply under 100 words
- writes only the body — no subject line

### 6. Send Message and Wait for Approval
The AI-generated draft is emailed to the reviewer with:
- the original email snippet
- the proposed AI reply
- Approve and Reject buttons (Gmail sendAndWait feature)

The workflow pauses until the reviewer responds.

### 7. Approval Check (If node)
- **Approved** → the AI reply is sent to the original sender
- **Rejected** → the workflow loops back to the Edit Fields node, triggering the AI Agent to regenerate a new draft

### 8. Send a Message
The approved reply is sent back to the original sender with the subject prefixed by `RE:`.

---

## Use Cases

- AI-assisted customer support email handling
- Business inbox automation with human oversight
- Email triage and response drafting
- HITL email workflows for compliance-sensitive environments
- Personal productivity — AI drafts, human approves

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure Gmail OAuth2 credentials in n8n
3. Configure OpenAI API credentials in n8n
4. In the **Send message and wait for response** node, replace `YOUR_APPROVAL_EMAIL` with the email address that should receive approval requests
5. Activate the workflow
6. Send a test email to your monitored inbox to trigger the flow

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
The workflow polls every minute by default — adjust the poll interval in the Gmail Trigger node to suit your needs.  
This project is intended for educational and test purposes.
