# 🤖 Telegram Bot – n8n Workflow

An AI-powered Telegram chatbot built with n8n, OpenAI GPT-4.1-mini, and SerpAPI.

This workflow creates a fully functional Telegram bot that responds to user messages with witty, emoji-rich answers powered by an AI agent. The bot includes real-time web search capabilities via SerpAPI and maintains a short-term conversation memory for a more natural chat experience.

---

## Features

- Real-time Telegram message handling
- Typing indicator ("is typing...") for a natural chat feel
- AI responses powered by OpenAI GPT-4.1-mini
- Real-time web search via SerpAPI for up-to-date answers
- Short-term conversation memory (last 10 messages)
- Humorous, emoji-rich response style
- No hardcoded credentials — fully managed via n8n credential store

---

## Tech Stack

- n8n
- OpenAI GPT-4.1-mini
- Telegram Bot API
- SerpAPI (Google Search)
- n8n Memory Buffer Window

---

## Workflow Overview

### 1. Telegram Trigger
Listens for incoming messages sent to the Telegram bot.  
Activates on every new `message` update received.

### 2. Send a Chat Action
Immediately sends a `typing` indicator to the user's chat.  
This creates a natural "the bot is thinking" experience before the AI response arrives.

### 3. AI Agent
The core of the workflow — a GPT-4.1-mini powered agent that:
- receives the user's message text
- applies a witty, humorous system prompt with emoji-rich language
- uses SerpAPI to search the web when real-time information is needed
- accesses conversation memory to maintain context across messages

### 4. OpenAI Chat Model
Provides the language model backend (GPT-4.1-mini) to the AI Agent.

### 5. Simple Memory
A buffer window memory node that:
- stores the last 10 messages of the conversation
- uses the execution ID as the session key
- enables contextual, multi-turn conversations

### 6. SerpAPI (Tool)
Gives the AI Agent the ability to perform real-time Google searches when the user asks about current events, news, or any topic requiring up-to-date information.

### 7. Send a Text Message
Sends the AI-generated response back to the user in the same Telegram chat.

---

## Use Cases

- Personal AI assistant accessible via Telegram
- Community Telegram bot with web search capabilities
- Customer support bot prototype
- Conversational AI with real-time information access
- Entertainment bot with humor and personality

---

## Setup Instructions

1. Import the workflow into n8n
2. Create a Telegram bot via [@BotFather](https://t.me/BotFather) and obtain the bot token
3. Configure **Telegram API** credentials in n8n using your bot token
4. Configure **OpenAI API** credentials in n8n
5. Create a SerpAPI account at [serpapi.com](https://serpapi.com) and configure **SerpAPI** credentials in n8n
6. Link all credentials to their respective nodes
7. Activate the workflow
8. Start a conversation with your Telegram bot

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
The conversation memory resets with each new n8n execution — for persistent memory across sessions, consider replacing the Simple Memory node with a database-backed memory solution.  
This project is intended for educational and test purposes.
