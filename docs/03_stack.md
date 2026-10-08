# Outils informatiques que j'utilise (Stack MLOps / LLMOps élargie)

Les outils (*langages, bibliothèques, frameworks, plateformes*) maîtrisés apparaissent en <span class="ctp-green">vert</span> ; les autres ont été expérimentés et sont en cours de perfectionnement.

<div class="no-header" markdown>

## :lucide-square-terminal: &nbsp; Software Engineering

Écrire du code propre, versionné et testé, en industrialisant dès le départ les bonnes pratiques de développement.

| Compétences | Outils |
| :--- | :--- |
| Langage & packaging | <span class="ctp-green">`Python` · `uv`</span> |
| Versioning & conventions | <span class="ctp-green">`Git` · `Conventional Commits` · `Commitizen`</span> · `git-cliff` |
| Configuration | <span class="ctp-green">`YAML` · `OmegaConf` · `Pydantic` · `pydantic-settings`</span> |
| Interaction terminal | <span class="ctp-green">`Typer` · `Argparse` · `Loguru` · `Rich`</span> |
| Qualité, formatage & typage | <span class="ctp-green">`Ruff` · `dprint` · `ty`</span> |
| Tests & couverture | <span class="ctp-green">`pytest` · `pytest-cov`</span> |
| Automatisation & hooks | `mise` · `lefthook` |
| Documentation | <span class="ctp-green">`Zensical`</span> |

## :lucide-database-search: &nbsp; Data & Analytics

Explorer, transformer et valider les données, du notebook au pipeline de production.

| Compétences | Outils |
| :--- | :--- |
| Acquisition & ingestion | `dlt` · <span class="ctp-green">`Selenium` · `BeautifulSoup` · `PyMuPDF` · `Docling`</span> |
| Dataframes & manipulation | <span class="ctp-green">`pandas` · `PyArrow` · `Polars` · `DuckDB`</span> |
| Bases de données | <span class="ctp-green">`SQL` · `PostgreSQL` · `SQLite` · `SQLAlchemy 2` · `psycopg 3`</span> · `MongoDB` |
| Transformation & orchestration | `dbt` · `Dagster` · `Airflow` |
| Big data & lakehouse | `Spark` · `PySpark` · `Databricks` |
| Qualité & lineage | <span class="ctp-green">`pandera`</span> · `dbt contracts` · `OpenLineage` |
| Statistiques & EDA | <span class="ctp-green">`numpy` · `scipy` · `statsmodels`</span> |
| Visualisation | <span class="ctp-green">`matplotlib` · `seaborn` · `plotly`</span> |
| BI & dashboards | <span class="ctp-green">`Tableau` · `Power BI`</span> · `Apache Superset` |
| Traitement texte & images | `NLTK` ·`spaCy`· `OpenCV` |

## :lucide-brain-circuit: &nbsp; Machine Learning

Modéliser, entraîner et évaluer des modèles de ML et de deep learning.

| Compétences | Outils |
| :--- | :--- |
| Apprentissage supervisé & évaluation | <span class="ctp-green">`scikit-learn` · `XGBoost` · `LightGBM`</span> |
| Non supervisé & réduction de dimension | `HDBSCAN` · `UMAP` |
| Optimisation d'hyperparamètres | <span class="ctp-green">`Optuna`</span> |
| Explicabilité | <span class="ctp-green">`SHAP`</span> |
| Apprentissage profond | `PyTorch` |
| Accélération matérielle | `CUDA` |

## :lucide-bot: &nbsp; LLM & GenAI

Construire des applications à base de LLM : prompting, RAG, agents, embeddings et adaptation de modèles.

| Compétences | Outils |
| :--- | :--- |
| Prompting & APIs | <span class="ctp-green">`OpenAI` · `Anthropic` · `Mistral`</span> |
| Embeddings | <span class="ctp-green">`Sentence Transformers` · `BGE`</span> |
| RAG | <span class="ctp-green">`LangChain`</span> |
| Vector stores | <span class="ctp-green">`FAISS`</span> · `Qdrant` |
| Reranking | `BGE Reranker` |
| GraphRAG | `Neo4j` |
| Agents & tool calling | <span class="ctp-green">`Pydantic AI` · `LangGraph` · `MCP`</span> |
| Évaluation | <span class="ctp-green">`Ragas` · `pydantic-evals`</span> |
| Modèles open-source & fine-tuning | `Transformers` · `PEFT(LoRA/QLoRA)` |

## :lucide-boxes: &nbsp; Industrialisation, Déploiement & Cloud

Déployer, servir et surveiller les applications en production.

| Compétences | Outils |
| :--- | :--- |
| Conteneurs & orchestration | <span class="ctp-green">`Docker`</span> · `Kubernetes` |
| Cloud, hébergement & IaC | <span class="ctp-green">`GCP` · `Cloud Run`· `Hugging Face Spaces`</span> · `AWS` · `Terraform` |
| CI/CD & maintenance | <span class="ctp-green">`GitHub Actions`  · `GitHub Pages`</span> · `GitLab CI`· `Dependabot` |
| Serving API | <span class="ctp-green">`FastAPI`</span> |
| Apps & interfaces | <span class="ctp-green">`Gradio` · `Streamlit` </span> |
| Observabilité plateforme | `OpenTelemetry` · `Grafana` |

###  Spécifique modèle ML (MLOps)

| Compétences | Outils |
| :--- | :--- |
| Tracking & registry | <span class="ctp-green">`MLflow`</span> |
| Versioning données & modèles | `DVC` |
| Packaging & serving modèle | <span class="ctp-green">`ONNX`</span> · `BentoML` |
| Feature store | `Feast` |
| Orchestration de pipelines ML | `ZenML` · `Kubeflow` |
| Monitoring drift & qualité | <span class="ctp-green">`Evidently`</span> |

### Spécifique application LLM (LLMOps)

| Compétences | Outils |
| :--- | :--- |
| Serving LLM | `vLLM` |
| Passerelle LLM & caching | `LiteLLM` |
| Observabilité & monitoring | <span class="ctp-green">`Logfire`</span> (basé sur `OpenTelemetry`, voir *Observabilité plateforme*) |
| Sécurité LLM & red teaming | `Garak` |

---

**À explorer** : Data catalog · Sécurité & gestion des secrets · Alerting · Gestion des coûts · GitOps

</div>