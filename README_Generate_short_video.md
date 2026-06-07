# 🎬 Generate a Short Video – n8n Workflow

An AI-powered short video generation workflow built with n8n, Google Gemini, Google Text-to-Speech, Pexels, Cloudinary, and Shotstack.

This workflow automatically generates a complete 30-second YouTube Short — from topic ideation and script writing, through voiceover creation and image sourcing, to final video rendering — all triggered from a single chat message.

---

## Features

- AI topic generation optimized for YouTube Shorts (30 seconds)
- Structured JSON script generation with title, narration, and scene descriptions
- Text-to-speech voiceover using Google Neural2 voices
- Automatic audio upload to Cloudinary for public URL hosting
- Per-scene keyword extraction for contextual image search
- Stock image sourcing via Pexels API (portrait format)
- Parallel audio and image processing pipelines
- Automatic video assembly and rendering via Shotstack (9:16 MP4)

---

## Tech Stack

- n8n
- Google Gemini (PaLM API)
- Google Cloud Text-to-Speech API
- Cloudinary (audio hosting)
- Pexels API (stock images)
- Shotstack API (video rendering)

---

## Workflow Overview

### 1. Chat Trigger
The workflow starts when a message is received in the n8n chat interface.  
No specific input is required — the AI generates the topic autonomously.

### 2. Generate Topic Agent
A Google Gemini-powered agent generates one surprising, engaging world fact formatted for exactly 30 seconds of narration.

### 3. Generate Script (Structured) Agent
A second Gemini agent converts the fact into a structured video script in strict JSON format containing:
- `title` — the video title
- `script` — the full narration text (max 85–90 words)
- `scenes` — an array of scene objects with `text` and `visual` descriptions

### 4. Edit Fields (Parse Script)
Parses the JSON string output from the script agent into separate fields:  
`title`, `script`, `scenes` — making them available to downstream nodes.

From this point the workflow splits into two parallel tracks:

---

### 🎙️ Audio Track

#### 5. Text to Speech
The full script is sent to Google Cloud Text-to-Speech API:
- Voice: `en-US-Neural2-D`
- Speaking rate: 1.05x
- Output: MP3 audio encoded in base64

#### 6. Convert Audio to Binary
The base64-encoded audio response is converted to a binary MP3 file.

#### 7. Generate URL for Download (Cloudinary)
The MP3 file is uploaded to Cloudinary via multipart form upload.  
A public `secure_url` is returned for use in the video timeline.

#### 8. Edit Fields2
Extracts and stores the Cloudinary `secure_url` for the Merge node.

---

### 🖼️ Image Track

#### 5. Code in JavaScript
Extracts the `scenes` array from the parsed script and maps each scene to its `text` and `visual` fields.

#### 6. Code in JavaScript1
Processes each scene's `visual` description with a keyword extraction algorithm:
- removes stop words and short words
- extracts 4 meaningful search terms per scene
- outputs a `pexelsSearchTerm` for each scene

#### 7. Pexels Request
For each scene, queries the Pexels API with the extracted search term:
- returns 1 portrait-format stock photo per scene

#### 8. Edit Fields1
Extracts the `portrait` image URL from the Pexels response and stores it as `image_url`.

---

### 🎬 Video Assembly

#### 9. Merge
Combines the audio URL (from the Audio track) and all image URLs (from the Image track) into a single output for video building.

#### 10. Build Shotstack Body
A JavaScript code node assembles the complete Shotstack video timeline:
- creates one image clip per scene, evenly distributed across 30 seconds
- adds the audio track spanning the full duration
- sets output format to MP4, HD resolution, 9:16 aspect ratio

#### 11. Shotstack Request
Submits the video render job to the Shotstack API.  
Shotstack processes the timeline asynchronously and returns a render ID.

---

## Use Cases

- Automated YouTube Shorts creation
- AI content production pipelines
- Social media video automation
- Fact-based educational short video generation
- Template for any text-to-video automation workflow

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure Google Gemini (PaLM API) credentials in n8n
3. Obtain a **Google Cloud Text-to-Speech API key** and replace `YOUR_GOOGLE_TTS_API_KEY` in the **Text to speech** node URL
4. Create a **Cloudinary** account, create an unsigned upload preset, and replace:
   - `YOUR_CLOUDINARY_CLOUD_NAME` in the **Generate URL for Download** node URL
   - `YOUR_CLOUDINARY_UPLOAD_PRESET` in the request body
5. Obtain a **Pexels API key** and replace `YOUR_PEXELS_API_KEY` in the **Pexels request** node `Authorization` header
6. Obtain a **Shotstack API key** and replace `YOUR_SHOTSTACK_API_KEY` in the **Shotstack Request** node `x-api-key` header
7. Activate the workflow
8. Open the n8n chat interface and send any message to trigger video generation

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials before activating the workflow.  
Shotstack renders videos asynchronously — use the returned render ID to poll the Shotstack status endpoint for the final video URL.  
This project is intended for educational and portfolio purposes.
