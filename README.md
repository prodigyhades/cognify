# Cognify 🧠
### Hybrid Originality & Cognition Engine

**Cognify** is a system that verifies the originality of ideas using both strict text matching and abstract semantic understanding. It quantifies how "hard" an idea is to interpret and provides a unified **Cognitive Originality Index**.

---

## 🚧 Project Status
**✅ Level 0 (Core MVP):** The foundation. Accepts text, creates a deterministic SHA-256 hash to find exact duplicates, performs basic AI summarization, and stores data in Google Sheets.

**✅ Level 1 (Semantic Memory & Gatekeeper):** The upgrade. Adds **Vector Search (Qdrant)** to find conceptual duplicates (e.g., matching "Food coding app" with "Leetcode for cooking"). Introduces a **Cognitive Gatekeeper** that analyzes ideas for ambiguity and rejects vague or low-effort submissions before they enter the database.

---

## ⚙️ Architecture (Level 1)

* **Frontend:** Single-page HTML5 app (hosted on Vercel).
* **Orchestrator:** n8n (Self-hosted or Cloud).
* **Intelligence:** Google Gemini (LLM).
* **Memory:**
    * **Qdrant:** Vector database for semantic similarity.
    * **Google Sheets:** Relational storage for text and metadata.

### The Dual-Pipeline Workflow
Level 1 splits the logic into two distinct workflows for better user experience:
1.  **Search Pipeline:** Runs fast. Checks for semantic matches and calculates an Ambiguity Score. Does *not* save data.
2.  **Submission Pipeline:** Runs deep. Re-validates the Ambiguity Score. If the idea passes the "Gatekeeper" (Score < 0.7), it saves the text to Sheets and the vector embedding to Qdrant.

---

## 🧠 System Logic: The "VC Persona"

The AI evaluator is tuned to act as a **Product Strategist**. It grades ideas based on **Operational Specificity**:

* **Actionable (0.0 - 0.25):** Specific mechanics and features defined.
* **High-Level Concept (0.3 - 0.6):** "X for Y" analogies (e.g., "Leetcode for Cooking").
* **Vague (0.7 - 1.0):** Broad aspirations without details. **Rejected.**

**The "Lazy Analogy" Rule:**
If a user uses an analogy (e.g., "Uber for X") without explaining *how* it works, the system forces a high ambiguity score, blocking submission until specific details are added.

---

## 🚀 Setup Guide

### 1. Prerequisites
* **n8n** instance.
* **Google Cloud** project (Gemini API + Google Sheets API).
* **Qdrant Cloud** account (Free tier).
* **GitHub** & **Vercel** accounts.

### 2. Database
* **Google Sheets:** Create a sheet `cognify_db` with a tab `ideas`.
    * *Headers:* `idea_id`, `timestamp`, `normalized_text`, `raw_text`, `verdict`, `ai_analysis`, `embedding_stored`.
* **Qdrant:** Create a cluster and a collection named `ideas`.

### 3. n8n Workflows
Import the files from the `/n8n` folder:
1.  **`Cognify - Search.json`**: Connects to Gemini and Qdrant (Read Mode).
2.  **`Cognify.json`**: Connects to Gemini, Qdrant (Write Mode), and Google Sheets.

### 4. Frontend
1.  Deploy `/frontend` to Vercel.
2.  Update `index.html` constants with your n8n webhook URLs:
    ```javascript
    const SUBMIT_URL = "[https://your-n8n.com/webhook/cognify-v0](https://your-n8n.com/webhook/cognify-v0)";
    const SEARCH_URL = "[https://your-n8n.com/webhook/cognify-search](https://your-n8n.com/webhook/cognify-search)";
    ```

---

## 🗺️ Roadmap

* **✅ Level 0: Core MVP** - Hashing & Sheets persistence.
* **✅ Level 1: Semantic Memory** - Vector search & Ambiguity Gatekeeping.
* **🚧 Level 2: Reflex Diary** - Private cognitive analytics & data export.
* **🔮 Level 3: PersonaClone** - Generative user persona seeding.
* **🔮 Level 4: SynapseMatch** - Persona graph and simulation.

---

[MIT License](LICENSE)