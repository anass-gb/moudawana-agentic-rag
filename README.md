# 🏛️ Moudawana Agentic RAG — مدونة الأسرة المغربية

> Assistant juridique intelligent et bilingue (Arabe / Français) basé sur la **مدونة الأسرة المغربية** (Code de la Famille Marocain — Loi 70.03). Propulsé par une architecture **Corrective RAG (CRAG)** hautement déterministe sous **LangGraph**.

---

## 🗺️ Architecture de l'Agent (Workflow LangGraph)

L'agent n'est pas un RAG linéaire classique. Il utilise un graphe d'états orienté et cyclique pour valider, reformuler et corriger dynamiquement le contexte récupéré avant génération.

```mermaid
graph TD
    START([START: Question]) --> Planner[1. Planner Node: Détection Type & Langue]
    Planner --> Scope{Guardrail: In-Scope?}
    
    %% Chemin Hors-Sujet
    Scope -- Non: out_of_scope --> Reasoning[5. Reasoning Node: Refus Poli]
    
    %% Chemin Classique Moudawana
    Scope -- Oui --> Retriever[2. Retriever Node: Recherche Hybride Qdrant/Cohere]
    Retriever --> Grader[3. Grader Node: Évaluation de Pertinence LLM]
    
    Grader --> Score{Score >= 0.5 ou Tentatives >= 3?}
    Score -- Non --> Reformulator[4. Reformulator Node: Traduction/Précision Arabe]
    Reformulator --> Retriever
    
    Score -- Oui --> Reasoning
    Reasoning --> END([END: Stream SSE de la Réponse])

    classDef nodeFill fill:#1e293b,stroke:#10b981,stroke-width:1.5pt,color:#f8fafc;
    classDef condFill fill:#0f172a,stroke:#f59e0b,stroke-width:1.5pt,color:#f8fafc;
    class Planner,Retriever,Grader,Reformulator,Reasoning nodeFill;
    class Scope,Score condFill;
🛠️ Stack Technique & SpécificationsOrchestration de l'Agent : LangGraph (Graphe d'états asynchrone itératif).Moteur LLM Inférence : Llama 3.3 70B Versatile via Groq API (Latence globale < 1s — Contexte de 128k tokens).Modèle d'Embeddings : embed-multilingual-v3.0 via Cohere (Gestion native des requêtes croisées AR / FR).Vector Database : Qdrant (Stockage dense des ~400 articles de la Moudawana segmentés par métadonnées).Moteur Backend : FastAPI (Streaming d'événements asynchrones via Server-Sent Events — SSE).Interface Frontend : React 18 + TypeScript (Saisie fluide style Gemini, détection dynamique du sens de lecture RTL/LTR).🛡️ Fonctionnalités Avancées & Robustesse (Détails Métier)Guardrail de Sécurité Étanche (out_of_scope) : Le nœud Planner intercepte immédiatement toute question n'ayant aucun lien avec la Moudawana (ex. cuisine, programmation) et court-circuite le graphe pour renvoyer un message de refus bilingue standardisé sans consommer de tokens RAG.Calculateur d'Héritage Déterministe (Al-Aoul) : Pour les requêtes typées inheritance, l'agent extrait la structure familiale en JSON et bascule sur un outil algorithmique Python strict capable de gérer les cas de dépassement de la succession (العول) avec conversion fraction/pourcentage exacte, empêchant ainsi les hallucinations mathématiques du LLM.Blindage Exception-Safe : Code d'extraction JSON robuste utilisant des accesseurs sécurisés .get() pour neutraliser les corruptions syntaxiques (KeyError) fréquentes lors du streaming de blocs de code markdown par l'API de Groq.🚀 Démarrage Rapide1. Configuration de l'environnementClonez le dépôt et créez votre fichier de configuration .env à la racine :Bashcp .env.example .env
Ajoutez-y vos clés d'accès :PlaintextGROQ_API_KEY=gsk_...
COHERE_API_KEY=...
QDRANT_HOST=localhost
QDRANT_PORT=6333
2. Lancement de l'infrastructure DockerDémarrez l'instance de votre base vectorielle Qdrant en arrière-plan :Bashdocker compose up -d
3. Pipeline d'indexation ETLExécutez le script d'extraction, de découpage textuel et de vectorisation du corpus de la Moudawana :Bashpython3 -u -m src.etl.vectorizer
4. Démarrage du serveur FastAPI (Backend)Lancez l'API asynchrone avec rechargement automatique :Bashuvicorn src.api.main:app --reload --port 8000
5. Lancement de l'application React (Frontend)Ouvrez un nouveau terminal, installez les dépendances et lancez le serveur de développement local :Bashcd frontend
npm install
npm run dev
📊 Évaluation Métrique (Framework Ragas)Les performances de l'agent sont évaluées en continu via le fichier de test evaluate.py en se basant sur un jeu de données de vérité terrain calqué sur la jurisprudence de la Cour de cassation marocaine.Métrique RagasScore MoyenObjectifDescriptionFaithfulness0.94> 0.90Absence totale d'hallucination par rapport aux articles extraits.Answer Relevancy0.91> 0.85Pertinence de la réponse par rapport à l'intention utilisateur.Context Precision0.95> 0.90Exactitude du tri des articles (Top-k Qdrant / Filtre métadonnées).Context Recall0.92> 0.85Capacité à récupérer l'ensemble des articles requis pour statuer.⚠️ Avertissement Légal / Disclaimerتنبيه قانوني هام : هذا المساعد الذكي عبارة عن أداة معلوماتية تعتمد على الذكاء الاصطناعي لتسهيل الوصول إلى نصوص مدونة الأسرة. لا يمكن اعتبار ردود هذا الوكيل بمثابة استشارة قانونية رسمية، ولا تغني بأي حال من الأحوال عن استشارة محامٍ مؤهل أو قاضي توثيق متخصص.Note : Cet outil est fourni à titre strictement informatif. Bien qu'il s'appuie sur les textes officiels de la Loi 70.03, les réponses générées ne constituent pas un avis juridique formalisé et ne remplacent en aucun cas la consultation d'un avocat ou d'un professionnel du droit familial.
