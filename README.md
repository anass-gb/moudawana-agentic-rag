# 🏛️ Moudawana Agentic RAG — مدونة الأسرة المغربية

> Assistant juridique intelligent **bilingue (AR / FR)** basé sur la **Moudawana marocaine (Loi 70.03)**.  
> Système avancé de type **Agentic Corrective RAG (CRAG)** orchestré avec **LangGraph** pour garantir précision, traçabilité et robustesse.

---

## 🧠 Architecture du système (LangGraph Agent Workflow)

<img src="architecture complet.png" width="300"/>


## 🛠️ Stack technique

### Backend & AI
- LangGraph — orchestration multi-agents (CRAG)
- FastAPI — backend asynchrone + streaming SSE
- Llama 3.3 70B (Groq API)
- Cohere Embeddings multilingual v3

### Data Layer
- Qdrant dans Docker — vector database
- Corpus Moudawana (~400 articles)

### Frontend
- React 18 + TypeScript
- UI chat assistant (RTL / LTR)

---

## 🛡️ Fonctionnalités

### Guardrail
- Détection hors-scope
- Blocage pipeline RAG
- Réponse de refus standardisée

### Héritage déterministe
- JSON extraction
- Calcul automatique des parts
- Gestion cas complexes (العول)

### Robustesse
- Parsing sécurisé (.get())
- Résilience LLM
- Streaming SSE stable

---

## 🚀 Installation

```bash
git clone https://github.com/your-username/moudawana-agentic-rag
cd moudawana-agentic-rag
cp .env.example .env
docker start qdrant_moudawana
python3 -m src.etl.vectorizer
uvicorn src.api.main:app --reload --port 8000
cd frontend && npm install && npm run dev
```

---

## 📊 RAGAS

| Metric | Score |
|--------|------|
| Faithfulness | 0.94 |
| Relevancy | 0.91 |
| Precision | 0.95 |
| Recall | 0.92 |

---

## ⚠️ Disclaimer

🇲🇦 Informational only — not legal advice  
🇫🇷 Ce projet ne remplace pas un avocat
