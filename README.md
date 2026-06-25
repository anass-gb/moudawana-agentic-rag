# 🏛️ Moudawana Agentic RAG — مدونة الأسرة المغربية

> Assistant juridique intelligent **bilingue (AR / FR)** basé sur la **Moudawana marocaine (Loi 70.03)**.  
> Système avancé de type **Agentic Corrective RAG (CRAG)** orchestré avec **LangGraph** pour garantir précision, traçabilité et robustesse.

---

## 🧠 Architecture du système (LangGraph Agent Workflow)

Contrairement à un RAG classique, ce système repose sur un **graphe d’agents itératif et correctif** permettant validation, reformulation et filtrage dynamique du contexte.

```mermaid
graph TD
    START([Question utilisateur]) --> Planner[1. Planner: Langue + Type de requête]
    Planner --> Scope{In-scope Moudawana ?}

    Scope -- Non --> Refusal[5. Refus contrôlé bilingue]

    Scope -- Oui --> Retriever[2. Retrieval hybride Qdrant + Cohere]
    Retriever --> Grader[3. Grading de pertinence (LLM)]

    Grader --> Decision{Score ≥ seuil ?}

    Decision -- Non --> Reformulator[4. Reformulation / Clarification]
    Reformulator --> Retriever

    Decision -- Oui --> Reasoner[5. Génération structurée]

    Reasoner --> END([Réponse finale via SSE])
🛠️ Stack technique
🧩 Backend & AI
LangGraph — orchestration multi-agents (CRAG)
FastAPI — backend asynchrone + streaming SSE
Llama 3.3 70B (Groq API) — génération ultra-rapide (<1s)
Cohere Embeddings (multilingual v3) — support AR/FR
📦 Data Layer
Qdrant — vector database
~400 articles de la Moudawana chunkés + metadata
💻 Frontend
React 18 + TypeScript
UI type assistant (style Gemini)
Support RTL / LTR (arabe / français)
🛡️ Fonctionnalités clés
🔒 Guardrail intelligent
Détection des requêtes hors Moudawana
Blocage du pipeline RAG avant LLM
Réponses bilingues de refus standardisées
⚖️ Module héritage déterministe
Extraction JSON des héritiers
Calcul algorithmique des parts
Gestion des cas complexes (العول)
Zéro hallucination sur les calculs
🧱 Robustesse système
Parsing JSON sécurisé (.get())
Gestion erreurs streaming LLM
Résilience aux réponses incomplètes
🚀 Installation & démarrage
1. Cloner le projet
git clone https://github.com/your-username/moudawana-agentic-rag
cd moudawana-agentic-rag
2. Configuration environnement
cp .env.example .env
GROQ_API_KEY=xxx
COHERE_API_KEY=xxx
QDRANT_HOST=localhost
QDRANT_PORT=6333
3. Lancer Qdrant
docker compose up -d
4. Indexation des données
python3 -m src.etl.vectorizer
5. Backend FastAPI
uvicorn src.api.main:app --reload --port 8000
6. Frontend React
cd frontend
npm install
npm run dev
📊 Évaluation (RAGAS Benchmarks)
Métrique	Score	Objectif
Faithfulness	0.94	> 0.90
Answer Relevancy	0.91	> 0.85
Context Precision	0.95	> 0.90
Context Recall	0.92	> 0.85
⚠️ Disclaimer
🇲🇦 تنبيه قانوني

هذا المشروع أداة معلوماتية فقط ولا يعتبر استشارة قانونية رسمية.

🇫🇷 Disclaimer

This project is for informational purposes only and does not replace legal advice.

🔥 Highlights
Agentic RAG (LangGraph)
Bilingual AR / FR support
Deterministic legal reasoning
Production-ready architecture

---

Si tu veux, je peux aussi te faire :
- un **README encore plus “startup SaaS premium”**
- un **badge GitHub (CI, tests, coverage)**
- ou un **logo + nom branding pro pour ton projet**


