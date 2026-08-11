# ✨ Lumora AI

**An intelligent, Gemini-powered Agentic RAG platform built with Streamlit.**

Lumora AI is an AI-powered workspace that allows users to interact with documents, perform web searches, access live information, and get intelligent answers through an automated query-routing system.

The platform combines **Retrieval-Augmented Generation (RAG)**, multi-query retrieval, corrective retrieval, web search, tool calling, source citations, and conversational context into a single Streamlit application.

---

## 📸 Screenshots

Add your application screenshots inside the `assets/` directory.

| Chat Interface    | Router Inspector              |
| ----------------- | ----------------------------- |
| `assets/chat.png` | `assets/router-inspector.png` |

| Knowledge Base              | Diagnostics              |
| --------------------------- | ------------------------ |
| `assets/knowledge-base.png` | `assets/diagnostics.png` |

---

## 🚀 Features

### 🧠 Retrieval & Reasoning

* **Intelligent Query Router** — Automatically determines the best processing route for each user query.
* **Document Q&A** — Ask questions about uploaded documents using the RAG pipeline.
* **Corrective RAG** — Falls back to web search when retrieved information has low confidence.
* **Multi-Query Retrieval** — Generates multiple query variations to improve document retrieval.
* **MMR Retrieval** — Improves retrieval diversity while maintaining relevance.
* **Gemini-based Reranking** — Optionally reranks retrieved documents for better results.
* **Conversation-Aware Queries** — Uses previous conversation context to understand follow-up questions.
* **Confidence Scoring** — Evaluates retrieval relevance and answer grounding.
* **Source Citations** — Displays document sources and page references.
* **Tool Calling** — Supports a bounded ReAct-style tool execution flow.

### 💬 Chat Experience

* Real-time token streaming
* Markdown rendering
* Code blocks
* Tables
* Copy messages
* Regenerate responses
* Conversation-aware responses
* Router Inspector
* Response confidence information
* Developer diagnostics dashboard

### 🔎 Supported Capabilities

Lumora AI can route queries to different capabilities, including:

* 📄 Document Q&A
* 🌐 Web Search
* 🌤️ Weather Information
* 💰 Financial Information
* 🧮 Calculator
* 🔀 Hybrid RAG + Web Search
* 🤖 General Knowledge

---

## 🏗️ Architecture

```text
                         User Query
                             │
                             ▼
                  ┌────────────────────┐
                  │   Input Safety     │
                  │     Screening      │
                  └─────────┬──────────┘
                            │
                            ▼
                  ┌────────────────────┐
                  │    Query Router    │
                  └─────────┬──────────┘
                            │
          ┌─────────────────┼──────────────────┐
          │                 │                  │
          ▼                 ▼                  ▼
     Document RAG       Web Search       External Tools
          │                 │                  │
          ▼                 ▼                  ▼
    Multi-Query +        SerpAPI        Weather / Finance
    MMR Retrieval
          │
          ▼
       Reranking
          │
          ▼
    Corrective RAG
          │
          ▼
   Gemini Generation
          │
          ▼
 Streaming Response
 + Citations + Confidence
```

The query router determines whether a request should be processed through document retrieval, web search, weather, finance, hybrid retrieval, tool calling, or direct Gemini generation.

For more information about the complete architecture and retrieval pipeline, see:

```text
docs/ARCHITECTURE.md
```

---

# 🛠️ Installation

## Requirements

* Python **3.11 or 3.12**
* Git
* Google Gemini API Key

## 1. Clone the Repository

```bash
git clone <this-repository-url>
```

## 2. Create a Virtual Environment

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

## 4. Configure Environment Variables

Create a `.env` file based on `.env.example`.

```env
GEMINI_API_KEY=your-gemini-api-key
```

Optional API keys:

```env
SERPAPI_API_KEY=your-serpapi-api-key
OPENWEATHER_API_KEY=your-openweather-api-key
ALPHA_VANTAGE_API_KEY=your-alpha-vantage-api-key
```

## 5. Run the Application

```bash
streamlit run main.py
```

Lumora AI will start using Streamlit's local development server.

---

# ☁️ Deployment

## Streamlit Community Cloud

Lumora AI can be deployed using Streamlit Community Cloud.

### Step 1 — Push the Repository to GitHub

Push your project to GitHub.

Make sure your `.env` file is included in `.gitignore` and is **never committed** to the repository.

### Step 2 — Create a Streamlit Application

Create a new application and select your GitHub repository.

Set the main application file to:

```text
main.py
```

### Step 3 — Configure Secrets

