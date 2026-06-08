# 📊 Workflow Chart Generator – n8n Workflow

An automated chart generation workflow built with n8n, Google Sheets, and QuickChart.

This workflow reads data from a Google Sheets spreadsheet and generates a PNG chart image using the QuickChart API. The chart type is configurable via a single field, making it easy to switch between bar, line, pie, and other chart types without modifying the code.

---

## Features

- Manual trigger for on-demand chart generation
- Configurable chart type via Edit Fields node (bar, line, pie, etc.)
- Data fetched dynamically from Google Sheets
- Custom JavaScript chart data builder
- PNG chart image generated via QuickChart API (no API key required)
- Supports responsive chart options, legend, and axis configuration
- Output returned as a binary PNG file

---

## Tech Stack

- n8n
- Google Sheets API (OAuth2)
- QuickChart API (free, no auth required)
- JavaScript (n8n Code node)

---

## Workflow Overview

### 1. Manual Trigger
The workflow is started manually by clicking "Execute workflow" in n8n.

### 2. Edit Fields (Chart Type)
Sets the chart type as a configurable variable:
- Default: `bar`
- Change this value to `line`, `pie`, `doughnut`, `radar`, etc. to switch chart types without touching the JavaScript code

### 3. Get Row(s) in Sheet
Reads all rows from the configured Google Sheets document.  
The sheet is expected to have at least two columns:
- `Month` — the X-axis labels
- `Revenue ($)` — the Y-axis numeric values

### 4. Code in JavaScript
Processes the sheet data and builds the QuickChart-compatible chart configuration:
- extracts `Month` values as chart labels
- extracts `Revenue ($)` values as numeric data points
- assembles the full chart object with dataset styling and axis options
- references the chart type dynamically from the Edit Fields node

### 5. HTTP Request (QuickChart API)
Sends the chart configuration to the QuickChart API:
- dimensions: 1000 x 500 px
- format: PNG
- background: white
- returns the chart as a binary image file

---

## Use Cases

- Automated business reporting from Google Sheets
- Monthly revenue or KPI chart generation
- No-code data visualization pipeline
- Scheduled or on-demand chart export
- Template for any spreadsheet-to-chart automation

---

## Google Sheets Format

The workflow expects the following column structure:

| Month | Revenue ($) |
|-------|-------------|
| Jan   | 12000       |
| Feb   | 15000       |
| Mar   | 11000       |

Column names are case-sensitive — ensure they match exactly.

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure Google Sheets OAuth2 credentials in n8n
3. Create a Google Sheets document with `Month` and `Revenue ($)` columns
4. In the **Get row(s) in sheet** node, replace `YOUR_GOOGLE_SHEETS_DOCUMENT_ID` with your actual document ID
5. Optionally change the chart type in the **Edit Fields** node (`bar`, `line`, `pie`, etc.)
6. Click **Execute workflow** to generate the chart
7. The output binary PNG can be passed to further nodes (email, Slack, Google Drive, etc.)

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
QuickChart is a free and open-source chart API — no API key is required for basic usage.  
To use different data columns, update the field names in the **Code in JavaScript** node accordingly.  
This project is intended for educational and test purposes.
