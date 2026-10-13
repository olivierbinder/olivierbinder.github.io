# Outils informatiques que j'utilise (Stack MLOps / LLMOps élargie)

Les outils (*langages, bibliothèques, frameworks, plateformes*) maîtrisés apparaissent en <span class="ctp-green">vert</span> ; les autres ont été expérimentés et sont en cours de perfectionnement.

<div class="no-header" markdown>

## :lucide-square-terminal: &nbsp; Software Engineering

Écrire du code propre, versionné et testé, en industrialisant dès le départ les bonnes pratiques de développement.

| Compétences | Outils |
| :--- | :--- |
| Langage & packaging | <span class="ctp-green">`Python` · `uv`</span> |
| Versioning & conventions | <span class="ctp-green">`Git` · `Commitizen`</span>  |
| Configuration | <span class="ctp-green">`YAML` · `OmegaConf` · `Pydantic` · `pydantic-settings`</span> |
| Interaction terminal | <span class="ctp-green">`Typer` · `Loguru` · `Rich`</span> |
| Qualité, formatage & typage | <span class="ctp-green">`Ruff` · `ty`</span> |
| Tests & couverture | <span class="ctp-green">`pytest` · `pytest-cov`</span> |
| Automatisation & hooks | `mise` · `lefthook` |
| Documentation | <span class="ctp-green">`Zensical`</span> |

## :lucide-database-search: &nbsp; Data & Analytics

Explorer, transformer et valider les données, du notebook au pipeline de production.

| Compétences | Outils |
| :--- | :--- |
| Acquisition & ingestion | `dlt` · <span class="ctp-green">`Selenium` · `BeautifulSoup` · `PyMuPDF` · `Docling`</span> |
| Dataframes & manipulation | <span class="ctp-green">`pandas` · `Polars` · `DuckDB`</span> |
| Bases de données | <span class="ctp-green">`SQL` · `PostgreSQL` · `SQLite` · `SQLAlchemy 2` · `psycopg 3`</span> |
| Transformation & orchestration | `dbt` · `Dagster` |
| Qualité & lineage | <span class="ctp-green">`pandera`</span> ·  `OpenLineage` |
| Statistiques & EDA | <span class="ctp-green">`numpy` · `scipy` · `statsmodels`</span> |
| Visualisation | <span class="ctp-green">`matplotlib` · `seaborn` · `plotly`</span> |
| BI & dashboards | <span class="ctp-green">`Tableau` · `Power BI`</span>  |
| Traitement texte & images | `NLTK` ·`spaCy`· `OpenCV` |

## :lucide-brain-circuit: &nbsp; Machine Learning

Modéliser, entraîner et évaluer des modèles de ML et de deep learning.

| Compétences | Outils |
| :--- | :--- |
| Apprentissage supervisé & évaluation | <span class="ctp-green">`scikit-learn` · `XGBoost` · `LightGBM`</span> |
| Optimisation d'hyperparamètres | <span class="ctp-green">`Optuna`</span> |
| Explicabilité | <span class="ctp-green">`SHAP`</span> |
| Apprentissage profond | `PyTorch` |

## :lucide-bot: &nbsp; LLM & GenAI

Construire des applications à base de LLM : prompting, RAG, agents, embeddings et adaptation de modèles.

| Compétences | Outils |
| :--- | :--- |
| Prompting & APIs | <span class="ctp-green">`OpenAI` · `Anthropic` · `Mistral`</span> |
| Embeddings | <span class="ctp-green">`Sentence Transformers` · `BGE`</span> |
| RAG | <span class="ctp-green">`LangChain`</span> |
| Vector stores | <span class="ctp-green">`FAISS`</span> |
| Agents & tool calling | <span class="ctp-green">`Pydantic AI` · `LangGraph` · `MCP`</span> |
| Évaluation | <span class="ctp-green">`Ragas` · `pydantic-evals`</span> |

## :lucide-boxes: &nbsp; Industrialisation, Déploiement & Cloud

Déployer, servir et surveiller les applications en production.

| Compétences | Outils |
| :--- | :--- |
| Conteneurs & orchestration | <span class="ctp-green">`Docker`</span>  |
| Cloud, hébergement & IaC | <span class="ctp-green">`GCP` · `Cloud Run`· `Hugging Face Spaces`</span>  |
| CI/CD & maintenance | <span class="ctp-green">`GitHub Actions`  · `GitHub Pages`</span>  |
| Serving API | <span class="ctp-green">`FastAPI`</span> |
| Apps & interfaces | <span class="ctp-green">`Gradio` · `Streamlit` </span> |

###  Spécifique modèle ML

| Compétences | Outils |
| :--- | :--- |
| Tracking & registry | <span class="ctp-green">`MLflow`</span> |
| Versioning données & modèles | `DVC` |
| Packaging & serving modèle | <span class="ctp-green">`ONNX`</span> · `BentoML` |
| Feature store | `Feast` |
| Monitoring drift & qualité | <span class="ctp-green">`Evidently`</span> |

### Spécifique application LLM

| Compétences | Outils |
| :--- | :--- |
| Observabilité & monitoring | <span class="ctp-green">`Logfire`</span> (basé sur `OpenTelemetry` |

---


</div>