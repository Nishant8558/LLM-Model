# Assignment README — MLOps, LangChain, LangGraph, Docker

This README explains each assignment task in a simple, task‑wise manner so you can run everything easily.

---

# **Task 1 — MLOps for LLMs**

### Goal

Monitor latency & throughput, call a scoring endpoint, and test with multiple prompts.

### Steps

1. **Set environment variables** (HF token, model name):

   ```bash
   export HF_TOKEN="your_hf_token"
   export MODEL_NAME="TeichAI/Qwen3-1.7B-Gemini-2.5-Flash-Lite-Preview-Distill"
   ```
2. **Run the app:**

   ```bash
   python app.py
   ```
3. **Call the endpoint:**

   ```bash
   curl -X POST http://localhost:8080/chat \
   -H "Content-Type: application/json" \
   -d '{"message":"Hello","conversation_id":"t1"}'
   ```
4. **Latency Monitoring Script (example):**

   ```python
   import time, requests
   prompts=["Hello","Explain AI","Write poem"]
   for p in prompts:
       t=time.time()
       r=requests.post("http://localhost:8080/chat",json={"message":p,"conversation_id":"bench"})
       print(p, round(time.time()-t,3), r.status_code)
   ```

---

# **Task 2 — LangChain & LangGraph Basics**

### Install

```bash
pip install langchain langgraph openai
```

### LangChain Basic Pipeline

* Use `PromptTemplate`
* Use a Chat Model
* Use `ConversationBufferMemory`

### LangGraph Basic Flow

Structure: **Input → LLM → Output**.

Your provided notebook already demonstrates these.

---

# **Task 3 — Minimal Chatbot with Memory (LangGraph)**

### Nodes in the Graph

* **User Input Node**
* **LLM Node** (local transformer in your case)
* **Memory Node** (stores history)
* **Output Node**

### Test Memory

Example:

1. `My name is Nishant.`
2. `What did I tell you?` → Bot should recall *“You said your name is Nishant.”*

---

# **Task 4 — Docker Basics**

### Steps

1. Install **Docker Desktop**.
2. Use Dockerfile:

   ```dockerfile
   FROM python:3.10-slim
   WORKDIR /app
   COPY . /app
   RUN pip install -r requirements.txt
   EXPOSE 8080
   CMD ["python", "app.py"]
   ```
3. Build image:

   ```bash
   docker build -t langgraph-chatbot .
   ```
4. Run container:

   ```bash
   docker run -p 8080:8080 --env HF_TOKEN="…" langgraph-chatbot
   ```

---

# **Task 5 — DAG with LangGraph**

### Objective

Route messages based on type:

* If user asks **calculation** → send to Calculator Node
* Else → LLM Node

### Example Logic

```python
if any(op in msg for op in "+-*/"):
    return "calculator"
else:
    return "llm"
```

---

# **Task 6 — Simple Prompt Engineering**

### Try 3 Variants:

1. **Direct Prompt:**
   "Explain AI in 2 lines."
2. **Role Prompt:**
   "You are a helpful tutor. Explain AI very clearly."
3. **Few-shot Prompt:**
   "Example: AI is X. Now explain Y in similar style."

### Document Results

* Which is clearer?
* Which gives longest/shortest output?
* Which respects format best?

---

# **Basic Run Instructions (Summary)**

```
pip install -r requirements.txt
python app.py
```

API:

* `/health` → server check
* `/chat` → send message

---

If you want, I can generate **requirements.txt**, **monitoring script**, **docker-compose**, or even prepare a final **submission PDF**.
