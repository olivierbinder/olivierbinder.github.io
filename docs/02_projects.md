# Applications data & IA que j'ai développées

Quatre projets de bout en bout, de la préparation des données au déploiement : modélisation, industrialisation MLOps, applications LLM et suivi en production.

---

## :lucide-clapperboard: &nbsp; Suggestion de films

Cette application enrichit la base de référence <span class="ctp-mauve">**IMDb**</span> de la fréquentation en salles et de <span class="ctp-mauve">**références cinéphiles soigneusement sélectionnées**</span> : notes des Cahiers du cinéma, sélections de certains festivals, listes personnelles... Elle permet d'explorer l'histoire du cinéma à travers ces regards, de découvrir des <span class="ctp-mauve">**familles de cinéastes**</span>, de recevoir des <span class="ctp-mauve">**recommandations personnalisées**</span> et d'être <span class="ctp-mauve">**conseillé en langage naturel sur tout le catalogue**</span>.

<div class="split" markdown>

<div class="split-media">
<img src="assets/ui_cine.jpg" alt="Interface utilisateur" style="width: 100%; border-radius: 12px;border: 1px solid var(--ctp-mauve);">
</div>

<div class="split-body" markdown>

- Enrichissement automatisé de la base : IMDb, références cinéphiles
- Exploration par cinéastes, régions, périodes, listes
- Comparaison critiques et public
- Profil cinéphile et recommandations à partir de films préférés
- Conseils en langage naturel par un agent conversationnel, justifiés par les signaux

</div>

</div>

*Lien vers l'application à venir*

<details markdown>
<summary><strong>Architecture de la solution</strong></summary>





```mermaid
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontSize": "16px"
  },
  "themeCSS": ".node rect, .node polygon, .node path, .node circle, .node ellipse { fill: var(--ctp-base); stroke: var(--ctp-surface1); stroke-width: 1.5px; }\n.nodeLabel { color: var(--ctp-text); fill: var(--ctp-text); }\n.cluster rect { fill: var(--ctp-mantle); stroke: var(--ctp-surface1); stroke-width: 1px; rx: 10px; ry: 10px; }\n.cluster-label, .cluster .nodeLabel, .cluster span, .cluster text { color: var(--ctp-text); fill: var(--ctp-subtext0); }\n.edgePath .path, .flowchart-link { stroke: var(--ctp-overlay1); stroke-width: 1.5px; }\n.marker, .arrowheadPath { fill: var(--ctp-overlay1); stroke: var(--ctp-overlay1); }\n.edgeLabel { background-color: transparent !important; color: var(--ctp-text) !important; }\n.labelBkg, .edgeLabel .labelBkg, .edgeLabel .label rect, .edgeLabel rect { background-color: var(--ctp-mantle) !important; fill: var(--ctp-mantle) !important; }\n.edgeLabel .label, .edgeLabel span, .edgeLabel text, .edgeLabel p { background-color: var(--ctp-mantle) !important; color: var(--ctp-text) !important; fill: var(--ctp-text) !important; }",
  "flowchart": {
    "nodeSpacing": 40,
    "rankSpacing": 50,
    "htmlLabels": true,
    "padding": 15,
    "curve": "basis",
    "subGraphTitleMargin": {"top": 20, "bottom": 20}
  }
}}%%
flowchart LR
    subgraph CONC["<b>Conception · Données & signaux</b>"]
        direction TB
        A[("<b>Sources</b><br/>IMDb · Wikidata · signaux cinéphiles")] --> B("<b>Ingestion & matching</b><br/>collecte · résolution en cascade · revue manuelle<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/dlt-4B4B8F?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/pandera-2C5F8A?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        B --> C("<b>Entrepôt & transformations</b><br/>ref_* · stg_* · cœur métier<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/DuckDB-FFF000?style=for-the-badge&logo=duckdb&logoColor=black' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/dbt-FF694B?style=for-the-badge&logo=dbt&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        C --> D("<b>Orchestration</b><br/>assets · planification · contrôles qualité<br/><div style='width:fit-content; margin:6px auto 0; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Dagster-4F43DD?style=for-the-badge&logo=dagster&logoColor=white' height='30' style='display:block;'/></div>")
    end

    subgraph DEPL["<b>Déploiement · Application & modèles</b>"]
        direction TB
        E("<b>Base cloud</b><br/>entrepôt partagé, même modèle de données<br/><div style='width:fit-content; margin:6px auto 0; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/MotherDuck-1B1B1B?style=for-the-badge&logoColor=white' height='30' style='display:block;'/></div>")
        E --> F("<b>Application Streamlit</b><br/>exploration · profils · pondérations<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Plotly-3F4F75?style=for-the-badge&logo=plotly&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        F --> G("<b>Familles & recommandations</b><br/>clustering des signaux · profil de goût<br/><div style='width:fit-content; margin:6px auto 0; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white' height='30' style='display:block;'/></div>")
        G --> H("<b>Agent conversationnel</b><br/>conseils fondés sur les films et les signaux<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Mistral%20AI-FA520F?style=for-the-badge&logo=mistralai&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pydantic%20AI-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
    end

    CONC ==>|publication de la base| DEPL
```
Côté données, dlt collecte IMDb, Wikidata et les signaux cinéphiles, puis les cas ambigus sont résolus par une cascade de règles et une revue manuelle. dbt transforme ensuite les référentiels bruts et les zones de staging en un cœur métier où tous les signaux partagent le même modèle : un agent (critique, festival, cinéaste…) applique une valeur à un film ou à un cinéaste. Dagster orchestre l'ensemble et contrôle la qualité à chaque exécution.

