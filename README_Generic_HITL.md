# ✅ Generic HITL (Human-in-the-Loop) – n8n Workflow

A Human-in-the-Loop approval workflow built with n8n, Discord, and Gmail.

This workflow demonstrates a real-world refund request process where low-value requests are auto-approved, while high-value requests are escalated to a human reviewer via Discord. The reviewer approves or denies the request directly from the Discord message, and the customer is notified automatically by email.

---

## Features

- n8n form trigger for customer refund submissions
- Automatic approval for low-value requests (under $100)
- Human review escalation via Discord for high-value requests
- Resume URL sent to Discord for one-click approve or deny
- 24-hour timeout on pending decisions
- Gmail notifications for both approved and denied outcomes
- Conditional branching with Merge node for unified approval path

---

## Tech Stack

- n8n
- Discord Bot API (OAuth2)
- Gmail API (OAuth2)
- n8n Form Trigger
- n8n Wait node (webhook resume)

---

## Workflow Overview

### 1. Form Submission
A customer fills out a refund request form with:
- Name
- Email
- Requested refund amount

### 2. Amount Check (If node)
The workflow evaluates the refund amount:
- **Under $100** → automatically routed to the approval path (no human review needed)
- **$100 or more** → escalated to a human reviewer via Discord

### 3. Discord Notification (High-value requests only)
A Discord message is sent to the configured channel containing:
- Customer name, email, and refund amount
- A clickable **Approve** link
- A clickable **Disapprove** link

Both links use n8n's built-in `$execution.resumeUrl` — no custom webhook setup required.

### 4. Wait Node
The workflow pauses and waits up to **24 hours** for the reviewer to click one of the links.  
If no action is taken within 24 hours, the execution times out.

### 5. Decision Check (If1 node)
The resume URL response is evaluated:
- **Approved** (`signature=true`) → routed to the Merge node
- **Denied** → customer receives a denial email

### 6. Email Notification
- **Approved path** → Gmail sends an approval confirmation to the customer
- **Denied path** → Gmail sends a denial notification to the customer

---

## Use Cases

- Refund request automation with human oversight
- Expense approval workflows
- Content moderation pipelines
- Any multi-step process requiring human sign-off
- Customer support escalation systems

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure Discord OAuth2 credentials in n8n
3. Configure Gmail OAuth2 credentials in n8n
4. In the **Send a message1** node, replace `YOUR_DISCORD_SERVER_ID` and `YOUR_DISCORD_CHANNEL_ID` with your actual values
5. In the **Approved** and **Denied** nodes, update the `sendTo` field to use the customer's email dynamically (e.g. `={{ $('On form submission').item.json.Email }}`)
6. Activate the workflow
7. Share the form URL with customers or embed it in your website

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
The `$execution.resumeUrl` is generated dynamically at runtime by n8n — no hardcoded URLs are needed.  
This project is intended for educational purposes.
