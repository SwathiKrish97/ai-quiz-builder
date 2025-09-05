# AI-Powered Knowledge Quiz Builder

This is a simple MVP web application that generates multiple-choice quizzes from any user-provided topic using an AI model.  
It was built as part of a technical interview challenge.

---

## Objective
The app allows a user to:
1. Enter a topic (e.g., "Photosynthesis", "Cricket", "Ancient Rome").
2. Generate **5 multiple-choice questions** (each with 4 options, only one correct).
3. Answer the quiz inside a web interface.
4. Submit answers and receive:
   - Score (e.g., 3/5)
   - Correct answers
   - Feedback with explanations
5. Review past results (saved to `results.json`).

---

## Tech Stack
- **Frontend:** [Gradio](https://www.gradio.app/) (lightweight Python web UI framework)  
- **Backend:** Python 3.x  
- **AI Model:** OpenAI GPT (default: `gpt-4o-mini`)  
- **Extras:**
  - Wikipedia retrieval for better factual grounding
  - Persistent quiz results (`results.json`)
  - Secure key handling via `.env`

---

## Project Structure
```
ai-quiz-builder/
├── app.py            # Main app (run in terminal)
├── results.json      # Stores past quiz results (runtime)
├── requirements.txt  # Dependencies
├── .env.example      # Example env file (replace with your own API key)
├── .gitignore        # Ignore secrets and runtime files
└── README.md         # This file
```

---

## Setup Instructions

### 1. Clone repo and enter folder
```bash
git clone <your-repo-url>
cd ai-quiz-builder
```

### 2. Create virtual environment (recommended)
```bash
python3 -m venv venv
source venv/bin/activate   # macOS/Linux
venv\Scripts\activate    # Windows
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Set up environment variables
- Copy `.env.example` → `.env`
- Open `.env` and paste your OpenAI API key:
  ```
  OPENAI_API_KEY=sk-your-key-here
  ```

### 5. Run the app
```bash
python app.py
```

### 6. Open in browser
Visit 👉 [http://127.0.0.1:7860](http://127.0.0.1:7860)

---

## Example Usage
- Enter topic: `Photosynthesis`  
- App generates 5 MCQs  
- Select answers and click **Submit**  
- Get instant score + feedback:
  ```
  Score: 4/5
  Q1. Correct ✅
  Q2. Incorrect ❌ – Correct answer was X
  ```

---

## Notes
- Requires a valid **OpenAI API key**.  
- Keys are loaded via `.env` (never hardcoded).  
- Gradio provides a lightweight web-based interface.  
- Bonus features implemented:
  - ✅ Retrieval with Wikipedia grounding  
  - ✅ Feedback explanations  
  - ✅ Persistent results (saved to results.json)  

---

## Architecture & Design Summary

### Generate Quiz Flow
1. **User:** Enters a topic and clicks **Generate Quiz**.  
2. **Gradio UI:** Calls `generate_quiz()`.  
3. **Prompt Builder:** Prepares strict JSON instructions, optionally adds Wikipedia context.  
4. **OpenAI (gpt-4o-mini):** Returns quiz JSON (5 questions × 4 options, one correct).  
5. **Validator:** Enforces schema correctness (5 Qs, 4 distinct options, valid `correct_index`).  
6. **UI:** Displays questions as single-select radio groups.  

### Submit Answers Flow
1. **User:** Selects answers and clicks **Submit**.  
2. **Evaluator:** Compares selections with `correct_index` and computes score.  
3. **Persistence:** Appends the attempt to `results.json` (timestamp, topic, score, per-question details).  
4. **UI:** Shows score, correct answers, and explanations (when provided).  

### Design Priorities
- Simple, modular components (prompt, generator, validator, UI, persistence).  
- Strict JSON output + runtime validation for reliability.  
- Secrets via `.env` with `python-dotenv`.  
- Save attempts in JSON for easy local storage and future DB upgrade 

### Architecture Diagram
```
User ──> Gradio UI ──> Prompt Builder ──> OpenAI GPT
   │          │                               │
   │          └────────< Validator <──────────┘
   │
   └── Submit Answers ──> Evaluator ──> Persistence (results.json)
                               │
                               └──> Score + Feedback to UI
```

## Why gpt-4o-mini?  
- **Accurate + concise JSON outputs** (easy validation).  
- **Low latency** → smooth user experience.  
- **Affordable** for frequent quiz generations.  
- **Good balance** of correctness and speed for an MVP.  

---

## Demo Video  
[▶️ Click to watch the demo](https://github.com/SwathiKrish97/ai-quiz-builder/blob/main/ai-quiz-builder-demo.mp4)

---

## Author
**Swathi Ramesh**  
AI-Powered Quiz Builder