Côté application, l'entrepôt est publié sur MotherDuck et alimente l'interface Streamlit, qui permet d'explorer l'histoire du cinéma à travers ces regards. Un clustering des signaux dégage des familles de cinéastes, et la recommandation pondère les signaux selon le profil de l'utilisateur. Un agent conversationnel conseille des films en s'appuyant sur la base et sur ces signaux.

Pour plus de détails, retrouvez la documentation technique (à venir) et le code (à venir) du projet.


</details>

---

## :lucide-wheat: &nbsp; Prévision agricole

À destination des acteurs agricoles, cette application <span class="ctp-mauve">**prédit le rendement d'une culture**</span> et <span class="ctp-mauve">**classe les cultures les plus adaptées**</span> dans un contexte donné (zone, année, pluviométrie, pesticides, température). Entraîné sur des données de la *Food and Agriculture Organization* des années passées, le modèle prédit l'année suivante avec une très bonne précision.


<div class="split" markdown>

<div class="split-media">
<img src="assets/ui_agri.jpg" alt="Interface utilisateur" style="width: 100%; border-radius: 12px;border: 1px solid var(--ctp-mauve);">
</div>

<div class="split-body" markdown>

**Parcours utilisateur**

- Choix de la zone, de la culture et de l'année
- Réglage des conditions (pluviométrie, pesticides, température)
- Prédiction du rendement de la culture sélectionnée
- Classement de toutes les cultures pour le même contexte

</div>

</div>

