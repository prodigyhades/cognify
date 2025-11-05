# Cognify — Level 0: Core MVP

**Cognify** is a hybrid originality and cognition engine that verifies short-form ideas for originality, quantifies interpretive difficulty, and produces an explainable **Cognitive Originality Index (COI)**.  

Level 0 delivers a functional, testable MVP that connects a simple frontend to an n8n workflow and Gemini LLM.  
The system accepts text input, performs deterministic hashing and duplicate detection, calls Gemini for summarization or sentiment analysis, and returns a structured JSON response.

---

### ⚙️ Architecture
- **Frontend:** `/frontend/index.html` (textarea, mode selector, results panel)  
- **Backend:** `n8n` workflow (`/n8n/n8n-workflow.json`)  
- **Persistence:** Google Sheet or SQLite store  
- **Hosting:** Vercel (or local server)  
- **Version Control:** GitHub  

---

### 🚀 Setup
1. Create repo `cognify` with `main` and `dev` branches.  
2. Deploy or run `/frontend` locally.  
3. Configure an **n8n webhook URL** and assign it to `WEBHOOK_URL`.  
4. Connect Google Sheet or SQLite for persistence.  
5. Test via form submission or cURL POST.

---

### ✅ Tests
- Submitting the same text twice → “Exact Duplicate” response.  
- **Summary** and **Sentiment** modes return parsed Gemini JSON.

---

Level 0 forms the foundation for future Levels (semantic memory, crowd ratings, diary, persona, simulation).