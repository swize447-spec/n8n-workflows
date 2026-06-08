# ✍️ Telegram HITL Tweet Writer – n8n Workflow

An AI-powered tweet writing and publishing assistant with Human-in-the-Loop review built with n8n, Google Gemini, Telegram, and X (Twitter).

This workflow allows users to generate, review, and refine AI-written tweets directly inside Telegram — and publish them to X (Twitter) with a single approval. The user provides a topic, the AI drafts a tweet, and the user can approve it, request modifications, or discard it — all through a natural conversation in Telegram. A Text Classifier interprets the user's freeform response and routes the workflow accordingly.

---

## Features

- AI tweet generation powered by Google Gemini
- Human-in-the-Loop review loop via Telegram sendAndWait
- Natural language intent classification (approve / modify / discard)
- Iterative refinement loop — modify until satisfied
- Three-way routing: publish, revise, or discard
- Automatic publishing to X (Twitter) on approval
- Fully conversational Telegram interface
- Single shared Gemini model powering all three AI nodes

---

## Tech Stack

- n8n
- Google Gemini 2.5 Flash Lite
- Telegram Bot API
- X (Twitter) API (OAuth2)
- n8n Text Classifier node
- n8n sendAndWait (HITL)

---

## Workflow Overview

### 1. Telegram Trigger
Listens for incoming messages sent to the Telegram bot.  
The user's message becomes the topic or idea for the tweet.

### 2. AI Agent (Tweet Writer)
A Google Gemini-powered agent that:
- reads the user's topic
- generates a concise, ready-to-publish tweet
- outputs only the tweet content with no extra commentary

### 3. Send Message and Wait for Response
Sends the proposed tweet back to the user in Telegram using the `sendAndWait` method.  
The workflow pauses and waits for the user's freeform text response (approve, request changes, or discard).

### 4. Text Classifier
Classifies the user's response into one of three categories using Google Gemini:
- **`true`** — the user approves the tweet → routes to confirmation + publish
- **`end`** — the user wants to discard → routes to discard notification
- **`false`** — the user wants modifications → routes to the reviewer agent

### 5. AI Agent1 (Tweet Reviewer)
Activated only when the user requests changes.  
Takes the original tweet and the user's feedback, and rewrites the tweet accordingly.  
Routes back to **Send Message and Wait for Response**, creating an iterative refinement loop.

### 6. Send a Text Message (Approved)
Notifies the user in Telegram that "The tweet has been sent" before publishing.

### 7. Create Tweet (X / Twitter)
Publishes the approved tweet to X (Twitter) using the X OAuth2 API.  
The tweet text is pulled directly from the AI Agent's final approved output.

### 8. Send a Text Message1 (Discarded)
Notifies the user that "Tweet discarded" when they choose to end the process.

---

## Use Cases

- AI-assisted social media content creation with one-click publishing
- Tweet drafting with human approval gate
- Conversational content workflow via Telegram
- HITL pattern demonstration with iterative refinement
- End-to-end generate → review → approve → publish automation

---

## Setup Instructions

1. Import the workflow into n8n
2. Create a Telegram bot via [@BotFather](https://t.me/BotFather) and obtain the bot token
3. Configure **Telegram API** credentials in n8n
4. Configure **Google Gemini (PaLM API)** credentials in n8n
5. Set up X (Twitter) OAuth2 credentials in n8n:
   - Go to [developer.twitter.com](https://developer.twitter.com) and create a new project and app
   - Enable **OAuth 2.0** under User Authentication Settings
   - Set **App permissions** to `Read and Write`
   - Set **Type of App** to `Web App`
   - Copy the **Callback URL** from n8n and paste it into the Twitter app settings
   - Copy your **Client ID** and **Client Secret** into n8n under **X OAuth2 API** credentials
6. Link the X OAuth2 credential to the **Create Tweet** node
7. Activate the workflow
8. Send any topic or idea to your Telegram bot to generate and publish a tweet

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
X (Twitter) Free Developer tier allows up to 1,500 tweets per month. For higher volume, a Basic plan is required.  
This project is intended for educational and portfolio purposes.