Add your API keys in the Streamlit application's **Secrets** section.

Example:

```toml
GEMINI_API_KEY = "your-gemini-api-key"
SERPAPI_API_KEY = "your-serpapi-api-key"
OPENWEATHER_API_KEY = "your-openweather-api-key"
ALPHA_VANTAGE_API_KEY = "your-alpha-vantage-api-key"
```

The application automatically reads the configured secrets and uses them during runtime.

### ⚠️ Important: Vector Store Persistence

Streamlit Community Cloud uses an ephemeral filesystem.

Therefore, the local ChromaDB vector store:

```text
data/vector_store/
```

may not persist across application restarts or redeployments.

If persistent document storage is required, use an external or hosted vector database.

---

# 🐳 Docker / Self-Hosted Deployment

Lumora AI can also run in any environment capable of running Streamlit and the required Python dependencies.

The environment should provide:

```env
GEMINI_API_KEY=your-gemini-api-key
```

For persistent ChromaDB storage, mount a persistent volume to:

```text
data/vector_store
```

---

# 🧰 Technologies Used

| Layer           | Technology                 |
| --------------- | -------------------------- |
| LLM             | Google Gemini              |
| Framework       | LangChain                  |
| RAG             | LangChain + ChromaDB       |
| Vector Database | ChromaDB                   |
| Embeddings      | Gemini Embeddings          |
| Web Search      | SerpAPI                    |
| Weather         | OpenWeatherMap             |
| Finance         | Alpha Vantage              |
| Frontend        | Streamlit                  |
| Language        | Python 3.11+               |
| Configuration   | `.env` / Streamlit Secrets |

---

# 📁 Project Structure

```text
Lumora-AI/
│
├── main.py                    # Streamlit application entry point
├── requirements.txt            # Python dependencies
├── .env.example                # Environment variable template
│
├── .streamlit/
│   └── config.toml             # Streamlit configuration
│
├── config/
│   ├── settings.py             # Application settings
│   ├── model_config.py         # Model configuration
│   └── logging.py              # Logging configuration
│
├── prompts/                    # AI and router prompt templates
│
├── src/
│   ├── api/                    # API schemas and orchestration
│   ├── llm/                    # Gemini client and response handling
│   ├── rag/                    # Chunking, embeddings and retrieval
│   ├── routing/                # Query routing logic
│   ├── security/               # Security and validation
│   ├── tools/                  # Calculator, weather, finance, web search
│   ├── utils/                  # Utility functions
│   └── examples/               # Example implementations
│
├── ui/
│   ├── theme.py                # UI theme
│   └── components.py           # Reusable UI components
│
├── tests/                      # Automated tests
│
├── docs/
│   └── ARCHITECTURE.md         # Architecture documentation
│
├── assets/                     # Application screenshots
│
└── data/
    └── vector_store/           # ChromaDB vector storage
```

---

# 🧪 Testing

Run the test suite with:

```bash
pytest tests/ -q
```

The tests mock external services such as:

* Gemini
* Weather API
* Finance API
* Web Search API

Therefore, API keys and external network access are not required for the mocked test suite.

---

# 🔐 Security

Lumora AI follows several security practices:

* API keys are never stored directly in source code.
* `.env` is excluded through `.gitignore`.
* `.env.example` contains only placeholder values.
* Streamlit Cloud secrets are stored through the platform's Secrets manager.
* User input is checked for potential prompt injection.
* Uploaded files are validated before ingestion.
* File size and type restrictions are applied.
* Requests are rate-limited per session.
* AI safety controls are applied to model interactions.

---

# 🗺️ Future Improvements

Planned improvements for Lumora AI include:

* Hosted vector database support
* Persistent document storage across deployments
* More advanced multi-step tool execution
* Additional external tools
* Message editing and sharing
* Conversation export
* Suggested prompts
* Keyboard shortcuts
* Advanced cross-encoder reranking
* Improved document management
* Additional AI models and providers

---

# ⭐ Project Highlights

Lumora AI demonstrates the implementation of a production-oriented **Agentic RAG architecture** with:

* Intelligent query routing
* Retrieval-Augmented Generation
* Multi-query retrieval
* Maximum Marginal Relevance (MMR)
* Corrective RAG
* Conversational query understanding
* Tool calling
* Web search integration
* Source citations
* Confidence evaluation
* Real-time LLM streaming
* Streamlit-based custom UI
* Security and input validation

---

## 👨‍💻 Author

**Lov Kumar**

Built with using **Python, Streamlit, LangChain, ChromaDB, and Google Gemini**.
