# Task 1 – NLP & Intent Understanding

This module is responsible for **understanding what the user means**.
It does NOT generate replies. It only analyzes user input.

This task provides:
- Intent classification
- Named Entity Recognition (NER)
- Context memory
- FastAPI service

Other tasks (response generation, sentiment, UI) depend on this module.

---

## 📌 What This Task Does

### 1️ Intent Classification
Predicts the user's intent using a fine-tuned Transformer model.

Supported intents include:
- greeting
- faq
- support
- complaint
- feedback
- goodbye
- order_status
- cancel_order
- refund_request
- human_handoff
- fallback

Model:
- `distilbert-base-uncased`
- Fine-tuned on custom intent dataset
- Confidence-based fallback handling

---

### 2️ Named Entity Recognition (NER)

Extracts structured information from user text:
- **person** → names
- **date** → dates & time expressions
- **product** → product / organization mentions
- **action** → domain actions (cancel, order, refund, etc.)

Implementation:
- spaCy (`en_core_web_sm`)
- Regex-based action detection
- Entity extraction is **disabled for greetings** to avoid false positives

---

### 3️ Context Memory

Maintains short-term conversational memory:
- Last **5 user messages**
- Last predicted intent
- Accumulated entities across turns

Used to:
- Maintain conversation flow
- Prevent intent reset on greetings
- Enable multi-turn conversations

---

### 4️ API Service

FastAPI-based service exposing:
- `/analyze` endpoint

Input:
```json
{
  "user_id": "string",
  "message": "string"
}
