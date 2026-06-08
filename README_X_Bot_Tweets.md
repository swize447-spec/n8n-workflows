# 🤖 X Bot – AI News Tweet Generator – n8n Workflow

An automated AI news tweet bot built with n8n, Google Gemini, and X (Twitter).

This workflow reads the latest AI news from Google News RSS, selects the most impactful article using Google Gemini, sanitizes and parses the output, and publishes a clean tweet directly to X (Twitter) — all with a single click or on a schedule.

---

## Features

- Real-time AI news fetching via Google News RSS feed
- Limits input to the 10 most recent articles
- Non-ASCII character sanitization for X API compatibility
- AI-powered article selection and tweet generation via Google Gemini
- Strict 250-character tweet limit enforced via system prompt
- JavaScript parsing node for clean and reliable output handling
- Direct publishing to X (Twitter) via OAuth2
- No hardcoded credentials — fully managed via n8n credential store

---

## Tech Stack

- n8n
- Google Gemini Flash Lite
- Google News RSS Feed
- X (Twitter) API (OAuth2)

---

## Workflow Overview

### 1. Manual Trigger
The workflow is started manually by clicking "Execute workflow".  
For automated publishing, replace this with a **Schedule Trigger** (e.g. every 6 hours).

### 2. RSS Read
Fetches the latest articles from the Google News RSS feed filtered by the query `AI+tools`.  
The feed URL can be modified to track any topic by changing the query parameter.

### 3. Limit
Caps the input to the **10 most recent articles** to keep the AI prompt concise and focused.

### 4. Code in JavaScript (Sanitize & Format)
Processes the raw RSS items:
- removes non-ASCII characters (e.g. Cyrillic, Chinese, emoji) from titles to prevent X API encoding errors
- formats each article as a bullet point: `• Title — Link`
- joins all articles into a single `rss_summary_prompt` string for Gemini

### 5. Message a Model (Google Gemini)
Sends the sanitized article list to Google Gemini Flash Lite with a strict system prompt:
- selects the single most impactful article
- responds with plain tweet text only
- no JSON, no markdown, no hashtags, no explanation
- maximum 250 characters

### 6. Parse Tweet (Code Node)
Extracts and cleans the Gemini response:
- reads `content.parts[0].text` from the Gemini response structure
- strips any accidental markdown or code fences
- collapses newlines into spaces
- trims and enforces the 250-character hard limit
- outputs a clean `tweet` field

### 7. Create Tweet
Publishes the parsed tweet to X (Twitter) using OAuth2 authentication.  
Reads the clean `tweet` field from the Parse Tweet node.

---

## Use Cases

- Automated AI news Twitter/X bot
- Social media content automation from RSS feeds
- Topic monitoring and auto-publishing pipeline
- Personal brand content automation
- Template for any RSS-to-tweet workflow

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure **Google Gemini (PaLM API)** credentials in n8n
3. Set up **X (Twitter) OAuth2** credentials in n8n:
   - Go to [developer.twitter.com](https://developer.twitter.com) and create a new project and app
   - Enable **OAuth 2.0** under User Authentication Settings
   - Set **App permissions** to `Read and Write`
   - Set **Type of App** to `Web App`
   - Copy the **Callback URL** from n8n and paste it into the Twitter app settings
   - Copy your **Client ID** and **Client Secret** into n8n under **X OAuth2 API** credentials
4. Optionally change the RSS feed query in the **RSS Read** node to track a different topic
5. To automate publishing, replace the Manual Trigger with a **Schedule Trigger**
6. Activate the workflow

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
X (Twitter) Free Developer tier allows up to 1,500 tweets per month. For higher volume, a Basic plan is required.  
To track a different topic, update the `q=` parameter in the RSS Read node URL (e.g. `q=machine+learning`).  
This project is intended for educational and test purposes.
