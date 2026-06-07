# 🔍 LinkedIn Scraper – n8n Workflow

An AI-powered LinkedIn search and scraping workflow built with n8n, Google Gemini, Serper API, and Google Sheets.

This workflow allows users to describe a job role or candidate profile through a chat interface, and automatically generates optimized Boolean search strings to find matching LinkedIn job posts or profiles via Google Search. Results are scraped in paginated batches and saved to a Google Sheets spreadsheet for easy review.

---

## Features

- Dual mode: search LinkedIn job posts or LinkedIn profiles
- AI-powered Boolean search string generation using Google Gemini
- Real-time Google Search via Serper API
- Automatic Google Sheets creation per search query
- Paginated scraping loop (configurable batch size)
- LinkedIn URL extraction from organic search results
- Completion notification via n8n chat interface
- Wait node for rate-limit-friendly scraping

---

## Tech Stack

- n8n
- Google Gemini (PaLM API)
- Serper API (Google Search)
- Google Sheets API (OAuth2)

---

## Workflow Overview

### 1. Chat Trigger
The user sends a job description or candidate profile description through the n8n chat interface.

### 2. Edit Fields (Set Mode)
A configuration node sets the scraping mode:
- `jobs` → searches `site:linkedin.com/jobs`
- `profiles` → searches `site:linkedin.com/in`

Change the `mode` value in this node to switch between the two modes.

### 3. If (Mode Check)
Routes the workflow based on the selected mode to the appropriate prompt setter.

### 4. Set Jobs Prompt / Set Profiles Prompt
Loads the correct system prompt for Google Gemini based on the selected mode:
- **Jobs prompt** — generates Boolean strings targeting LinkedIn job listings
- **Profiles prompt** — generates Boolean strings targeting LinkedIn user profiles

### 5. Message a Model (Google Gemini)
Sends the user's input and the mode-specific prompt to Google Gemini, which returns:
- a precise Boolean search string
- a `sheet_name` for the Google Sheets tab

### 6. Create Sheet
A new tab is created in the configured Google Sheets document, named after the AI-generated `sheet_name`.

### 7. Code in JavaScript (Initialize)
Initializes the scraping loop by setting up an empty `linkedin_url` placeholder row and a `start` counter at `0`.

### 8. Append Row in Sheet
Writes the initial header row to the newly created Google Sheets tab.

### 9. Code in JavaScript1 (Loop Init)
Sets the pagination `start` value to `0` to begin the scraping loop.

### 10. If1 (Pagination Check)
Controls the scraping loop — continues fetching results while `start < 15` (configurable).  
When the limit is reached, routes to the Chat completion node.

### 11. HTTP Request (Serper API)
Sends the Boolean search string to the Serper API (Google Search) with the current pagination offset, retrieving a batch of organic search results.

### 12. Code in JavaScript2 (Extract Links)
Extracts all LinkedIn URLs from the organic search results and formats them as individual items with a `linkedin_url` field.

### 13. Append Row in Sheet1
Appends the extracted LinkedIn URLs to the Google Sheets tab in batch.

### 14. Aggregate
Collects all items from the current batch into a single output for pagination tracking.

### 15. Code in JavaScript3 (Increment)
Increments the `start` counter by 10 to fetch the next page of results on the next loop iteration.

### 16. Wait
Introduces a pause between pagination requests to avoid rate limiting.  
The loop then returns to **If1** for the next batch.

### 17. Chat (Completion)
Sends a "Completed" message back to the n8n chat interface when all pages have been scraped.

---

## Use Cases

- LinkedIn job post aggregation and research
- Candidate sourcing and talent pipeline building
- Recruitment automation
- Boolean search string generation for HR teams
- Lead generation from LinkedIn profiles

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure Google Gemini (PaLM API) credentials in n8n
3. Configure Google Sheets OAuth2 credentials in n8n
4. Add your Serper API key in the **HTTP Request** node (`x-api-key` header)
5. Create a Google Sheets document and replace `YOUR_GOOGLE_SHEETS_DOCUMENT_ID` in all three Google Sheets nodes
6. In the **Edit Fields** node, set `mode` to either `jobs` or `profiles`
7. Activate the workflow
8. Open the n8n chat interface and describe the role or candidate you are looking for

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
The pagination limit is set to `15` by default in the **If1** node — adjust this value to scrape more or fewer results.  
This project is intended for educational purposes. Users are responsible for ensuring their usage complies with LinkedIn's Terms of Service.
