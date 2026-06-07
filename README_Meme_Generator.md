# 🎭 Meme Generator – n8n Workflow

An AI-powered meme generator built with n8n, Google Gemini, Stability AI, and n8n's native image editing.

This workflow takes any topic or idea from a chat message and automatically generates a complete meme — including a funny concept, an AI-generated image, and text overlaid directly onto the image — using a multi-agent pipeline.

---

## Features

- Multi-agent AI pipeline for meme creation
- Structured JSON output parsing for reliable meme content
- AI image generation via Stability AI (Stable Diffusion XL)
- Automatic text overlay on generated images
- Two-stage prompt refinement: concept agent + image prompt agent
- Fully automated end-to-end meme generation
- Output delivered as a completed image with text

---

## Tech Stack

- n8n
- Google Gemini (PaLM API)
- Stability AI (Stable Diffusion XL v1)
- n8n Edit Image node
- LangChain Structured Output Parser

---

## Workflow Overview

### 1. Chat Trigger
The user sends a topic, idea, or prompt through the n8n chat interface.  
This becomes the inspiration for the meme.

### 2. Meme Generator Agent
A Google Gemini-powered agent that:
- interprets the user's input
- comes up with a funny, relatable meme concept
- generates a short, punchy single-line text for the image overlay
- outputs a structured JSON with `description` and `text` fields

The **Structured Output Parser** enforces the JSON schema, ensuring consistent and parseable output every time.

### 3. Prompt Generator Agent
A second Google Gemini-powered agent that:
- takes the meme `description` from the previous agent
- translates it into a detailed image generation instruction
- optimizes the prompt for Stable Diffusion image quality

### 4. Edit Fields
Prepares the final payload for the Stability AI API:
- sets the model to `stable-diffusion-xl-v1`
- maps the generated prompt
- sets the output format to `png`

### 5. HTTP Request (Stability AI)
Sends the optimized image prompt to the Stability AI API (`/v2beta/stable-image/generate/core`) using Bearer token authentication.  
Returns the generated image as binary data.

### 6. Edit Image
Overlays the AI-generated meme text onto the generated image:
- font size: 100px
- font color: white (`#F8F4F4`)
- positioned near the top of the image
- line length constrained for readability

---

## Use Cases

- Automated meme generation for social media
- Fun community engagement tools
- AI creative content pipelines
- Demonstration of multi-agent n8n workflows
- Image generation with dynamic text overlays

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure Google Gemini (PaLM API) credentials in n8n
3. Create a Stability AI account at [stability.ai](https://stability.ai) and generate an API key
4. Add the Stability AI API key as an **HTTP Bearer Auth** credential in n8n
5. Link the Bearer Auth credential to the **HTTP Request** node
6. Activate the workflow
7. Open the n8n chat interface and type any topic to generate a meme

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
This project is intended for educational and test purposes.
