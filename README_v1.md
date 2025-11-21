# Cognify 🧠
### Level 1: Semantic Memory & Cognitive Gatekeeper

**Cognify** is a hybrid originality engine. Level 1 upgrades the system from simple database lookups to **Semantic Vector Search**. It introduces a "Quality Gate" that uses AI to analyze the ambiguity of an idea, preventing vague or duplicate submissions from entering the database.

**New in Level 1:**
* **Vector Database (Qdrant):** Finds semantically similar ideas (e.g., "Food coding app" matches "Leetcode for cooking") even if words don't match exactly.
* **Cognitive Gatekeeping:** Ideas are scored on an Ambiguity Index (0.0 - 1.0). Ideas scoring > 0.7 are rejected automatically.
* **Dual-Pipeline Architecture:** Separate workflows for "Searching" and "Submitting" to optimize user experience.

---

## 🚀 How to Set Up & Run

### 1. Prerequisites
* All prerequisites from Level 0 (GitHub, Vercel, n8n, Google Cloud).
* **Qdrant Cloud Account:** A free tier cluster for vector storage. [Sign up here](https://qdrant.tech/).

### 2. Database Setup
#### A. Google Sheets (Persistence)
* Reuse your existing `cognify_db` sheet.
* Ensure the `ideas` tab has the following headers:
    `idea_id`, `timestamp`, `normalized_text`, `raw_text`, `verdict`, `ai_analysis`, `embedding_stored`

#### B. Qdrant (Vector Memory)
1.  Log in to your Qdrant Cloud Console.
2.  Create a **Cluster**.
3.  In the Dashboard, create a **Collection** named **`ideas`**.
4.  Get your **Cluster URL** and generate an **API Key**.

### 3. n8n Workflow Setup
Level 1 uses **two** distinct workflows. Import both into your n8n instance.

#### Workflow A: `Cognify - Search` (The Analyzer)
* **Purpose:** Calculates Ambiguity Score, generates Interpretations, and checks Qdrant for semantic duplicates without saving data.
* **Key Node:** `Structured Output Parser` (Enforces strict JSON formatting for the UI).
* **Configuration:**
    * **Embeddings Google Gemini:** Connect your Google PaLM/Gemini credential.
    * **Qdrant Vector Store (Search):** Connect your Qdrant credential. Set Operation to `Load` and Collection to `ideas`.

#### Workflow B: `Cognify` (The Submission Pipeline)
* **Purpose:** The "Gatekeeper". It re-analyzes the idea. If it passes the Ambiguity check (< 0.7), it saves the embedding to Qdrant and the text to Google Sheets.
* **Key Node:** `Ambiguity Check` (Code node that blocks low-quality data).
* **Configuration:**
    * **Qdrant Vector Store:** Set Operation to `Insert` and Collection to `ideas`.
    * **Append row in sheet:** Connect to your Google Sheet.

### 4. Frontend Setup
1.  Replace your existing `index.html` with the Level 1 version provided in `/frontend`.
2.  Update the `script` section in `index.html` with your **Production Webhook URLs**:
    ```javascript
    // Replace with your actual n8n production URLs
    const SUBMIT_URL = "[https://your-n8n-instance.com/webhook/cognify-v0](https://your-n8n-instance.com/webhook/cognify-v0)";
    const SEARCH_URL = "[https://your-n8n-instance.com/webhook/cognify-search](https://your-n8n-instance.com/webhook/cognify-search)";
    ```
3.  Commit and push to GitHub. Vercel will redeploy automatically.

---

## 🧠 System Logic

### The Scoring Rubric (VC Persona)
The AI evaluates ideas based on **Operational Specificity**:
* **0.0 - 0.25 (Actionable):** Specific mechanics and features defined.
* **0.3 - 0.6 (High-Level Concept):** "X for Y" analogies (e.g., "Leetcode for Cooking").
* **0.7 - 1.0 (Vague):** Broad aspirations without implementation details (e.g., "An app to help cooks"). **These are rejected.**

### The "Lazy Analogy" Rule
The system includes a specific penalty for analogies. If a user says "Uber for X" without explaining *how* it works, the Ambiguity Score is forced above 0.7, preventing submission.

---

## 🧪 Testing Level 1

1.  **Test Semantic Match:**
    * Submit: *"A chronological toaster that unburns bread."*
    * Search: *"A device to reverse entropy on toast."*
    * **Result:** Should show a high Similarity % (e.g., >80%) despite different wording.

2.  **Test Gatekeeper:**
    * Try to submit: *"An app that makes people happy."*
    * **Result:** The UI should block the Submit button and display: `⛔ Submission Blocked: Too Ambiguous`.

3.  **Test Submission:**
    * Submit a valid idea.
    * **Verify:** Check your Google Sheet for the row AND check your Qdrant dashboard to ensure the vector count increased.