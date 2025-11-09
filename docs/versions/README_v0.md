# Cognify 🧠
### Level 0: Core Originality Engine

**Cognify** is a hybrid originality + cognition engine. This Level 0 MVP is a functional pipeline that accepts a short idea, verifies its originality against a database, performs a basic AI sentiment analysis, and returns a unified JSON response.

This system is built using:
* **Frontend:** Vercel (hosting `index.html`)
* **Backend:** n8n (self-hosted or cloud)
* **AI:** Google Gemini
* **Database:** Google Sheets

---

## 🚀 How to Set Up & Run

### 1. Prerequisites
* A **GitHub** account
* A **Vercel** account (linked to GitHub)
* An **n8n** workspace (local or cloud)
* A **Google Cloud** project with the **Google Drive API** enabled
* A **Google Gemini** API Key
* A **Google Sheet** (for the database)

### 2. Google Sheets Setup
1.  Create a new sheet at [sheets.new](https://sheets.new).
2.  Name the spreadsheet **`cognify_db`**.
3.  Name the first tab (sheet) **`ideas`**.
4.  Add the following headers in row 1 (A1 to H1):
    `idea_id`, `timestamp`, `normalized_text`, `raw_text`, `verdict`, `sentiment`, `summary`, `model`

### 3. n8n Workflow Setup
1.  Download the workflow from this repository: `/n8n/level-0-workflow.json`.
    *(Note: You need to download your final, working workflow.json from your n8n canvas and place it in this folder).*
2.  Import this JSON file into your n8n workspace.
3.  **Configure Credentials:**
    * **Google Gemini:** Create a "Google Gemini" credential using your API key.
    * **Google Sheets:** Create a "Google Sheets" credential and authorize it with the Google account that owns `cognify_db`.
4.  **Configure Nodes:**
    * Go to the **`Google Gemini Chat Model`** node and select your Gemini credential.
    * Go to the **`Search Sheet for idea_id`** node:
        * Select your Google Sheets credential.
        * Select the `cognify_db` spreadsheet and `ideas` sheet.
    * Go to the **`Append to Sheet`** node:
        * Select your Google Sheets credential.
        * Select the `cognify_db` spreadsheet and `ideas` sheet.
5.  **Activate** the workflow.

### 4. Frontend Setup
1.  Clone this repository and push it to your own GitHub account.
2.  Create a new Vercel project and link it to your repository.
3.  Set the **Root Directory** in Vercel to `frontend`.
4.  The `index.html` file will not work with a `localhost` n8n instance from Vercel due to CORS. Use a tool like **Postman** or the `curl` commands below for testing.

---