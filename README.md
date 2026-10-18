# ContextFlow — Conversational Intelligence Engine

**Turning long conversations into searchable, evidence-backed answers.**

ContextFlow is a retrieval-augmented conversational intelligence system designed to answer questions from customer-support transcripts. It combines semantic retrieval, neural reranking, hierarchical summarization, and structured reasoning to generate answers grounded in relevant conversation turns.

Instead of returning an answer without context, the system lets users inspect the transcript evidence behind the response.

## ✨ Key Features

- **Semantic Retrieval:** Uses FAISS to find potentially relevant transcript passages.
- **Neural Reranking:** Uses ColBERT to improve the relevance of retrieved results.
- **Hierarchical Summarization:** Integrates RAPTOR summary trees to navigate information at different levels of detail.
- **Structured Reasoning:** Uses a ReWOO planning graph to organize retrieval and answer generation.
- **Evidence-Backed Responses:** Connects generated answers to supporting transcript turns.
- **Interactive Chat Interface:** Provides a Next.js frontend for querying conversations and exploring evidence.
- **Query Generation Experiments:** Includes research workflows for generating and evaluating conversational queries.
- **Python API:** Exposes the retrieval and answer-generation pipeline through an API.

## 🧠 System Architecture

```text
                 User Question
                       |
                       v
              ContextFlow Web UI
                  (Next.js)
                       |
                       v
                Chat API Proxy
                       |
                       v
              Python Retrieval API
                       |
                       v
               FAISS Retrieval
                       |
                       v
                ColBERT Reranking
                       |
                       v
             RAPTOR + ReWOO Pipeline
                       |
                       v
              LLM Answer Generation
                       |
                       v
             Answer + Transcript Evidence
```

The backend initializes its models for reuse across requests and maintains separate graph state for individual requests. The frontend displays generated answers and provides access to relevant transcript turns and full conversations.

## 🛠️ Technology Stack

| Component | Technology |
|---|---|
| Retrieval | FAISS |
| Reranking | ColBERT |
| Hierarchical summarization | RAPTOR |
| Reasoning workflow | ReWOO |
| Model serving | vLLM |
| Backend | Python |
| Frontend | Next.js |
| Research and experimentation | Jupyter Notebook |
| Deployment | Docker Compose |

## 📁 Repository Structure

```text
ContextFlow/
├── docs/                       # Project documentation
├── queries-generation/
│   ├── method-1/               # Domain- and intent-based queries
│   ├── method-2/               # Self-Instruct and query refinement
│   └── method-3/               # Transcript-based query generation
├── simulated-real-query-system/
│   └──                           # Query task workbooks
├── technical-report/
│   └── Technical_Report.pdf
├── transcript-rag/
│   ├──                           # Retrieval API and reasoning pipeline
│   ├──                           # Tests and research notebook
│   └──                           # Index and summary artifact locations
├── website/
│   └──                           # Next.js chat application
├── docker-compose.yml
├── Makefile
└── README.md
```

*The exact files and directory names depend on the checked-out repository version.*

## 🚀 Getting Started

### Prerequisites

The complete inference pipeline requires:

- Python and the dependencies specified by the backend
- Docker Compose
- NVIDIA GPU support and NVIDIA Container Toolkit
- Compatible model weights and transcript artifacts
- Node.js 22.18 or newer
- Yarn 1

### 1. Clone the repository

```bash
git clone https://github.com/Aditya-mittal21/ContextFlow.git
cd ContextFlow
```

### 2. Restore the required artifacts

The repository does not include all model weights, transcript data, or precomputed indexes needed for full inference.

Restore the following resources using authorized access to the matching artifacts:

| Location | Required contents |
|---|---|
| `model-gptoss/` | Model weights for vLLM |
| `huggingface/` | Required embedding and reranking model cache |
| `transcript-rag/const/faiss_index/` | FAISS index and corresponding metadata |
| `transcript-rag/const/summaries-20k.json` | Transcript records and summaries |

The original project documentation references Google Cloud Storage locations for some model artifacts. Their availability and access permissions must be verified independently.

**Security note:** Only load trusted index metadata files. Do not download or load serialized artifacts from untrusted sources.

### 3. Start the backend

From the project root, run:

```bash
docker compose up --build
```

Check whether the backend is ready:

```bash
curl http://localhost:8001/ready
```

The retrieval API documentation should be available at:

`http://localhost:8001/docs`

### 4. Configure and launch the frontend

Open another terminal:

```bash
cd website
yarn install --frozen-lockfile
cp .env.example .env.local
yarn dev
```

Configure any required environment variables according to the frontend and backend documentation.

Open the application at:

**http://localhost:3000**

You can then submit questions about the available transcripts and inspect the supporting evidence returned by the system.

Missing artifacts, unavailable models, or configuration errors may prevent inference from running successfully.

## 🔬 Query Generation and Research

ContextFlow includes separate experimental workflows for generating and evaluating questions from conversational data.

| Method | Description |
|---|---|
| Method 1 | Domain- and intent-based query generation with evaluation |
| Method 2 | Self-Instruct generation and Auto Evol-Instruct refinement |
| Method 3 | Query generation using transcript content |

These experiments are independent of the core chat-serving pipeline and are not required to run the frontend and backend.

Explore the corresponding directories under `queries-generation/` for notebooks, scripts, and method-specific instructions.

## 📓 Research Notebook

The project includes a research notebook for answer-generation experiments.

Where supported by the repository's Docker Compose configuration, start the notebook service with:

```bash
docker compose --profile notebook up transcript-rag
```

Open:

`http://localhost:8888`

Run `answer-generation.ipynb` in order, following the notebook's setup instructions.

**Security warning:** The original notebook configuration disables token authentication. Keep the notebook bound to a trusted local environment and do not expose port 8888 publicly without securing it.

## 🌐 Service Endpoints

| Service | Local address |
|---|---|
| Web interface | `http://localhost:3000` |
| Retrieval API | `http://localhost:8001` |
| Interactive API documentation | `http://localhost:8001/docs` |
| vLLM model server | `http://localhost:8000` |
| Jupyter notebook | `http://localhost:8888` |

Endpoints are available only when their corresponding services are running and correctly configured.

## 🧪 Testing and Development

Before contributing changes:

1. Follow the setup instructions in the backend and frontend directories.
2. Run the available backend and frontend tests.
3. Validate retrieval quality and evidence selection against representative queries.
4. Check that API responses and frontend behavior remain consistent.
5. Document any new dependencies, environment variables, or artifact requirements.

See the backend documentation in `transcript-rag/README.md` and the frontend documentation in `website/README.md` for component-specific instructions.

## 🗺️ Future Improvements

Potential directions for extending ContextFlow include:

- Conversation-level memory and context tracking
- Better citation accuracy and evidence verification
- Retrieval evaluation and latency benchmarking
- Streaming responses and improved error handling
- Support for additional conversational data sources
- Deployment monitoring and production-ready configuration

## 📄 License and Attribution

Review the license and attribution requirements of the source repository and all included dependencies before redistributing this project.

ContextFlow should document its own contributions, modifications, and independently developed components clearly. Retain any original attribution required by the applicable license.

---

**ContextFlow — Search conversations. Retrieve context. Generate evidence-backed answers.**