[*Lien vers l'application*](https://agri-ui-28873275232.europe-west1.run.app) (premier chargement lent)

<details markdown>
<summary><strong>Architecture de la solution</strong></summary>





```mermaid
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontSize": "16px"
  },
  "themeCSS": ".node rect, .node polygon, .node path, .node circle, .node ellipse { fill: var(--ctp-base); stroke: var(--ctp-surface1); stroke-width: 1.5px; }\n.nodeLabel { color: var(--ctp-text); fill: var(--ctp-text); }\n.cluster rect { fill: var(--ctp-mantle); stroke: var(--ctp-surface1); stroke-width: 1px; rx: 10px; ry: 10px; }\n.cluster-label, .cluster .nodeLabel, .cluster span, .cluster text { color: var(--ctp-text); fill: var(--ctp-subtext0); }\n.edgePath .path, .flowchart-link { stroke: var(--ctp-overlay1); stroke-width: 1.5px; }\n.marker, .arrowheadPath { fill: var(--ctp-overlay1); stroke: var(--ctp-overlay1); }\n.edgeLabel { background-color: transparent !important; color: var(--ctp-text) !important; }\n.labelBkg, .edgeLabel .labelBkg, .edgeLabel .label rect, .edgeLabel rect { background-color: var(--ctp-mantle) !important; fill: var(--ctp-mantle) !important; }\n.edgeLabel .label, .edgeLabel span, .edgeLabel text, .edgeLabel p { background-color: var(--ctp-mantle) !important; color: var(--ctp-text) !important; fill: var(--ctp-text) !important; }",
  "flowchart": {
    "nodeSpacing": 40,
    "rankSpacing": 50,
    "htmlLabels": true,
    "padding": 15,
    "curve": "basis",
    "subGraphTitleMargin": {"top": 20, "bottom": 20}
  }
}}%%
flowchart LR
    subgraph CONC["<b>Conception · Data Science</b>"]
        direction TB
        A[("<b>Données FAO</b><br/>rendements · pluviométrie · température")] --> B("<b>Feature engineering</b><br/> Variables métier · encodage · schémas validés <br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        B --> C("<b>Entraînement</b><br/> Suivi des expériences · tuning sur fenêtre glissante · explicabilité<br/><div style='display:flex; flex-wrap:wrap; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/MLflow-0194E2?style=for-the-badge&logo=mlflow&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/XGBoost-006ACC?style=for-the-badge&logo=xgboost&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/SHAP-FF6F00?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
    end

    subgraph DEPL["<b>Déploiement · MLOps</b>"]
        direction TB
        E("<b>CI/CD</b><br/>tests · build · publication<br/><div style='display:flex; flex-wrap:wrap; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/pytest-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Google%20Cloud-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        E --> F("<b>Modèle final exposé en API</b><br/>/predict · /recommend<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pydantic-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        F --> G("<b>Interface utilisateur</b><br/>prédiction · classement des cultures<br/><div style='width:fit-content; margin:6px auto 0; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Gradio-FF7C00?style=for-the-badge&logo=gradio&logoColor=white' height='30' style='display:block;'/></div>")
    end

    CONC ==>|modèle Champion| DEPL

```
Côté conception, le feature engineering enrichit les données FAO de trois variables métier et encode la zone et la culture par un `TargetEncoder`, avec des schémas contrôlés par `pandera`. Le split par année (1990-2012 en entraînement, 2013 en holdout) reproduit le cas d'usage réel : réentraîner chaque année et prédire la suivante. Après tuning sur fenêtre glissante et suivi dans MLflow, le modèle retenu est un XGBoost (R² ≈ 0,96 sur le holdout), promu par l'alias `Champion` et expliqué par SHAP.

Côté déploiement, la chaîne CI/CD teste le code, construit deux images Docker et les déploie sur Google Cloud Run. L'API FastAPI embarque le modèle et l'expose, avec sa [documentation Swagger](https://agri-api-28873275232.europe-west1.run.app/docs), à une interface Gradio légère.

Pour plus de détails, consultez la [documentation technique](https://olivierbinder.github.io/agri/) et le [code](https://github.com/olivierbinder/agri) du projet.


</details>

---

## :lucide-whistle: &nbsp; Assistant NBA
À destination des passionnés de basket, cette application est un <span class="ctp-mauve">**chatbot Mistral enrichi de données externes**</span> (discussions Reddit de fans et tableaux Excel de statistiques) pour répondre à des <span class="ctp-mauve">**questions pointues sur la saison NBA**</span>. Une évaluation sur un jeu de questions de référence a permis de valider l'amélioration des réponses.


<div class="split" markdown>

<div class="split-media">
<img src="assets/ui_nba.jpg" alt="Interface utilisateur" style="width: 100%; border-radius: 12px;border: 1px solid var(--ctp-mauve);">
</div>

<div class="split-body" markdown>

- Réponse donnée avec ses sources et la stratégie mobilisée (FAISS / SQL)
- Niveau de confiance et alerte contexte insuffisant
- Scores, latence et tokens consommés
- Consultation des rapports d'évaluation

</div>

</div>

[*Lien vers l'application*](https://llmeval-nba.streamlit.app/) (mot de passe : `nbademo` / premier chargement lent)

<details markdown>
<summary><strong>Architecture de la solution</strong></summary>





```mermaid
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontSize": "16px"
  },
  "themeCSS": ".node rect, .node polygon, .node path, .node circle, .node ellipse { fill: var(--ctp-base); stroke: var(--ctp-surface1); stroke-width: 1.5px; }\n.nodeLabel { color: var(--ctp-text); fill: var(--ctp-text); }\n.cluster rect { fill: var(--ctp-mantle); stroke: var(--ctp-surface1); stroke-width: 1px; rx: 10px; ry: 10px; }\n.cluster-label, .cluster .nodeLabel, .cluster span, .cluster text { color: var(--ctp-text); fill: var(--ctp-subtext0); }\n.edgePath .path, .flowchart-link { stroke: var(--ctp-overlay1); stroke-width: 1.5px; }\n.marker, .arrowheadPath { fill: var(--ctp-overlay1); stroke: var(--ctp-overlay1); }\n.edgeLabel { background-color: transparent !important; color: var(--ctp-text) !important; }\n.labelBkg, .edgeLabel .labelBkg, .edgeLabel .label rect, .edgeLabel rect { background-color: var(--ctp-mantle) !important; fill: var(--ctp-mantle) !important; }\n.edgeLabel .label, .edgeLabel span, .edgeLabel text, .edgeLabel p { background-color: var(--ctp-mantle) !important; color: var(--ctp-text) !important; fill: var(--ctp-text) !important; }",
  "flowchart": {
    "nodeSpacing": 40,
    "rankSpacing": 50,
    "htmlLabels": true,
    "padding": 15,
    "curve": "basis",
    "subGraphTitleMargin": {"top": 20, "bottom": 20}
  }
}}%%
flowchart LR
    subgraph CONC["<b>Conception · Données & RAG</b>"]
        direction TB
        A[("<b>Sources NBA</b><br/>discussions Reddit · statistiques Excel")] --> B("<b>Ingestion & indexation</b><br/>chunking · embeddings · base SQL<br/><div style='display:flex; flex-wrap:wrap; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/PyMuPDF-EF3939?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/FAISS-0467DF?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        B --> C("<b>Agent RAG hybride</b><br/>recherche vectorielle · agent SQL en repli<br/><div style='display:flex; flex-wrap:wrap; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Mistral%20AI-FA520F?style=for-the-badge&logo=mistralai&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pydantic%20AI-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        C --> D("<b>Évaluation reproductible</b><br/>jeu de cas · métriques · rapports<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/pydantic--evals-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/RAGAS-9C27B0?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
    end

    subgraph DEPL["<b>Déploiement · Application</b>"]
        direction TB
        H("<b>Artefacts versionnés</b><br/>index · chunks · base (≈ 2 Mo)<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Git-181717?style=for-the-badge&logo=git&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        H --> I("<b>Publication</b><br/>démarrage sans ré-ingestion<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Streamlit%20Cloud-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        I --> E("<b>Application Streamlit</b><br/>questions · sources · rapports<br/>mot de passe · quota par session<br/><div style='width:fit-content; margin:6px auto 0; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white' height='30' style='display:block;'/></div>")
        E -.->|traces| G("<b>Observabilité</b><br/>traces · latence · coûts<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Logfire-7B2BF9?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/OpenTelemetry-000000?style=for-the-badge&logo=opentelemetry&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
    end

    CONC ==>|index + base| DEPL

```
Côté conception, l'ingestion transforme les discussions Reddit et les statistiques Excel en un index vectoriel FAISS et une base SQLite. L'agent oriente chaque question vers la recherche vectorielle ou vers le SQL, puis renvoie une réponse structurée avec ses sources et son niveau de confiance. Évalué avec RAGAS sur un jeu de questions de référence, le pipeline hybride améliore nettement la fidélité et le rappel par rapport à la recherche vectorielle seule.

Côté déploiement, seuls les artefacts légers sont versionnés, ce qui permet à l'application Streamlit de démarrer sans ré-ingestion sur Streamlit Community Cloud, avec un accès protégé par mot de passe et un quota de questions par session. Les traces, la latence et les coûts sont suivis dans Logfire.

Pour plus de détails, consultez la [documentation technique](https://olivierbinder.github.io/llmeval/) et le [code](https://github.com/olivierbinder/llmeval) du projet.


</details>

---

## :lucide-piggy-bank: &nbsp; Scoring crédit


À destination d'organismes de crédit, cette application prédit le <span class="ctp-mauve">**risque de défaut d'un demandeur**</span> à l'aide d'un modèle de Machine Learning entraîné sur l'historique de clients ayant honoré ou non leurs remboursements.


<div class="split" markdown>

<div class="split-media">
<img src="assets/ui_credit_scoring.jpg" alt="Interface utilisateur" style="width: 100%; border-radius: 12px;border: 1px solid var(--ctp-mauve);">
</div>

<div class="split-body" markdown>


- Recherche d'un client
- Positionnement par rapport à la clientèle
- Prédiction du risque de défaut
- Suivi de la dérive des données et du coût d'exécution

</div>

</div>

[*Lien vers l'application*](https://huggingface.co/spaces/OlivierBinder/credit-scoring) (premier chargement lent)

<details markdown>
<summary><strong>Architecture de la solution</strong></summary>





```mermaid
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontSize": "16px"
  },
  "themeCSS": ".node rect, .node polygon, .node path, .node circle, .node ellipse { fill: var(--ctp-base); stroke: var(--ctp-surface1); stroke-width: 1.5px; }\n.nodeLabel { color: var(--ctp-text); fill: var(--ctp-text); }\n.cluster rect { fill: var(--ctp-mantle); stroke: var(--ctp-surface1); stroke-width: 1px; rx: 10px; ry: 10px; }\n.cluster-label, .cluster .nodeLabel, .cluster span, .cluster text { color: var(--ctp-text); fill: var(--ctp-subtext0); }\n.edgePath .path, .flowchart-link { stroke: var(--ctp-overlay1); stroke-width: 1.5px; }\n.marker, .arrowheadPath { fill: var(--ctp-overlay1); stroke: var(--ctp-overlay1); }\n.edgeLabel { background-color: transparent !important; color: var(--ctp-text) !important; }\n.labelBkg, .edgeLabel .labelBkg, .edgeLabel .label rect, .edgeLabel rect { background-color: var(--ctp-mantle) !important; fill: var(--ctp-mantle) !important; }\n.edgeLabel .label, .edgeLabel span, .edgeLabel text, .edgeLabel p { background-color: var(--ctp-mantle) !important; color: var(--ctp-text) !important; fill: var(--ctp-text) !important; }",
  "flowchart": {
    "nodeSpacing": 40,
    "rankSpacing": 50,
    "htmlLabels": true,
    "padding": 15,
    "curve": "basis",
    "subGraphTitleMargin": {"top": 20, "bottom": 20}
  }
}}%%
flowchart LR
    subgraph CONC["<b>Conception · Data Science</b>"]
        direction TB
        A[("<b>Données Home Credit</b><br/>8 tables · 58 M lignes")] --> B("<b>Feature engineering</b><br/> Pipelines créant 600 variables <br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        B --> C("<b>Entraînement</b><br/> Suivi des expériences · évaluation et optimisation des modèles · explicabilité<br/><div style='display:flex; flex-wrap:wrap; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/MLflow-0194E2?style=for-the-badge&logo=mlflow&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/LightGBM-02569B?style=for-the-badge&logo=microsoft&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/SHAP-FF6F00?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        C --> SPACER["<div style='height:104px'></div>"]:::spacer
    end

    subgraph DEPL["<b>Déploiement · MLOps</b>"]
        direction TB
        G("<b>CI/CD</b><br/>tests · build · publication<br/><div style='display:flex; flex-wrap:wrap; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/pytest-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black' height='30' style='display:block; max-width:none;'/></div></div>")
        G --> D("<b>Modèle final exposé en API</b><br/>/predict · /lookup · /reference<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pydantic-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        D -.->|logs API| M("<b>Monitoring & dérive</b><br/>qualité · latence · coût runtime<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Evidently-ED0500?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/psutil-3776AB?style=for-the-badge&logo=python&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        D --> E("<b>Interface utilisateur</b><br/>prédiction · surveillance<br/><div style='width:fit-content; margin:6px auto 0; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white' height='30' style='display:block;'/></div>")
        M --> E
    end

    CONC ==>|modèle validé| DEPL

    classDef spacer fill:transparent,stroke:transparent,color:transparent
    linkStyle 2 stroke:none,stroke-width:0px,marker-end:none

```
Côté conception, le feature engineering croise les 8 tables Home Credit et leurs 58 M de lignes (historiques de demandes de crédit, échéances de remboursement, encours mensuels) pour produire 600 variables. Après benchmark de plusieurs modèles, sélection de variables et optimisation, suivis dans MLflow, le modèle retenu est un LightGBM, dont les prédictions sont expliquées par SHAP.

Côté déploiement, la chaîne CI/CD teste le code, convertit le modèle au format ONNX, conteneurise l'API et la publie sur Hugging Face Spaces. L'API FastAPI expose le modèle à une interface Streamlit, qui intègre le suivi de la dérive des données et du coût d'exécution.

Pour plus de détails, retrouvez la [documentation technique](https://olivierbinder.github.io/Credit_scoring/) et le [code](https://github.com/olivierbinder/Credit_scoring) du projet.


</details>

