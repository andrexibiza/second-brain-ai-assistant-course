# Project Overview

This is the repository for the **"Building Your Second Brain AI Assistant"** course by Decoding ML. It teaches how to build a production-ready agentic RAG and LLM system using MLOps best practices.

The project is structured into two main Python applications:
1.  **`apps/second-brain-offline`** (Modules 1-5): Handles offline ML pipelines including data ETL (Notion/Crawling), RAG vector index computation, dataset generation, and LLM fine-tuning.
2.  **`apps/second-brain-online`** (Module 6): The online inference pipeline serving the "Second Brain" AI assistant, including the agentic application and evaluation.

**Key Technologies:**
-   **Language:** Python (3.11 for Online, 3.12 for Offline)
-   **Package Manager:** `uv`
-   **Orchestration:** ZenML
-   **Database:** MongoDB (with vector search)
-   **LLMs:** OpenAI, Hugging Face (Llama 3.1)
-   **Evaluation:** Opik, Comet
-   **Infrastructure:** Docker

# Building and Running

## Prerequisites
-   Python 3.11+
-   `uv` (Python package manager)
-   Docker & Docker Compose
-   GNU Make
-   Git

## 1. Offline Application (`apps/second-brain-offline`)

This app manages the data and training pipelines.

### Installation
Navigate to the directory and set up the environment:
```bash
cd apps/second-brain-offline
uv venv .venv-offline
source .venv-offline/bin/activate  # On Windows: .venv-offline\Scripts\activate
uv pip install -e .
# Post-install setup for crawling
uv pip install -U "crawl4ai==0.4.247"
crawl4ai-setup
```

### Configuration
Copy the example environment file and fill in your credentials (OpenAI, Hugging Face, MongoDB, etc.):
```bash
cp .env.example .env
```

### Running Infrastructure
Start the local infrastructure (ZenML, MongoDB):
```bash
make local-infrastructure-up
```

### Running Pipelines (Examples)
Run the ETL pipeline (populate MongoDB):
```bash
make etl-pipeline
# Or use precomputed data to save time/cost:
make download-crawled-dataset
make etl-precomputed-pipeline
```

Run RAG Vector Indexing:
```bash
make compute-rag-vector-index-openai-parent-pipeline
```

Run Tests:
```bash
make test
```

## 2. Online Application (`apps/second-brain-online`)

This app runs the actual AI assistant agent. **Note:** You must run the offline pipelines first to populate the database.

### Installation
Navigate to the directory and set up the environment:
```bash
cd apps/second-brain-online
uv venv .venv-online
source .venv-online/bin/activate  # On Windows: .venv-online\Scripts\activate
uv pip install -e .
```

### Configuration
Copy the example environment file and fill in your credentials:
```bash
cp .env.example .env
```

### Running the Agent
Run a single query from CLI:
```bash
make run_agent_query RETRIEVER_CONFIG=configs/compute_rag_vector_index_openai_parent.yaml
```

Run the Gradio UI:
```bash
make run_agent_app RETRIEVER_CONFIG=configs/compute_rag_vector_index_openai_parent.yaml
```

Run Evaluation:
```bash
make evaluate_agent RETRIEVER_CONFIG=configs/compute_rag_vector_index_openai_parent.yaml
```

# Development Conventions

-   **Code Structure:** Follows a domain-driven design with `application`, `domain`, and `infrastructure` layers within `src/`.
-   **Configuration:** ZenML configurations are stored in `configs/*.yaml`.
-   **Linting & Formatting:** The project uses `ruff`.
    -   Check format: `make format-check`
    -   Fix format: `make format-fix`
    -   Check lint: `make lint-check`
    -   Fix lint: `make lint-fix`
-   **Testing:** Uses `pytest` located in `tests/`.
