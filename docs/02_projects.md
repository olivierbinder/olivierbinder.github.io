# Applications data & IA que j'ai développées

Quatre projets menés de la conception au déploiement : modélisation supervisée, industrialisation MLOps, entrepôts de données et applications LLM. Pour chacun, cette page présente l'**architecture cible** et les **technologies mobilisées** ; le détail des compétences est regroupé dans la page [Stack](03_stack.md).

Certains projets sont encore en construction : l'architecture décrite est alors l'architecture visée, pas un état livré.

| Projet | Type | En une phrase | Démo |
| :--- | :--- | :--- | :--- |
| **Cine** | Projet personnel, en construction | Base cinéma personnelle enrichie de signaux cinéphiles, puis profils pondérés et recommandations | *à venir* |
| **Agri** | Bootcamp MLOps, déployé | Prédiction et recommandation de rendements agricoles, de l'entraînement au service en ligne | [App](https://agri-ui-28873275232.europe-west1.run.app) |
| **Crédit** | Bootcamp MLOps, déployé | Prédiction du risque de défaut de remboursement et scoring manipulable par un métier | [App](https://huggingface.co/spaces/OlivierBinder/credit-scoring) |
| **Basket (llmeval)** | Bootcamp LLMOps, déployé | Assistant NBA augmenté par un RAG hybride, et mesure reproductible de ses réponses | [App](https://llmeval-nba.streamlit.app/) |

---

## :lucide-clapperboard: &nbsp; App Cinéma

**Projet personnel — base cinéma enrichie et recommandations personnelles**

Construire et explorer une base cinéma issue d'IMDb, enrichie par des prescriptions cinéphiles (critiques, cinéastes, revues, institutions, festivals) et des mesures d'audience, puis pondérer ces signaux selon des profils personnels pour produire des recommandations. Un projet à la croisée de ma pratique du cinéma et de la donnée.

### Architecture cible

```mermaid
flowchart LR
    subgraph SRC["Sources"]
        direction TB
        IMDB["IMDb (TSV bruts)"]
        WEB["Festivals, revues,<br/>critiques (web, PDF, images)"]
        WIKI["Wikidata (SPARQL)"]
        BOX["Box-office France"]
    end

    subgraph LOCAL["Pipelines locaux"]
        direction TB
        ING["collect → stage → match → load<br/>revue manuelle des correspondances"]
        LLMDATA["Extraction assistée<br/>par LLM"]
        DUCK[("DuckDB local<br/>ref_* / stg_* / cœur métier")]
    end

    DBT["dbt<br/>modèles, tests, lineage"]
    DAG["Dagster<br/>orchestration, lineage"]
    MD[("MotherDuck<br/>warehouse cloud")]
    APP["App Streamlit Cloud<br/>consultation, profils, recommandations"]

    IMDB --> ING
    WEB --> ING
    WIKI --> ING
    BOX --> ING
    LLMDATA --> ING
    ING --> DUCK
    DUCK --> DBT
    DBT --> MD
    MD --> APP
    DAG -.-> ING
    DAG -.-> DBT
```

Trois processus semi-automatisés structurent le remplissage : **référence** (import IMDb et calcul des colonnes de recherche), **signaux** (collecte d'un signal puis appariement en cascade aux films et cinéastes) et **enrichissement** (métadonnées des cinéastes via Wikidata). Chaque étape de correspondance passe par une zone de staging relue manuellement avant intégration au cœur métier.

### Stack

| Domaine | Brique | Rôle |
| :--- | :--- | :--- |
| Data & Analytics | IMDb datasets, `DuckDB` | Référentiels bruts (`ref_*`) puis entrepôt local (≈1,9 Go, 78 M lignes de contributeurs) |
| Data & Analytics | `Selenium`, `Playwright`, `BeautifulSoup` | Collecte des palmarès, sélections et classements de critiques |
| Data & Analytics | `PyMuPDF`, `pdfplumber`, `img2table`, `pytesseract` / `ocrmac`, `OpenCV` | Extraction des revues et documents scannés (OCR, tableaux) |
| Data & Analytics | SPARQL Wikidata, `mwparserfromhell` | Enrichissement des cinéastes : nationalités, genres, identifiants |
| Data & Analytics | `pandas`, `Polars`, `PyArrow` | Préparation, contrôles de volumétrie, exports |
| Data & Analytics | `pandera`, `Pydantic` | Garde-frontière des tables de staging (schémas de validation) |
| Data & Analytics | **dbt** | Modélisation en couches (`ref` / `stg` / cœur), tests et lineage |
| Data & Analytics | **Dagster** | Orchestration, planification et traçabilité des pipelines |
| Data & Analytics | **MotherDuck** | Warehouse cloud partagé, base de l'application publiée |
| Machine Learning | Cascades de matching (`MATCH_NAME`, `MATCH_TITLE`) et normalisation de texte | Appariement agents / œuvres / cinéastes avec revue des cas ambigus |
| Machine Learning | `scikit-learn`, `scipy` | Similarités films et cinéastes, familles de films |
| LLM & GenAI | `google-genai` (Gemini 2.5 Flash) | Contrôle assisté des grilles de notes extraites de documents scannés |
| Interfaces | `Streamlit`, `Altair`, `Plotly` | Application de consultation (filmographies, pays, couverture, matching, analyses) |
| Industrialisation | `uv`, `just`, `ruff`, `ty`, `pytest`, pre-commit, Commitizen | Outillage de développement et de qualité |
| Industrialisation | GitHub Actions, Docker, Zensical | CI, conteneurisation, documentation technique |

### Roadmap

| Phase | Contenu |
| :--- | :--- |
| **MVP** | Référentiels, enrichissement, ingestion, matching, sauvegardes et consultation |
| **Évolution** | Profils cinéphiles, pondérations et exploration avancée |
| **Dernière brique** | Recommandations automatiques basées sur les profils |
| **Cible technique** | Passage à un warehouse cloud (MotherDuck) et publication de l'app (Streamlit Cloud) |

### Liens

- **Démo** : *à venir* (app Streamlit Cloud, dépôt public à publier)
- **Code** : *à venir* (dépôt local, pas encore de dépôt distant)
- **Doc technique** : *à venir* (documentation Zensical des trois processus, construite localement)

---

## :lucide-wheat: &nbsp; App Agriculture

À destination des acteurs agricoles, cette application prédit le **rendement d'une culture** à partir de données climatiques et agricoles — pluviométrie, pesticides, température — issues du dataset FAO, et **classe toutes les cultures** pour un contexte donné. [**Lien vers l'application**](https://agri-ui-28873275232.europe-west1.run.app) (cold start)

<div style="display: flex; gap: 1rem; align-items: flex-start;" markdown>

<div style="flex: 0 0 50%;">
<img src="assets/ui_agri.jpg" alt="Interface utilisateur" style="width: 100%; border-radius: 12px;">
</div>

<div style="flex: 1;" markdown>

**Parcours utilisateur**

- Choix de la zone, de la culture et de l'année
- Réglage des conditions (pluviométrie, pesticides, température) ou reprise des conditions réellement enregistrées
- Prédiction du rendement de la culture sélectionnée
- Classement de toutes les cultures par score relatif, pour le même contexte
- Onglet par usage : prédiction ou recommandation, servis par l'API

</div>

</div>


<details markdown>
<summary><strong>Architecture de la solution</strong></summary>

Côté conception, le pipeline part du dataset FAO et d'un **split par année** — 1990-2012 pour l'entraînement, 2013 en holdout — pour coller au cas d'usage réel : réentraîner chaque année et prédire l'année suivante. Le feature engineering ajoute trois variables métier (interaction pluie/température, efficacité de la pluie, écart à la température optimale de la culture), `Area` et `Item` sont encodés par un `TargetEncoder` validé en 5 folds, et les schémas d'entrée, de cible et de sortie sont contrôlés par `pandera`. Le tuning (`RandomizedSearchCV`, 30 combinaisons) s'appuie sur une validation glissante « 5 ans d'entraînement / 1 an de test » déroulée sur tout l'historique (17 folds) : la dérive temporelle des rendements rend les fenêtres courtes plus pertinentes, une fenêtre de 5 ans minimisant le RMSE (20 652 ± 1 306) contre 23 826 sur 20 ans. Le modèle final (`XGBoost`, baseline `RandomForest`) est entraîné sur les 5 dernières années, signé, enregistré dans le **MLflow Model Registry** puis promu par l'alias `Champion` (meilleur `R2_test`) ; importances et valeurs `SHAP` documentent ses décisions. Dernier run : `RMSE_test` ≈ 19 653 et `R2_test` ≈ 0,959 sur le holdout 2013.

Côté déploiement, deux images Docker indépendantes sont construites par la CI puis déployées sur **Google Cloud Run** (`europe-west1`, scale-to-zero) : `agri-api` embarque le modèle `Champion` (bundle exporté du registre, aucun registre MLflow au runtime) et expose `/predict` et `/recommend` avec sa [documentation Swagger](https://agri-api-28873275232.europe-west1.run.app/docs) ; `agri-ui` reste volontairement légère (Gradio seul, sans stack ML) et interroge l'API via `API_URL`. Déploiement par digest, authentification sans clé (Workload Identity Federation) et notification d'échec en fin de pipeline.

Pour plus de détails, consultez la [documentation technique](https://olivierbinder.github.io/agri/) et le [code](https://github.com/olivierbinder/agri) du projet.


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
        A(("<b>Données FAO</b><br/>rendements · pluviométrie<br/>pesticides · température")) --> B("<b>Feature engineering</b><br/>3 variables métier · TargetEncoder 5 folds<br/>schémas validés par pandera<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        B --> C("<b>Entraînement & tuning</b><br/>RandomizedSearchCV · fenêtre glissante 5 ans / 1 an<br/>XGBoost vs RandomForest<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/XGBoost-006ACC?style=for-the-badge&logo=xgboost&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/MLflow-0194E2?style=for-the-badge&logo=mlflow&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        C --> D("<b>Promotion & explicabilité</b><br/>alias Champion (meilleur R2) · importances et valeurs SHAP<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/MLflow-0194E2?style=for-the-badge&logo=mlflow&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/SHAP-FF6F00?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
    end

    subgraph DEPL["<b>Déploiement · MLOps</b>"]
        direction TB
        E("<b>CI/CD</b><br/>tests ≥ 80 % · build · publication<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/pytest-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        E --> F("<b>Images Docker Hub</b><br/>agri-api (modèle embarqué) · agri-ui (Gradio seul)<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Docker%20Hub-2496ED?style=for-the-badge&logo=docker&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        F --> G("<b>Modèle final exposé en API</b><br/>/predict · /recommend · Swagger<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pydantic-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        G --> H("<b>Interface utilisateur</b><br/>prédiction · classement des cultures<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Gradio-FF7C00?style=for-the-badge&logo=gradio&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Google%20Cloud-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
    end

    CONC ==>|modèle Champion| DEPL
```

</details>

---

## :lucide-piggy-bank: &nbsp; App Crédit


À destination d'organismes de crédit, cette application permet de prédire le **risque de défaut d'un demandeur** grace à un modèle de Machine Learning entraîné sur les données historiques des clients ayant honoré ou non leur remboursements. [**Lien vers l'application**](https://huggingface.co/spaces/OlivierBinder/credit-scoring) (cold start)



<div style="display: flex; gap: 1rem; align-items: flex-start;" markdown>

<div style="flex: 0 0 50%;">
<img src="assets/ui_credit_scoring.jpg" alt="Interface utilisateur" style="width: 100%; border-radius: 12px;">
</div>

<div style="flex: 1;" markdown>

**Parcours utilisateur**

- Chargement des données clients
- Visualisation par rapport aux autres clients
- Prédiction du risque de défaut
- Surveillance de la dérive du modèle et des performances de calcul

</div>

</div>


<details markdown>
<summary><strong>Architecture de la solution</strong></summary>

Côté conception, le feature engineering croise les 8 tables Home Credit et leurs 58 M de lignes —  historiques des demandes de crédits, échéances de remboursement et encours mensuels — pour aboutir à 600 variables, utilisées pour le benchmark des modèles. Les étapes de sélection de variables, d'évaluation et d'optimisation, suivies dans MLflow, ont permis d'obtenir notre modèle cible, dont les prédictions sont expliquées par SHAP.

Côté déploiement, la chaîne CI/CD teste, conteneurise le modèle optimisé au format ONNX et publie l'application sur Hugging Face Spaces : le modèle retenu est exposé via une API FastAPI, interrogée par une interface Streamlit qui intègre le suivi de la dérive des données et du coût d'exécution.

Pour plus de détails, consultez la [documentation technique](https://olivierbinder.github.io/Credit_scoring/) et le [code](https://github.com/olivierbinder/Credit_scoring) du projet.


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

</details>

---



## :lucide-whistle: &nbsp; App Basket

À destination des passionnés de NBA, cette application répond à des **questions pointues sur la saison** à partir de sources hétérogènes — discussions Reddit et statistiques structurées — grâce à un **RAG hybride** (recherche vectorielle et agent SQL), et **mesure la qualité de ses réponses** sur un jeu de cas de référence. [**Lien vers l'application**](https://llmeval-nba.streamlit.app/) (mot de passe à saisir : nbademo / cold start)

<div style="display: flex; gap: 1rem; align-items: flex-start;" markdown>

<div style="flex: 0 0 50%;">
<img src="assets/ui_nba.jpg" alt="Interface utilisateur" style="width: 100%; border-radius: 12px;">
</div>

<div style="flex: 1;" markdown>

**Parcours utilisateur**

- Question libre ou choix d'un cas d'exemple
- Réponse sourcée, avec la stratégie mobilisée : recherche vectorielle ou requête SQL
- Niveau de confiance et signalement d'un contexte insuffisant
- Détail des contextes récupérés, des scores, de la latence et des tokens
- Consultation des rapports d'évaluation dans l'application

</div>

</div>


<details markdown>
<summary><strong>Architecture de la solution</strong></summary>

Côté conception, l'ingestion unifie des sources hétérogènes — discussions Reddit (PDF) et statistiques NBA (Excel) — en deux socles : un **index vectoriel FAISS** alimenté par les embeddings Mistral, et une **base SQLite** des statistiques structurées. L'agent route ensuite chaque question : recherche vectorielle (`k = 5`) pour le qualitatif, agent SQL (requêtes `SELECT` et `WITH` uniquement, avec repli automatique sur le vectoriel) pour le chiffré. La réponse est structurée — texte, sources réellement utilisées, niveau de confiance, signalement d'un contexte insuffisant — et validée par un schéma Pydantic.

Côté évaluation, la qualité est mesurée sur un jeu de cas versionné (questions et réponses de référence en YAML) : exécution avec pydantic-evals, notation RAGAS sur quatre métriques, traces et coûts dans Logfire, rapports Markdown et CSV générés à chaque run pour comparer les versions du pipeline. Deux versions ont été comparées sur le même jeu de questions :

| Version | Stratégie | Faithfulness | Relevancy | Precision | Recall |
| :--- | :--- | ---: | ---: | ---: | ---: |
| `v0` | FAISS seul | 0,817 | 0,611 | 0,642 | 0,542 |
| `v1` | Hybride FAISS + SQL | **0,875** | **0,825** | **0,762** | **0,742** |

Le gain vient principalement des questions chiffrées : sur les cas traités réellement par SQL, la fidélité passe de 0,500 à 1,000 et le rappel de 0,000 à 1,000.

Côté déploiement, l'application Streamlit est publiée sur **Streamlit Community Cloud** depuis le dépôt : seuls l'index, les chunks et la base (≈ 2 Mo) sont versionnés, les dépendances de l'app sont isolées du reste du projet pour un build léger, et l'accès peut être protégé par mot de passe avec un quota de questions par session.

Pour plus de détails, consultez la [documentation technique](https://olivierbinder.github.io/llmeval/) et le [code](https://github.com/olivierbinder/llmeval) du projet (données, pipeline RAG, évaluation, résultats).


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
        A[("<b>Sources NBA</b><br/>discussions Reddit (PDF)<br/>statistiques (Excel)")] --> B("<b>Ingestion & indexation</b><br/>chunking · embeddings Mistral<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/PyMuPDF-EF3939?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/FAISS-0467DF?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        B --> C("<b>Agent RAG hybride</b><br/>recherche vectorielle (k = 5) · agent SQL en repli<br/>réponse structurée : sources · confiance<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Mistral%20AI-FA520F?style=for-the-badge&logo=mistralai&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Pydantic%20AI-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        C --> D("<b>Évaluation reproductible</b><br/>jeu de cas · métriques RAGAS · rapports<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/pydantic--evals-E92063?style=for-the-badge&logo=pydantic&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/RAGAS-9C27B0?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Logfire-7B2BF9?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
    end

    subgraph DEPL["<b>Déploiement · Application</b>"]
        direction TB
        H("<b>Artefacts versionnés</b><br/>index FAISS · chunks · base SQLite<br/>≈ 2 Mo, démarrage sans ingestion<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Git-181717?style=for-the-badge&logo=git&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        H --> E("<b>Application Streamlit</b><br/>questions · sources · métriques · rapports<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Streamlit%20Cloud-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        E -.->|traces| G("<b>Observabilité</b><br/>traces · coûts · latence<br/><div style='display:flex; gap:6px; justify-content:center; width:250px; margin:6px auto 0;'><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/Logfire-7B2BF9?style=for-the-badge&logoColor=white' height='30' style='display:block; max-width:none;'/></div><div style='flex:none; border-radius:4px; overflow:hidden;'><img src='https://img.shields.io/badge/OpenTelemetry-000000?style=for-the-badge&logo=opentelemetry&logoColor=white' height='30' style='display:block; max-width:none;'/></div></div>")
        E --> F("<b>Accès & coûts maîtrisés</b><br/>mot de passe · quota par session")
    end

    CONC ==>|index + base| DEPL
```

</details>

---

## :lucide-compass: &nbsp; Ce que ces projets couvrent

Pris ensemble, ils parcourent toute la chaîne : préparation et validation des données, entraînement et sélection de modèles, exposition d'un service, interface utilisateur, évaluation et surveillance, jusqu'à l'industrialisation des pipelines et à l'observabilité d'une application LLM. Le détail des outils et de leur niveau de maîtrise est dans la page [Stack](03_stack.md).
