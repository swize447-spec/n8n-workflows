# 🤖 Discord Bot Slash Command – n8n Workflow

An AI-powered Discord joke bot built with n8n, OpenAI GPT-4.1-mini, and Discord's Interactions API.

This workflow registers a slash command on a Discord server that users can trigger directly in any channel. When invoked, the bot generates a joke based on the user's input and replies instantly using Discord's webhook interaction endpoint — no persistent bot connection required.

---

## Features

- Native Discord slash command integration
- AI joke generation powered by OpenAI GPT-4.1-mini
- Webhook-based interaction handling (no bot hosting required)
- Automatic response escaping for safe Discord message formatting
- Lightweight and serverless-friendly architecture
- Works with Discord's Interactions API v10

---

## Tech Stack

- n8n
- OpenAI GPT-4.1-mini
- Discord Interactions API (v10)
- JavaScript (n8n Code node)

---

## Workflow Overview

### 1. Webhook Trigger
Discord sends a POST request to the n8n webhook URL whenever a user invokes the slash command.  
The payload contains the user's input, application ID, and interaction token.

### 2. AI Agent
The user's slash command input is passed to an AI agent that:
- reads the topic or keyword provided by the user
- generates a relevant, witty joke
- returns the joke as a plain text string

### 3. Code in JavaScript
A custom JavaScript node processes the AI output:
- escapes backslashes to prevent formatting issues
- escapes double quotes for safe JSON embedding
- prepares the content for the Discord API response

### 4. HTTP Request (Discord API)
The escaped joke is sent back to Discord via a POST request to:
```
https://discord.com/api/v10/webhooks/{application_id}/{token}
```
The `application_id` and `token` are extracted dynamically from the original webhook payload — no hardcoded values needed.

---

## Use Cases

- Fun Discord community bots
- Slash command integration with AI
- Serverless Discord bot architecture
- Learning Discord Interactions API with n8n
- Entertainment and engagement tools for Discord servers

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure OpenAI API credentials in n8n
3. Register a slash command in the [Discord Developer Portal](https://discord.com/developers/applications):
   - Create a new application
   - Add a slash command (e.g. `/joke`)
   - Set the Interactions Endpoint URL to your n8n webhook URL
4. Update the `path` field in the Webhook node to match your chosen webhook path
5. Activate the workflow
6. Use `/joke [topic]` in any Discord channel to test

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
The `application_id` and interaction `token` are passed automatically by Discord at runtime — they do not need to be stored.  
This project is intended for educational and test purposes.
