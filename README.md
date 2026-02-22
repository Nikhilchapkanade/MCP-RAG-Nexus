<p align="center">
  <h1 align="center">🔌 Mcp-Core</h1>
  <p align="center"><strong>RAG agent with MCP and Llama 3.2 — instantly connects your tools and knowledge base</strong></p>
  <p align="center"><em>Fully dockerized proof-of-concept: AI agent that autonomously queries a knowledge server via MCP.</em></p>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/MCP-Protocol-7C3AED?style=flat-square"/>
  <img src="https://img.shields.io/badge/Llama_3.2-Ollama-000000?style=flat-square"/>
  <img src="https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/LangChain-RAG-1C3C3C?style=flat-square"/>
</p>

---

## 🧠 How It Works

Three microservices running in Docker, talking to each other like three friends in a private room:

```
  Your Browser / curl
        │
        ▼
  ┌──────────────┐     MCP (SSE)     ┌───────────────┐
  │  RAG Agent   │ ◄───────────────► │  MCP Knowledge │
  │  (FastAPI)   │                   │   Server       │
  │  LangChain   │                   │  (FastMCP)     │
  └──────┬───────┘                   │  Mock: Drive,  │
         │                           │  Slack data    │
         │ Ollama API                └───────────────┘
         ▼
  ┌──────────────┐
  │   Ollama     │
  │  Llama 3.2   │
  └──────────────┘
```

| Service | Role | What It Does |
|---------|------|-------------|
| 🧠 **RAG Agent** | The Brain | LangChain agent — decides which tool to call |
| 🔧 **MCP Knowledge** | The Toolbox | Serves mock Google Drive and Slack data via MCP |
| ⚙️ **Ollama** | The Engine | Runs Llama 3.2 locally for text processing |

---

## 🚀 Quick Start

```bash
# 1. Clone
git clone https://github.com/Nikhilchapkanade/Mcp-Core.git
cd Mcp-Core

# 2. Build and start
docker-compose up --build

# 3. Download the LLM (in a new terminal)
docker exec -it mcp-rag-system-ollama-1 ollama pull llama3.2
```

---

## 🧪 Test It

**Scenario A — "Google Drive" search:**
```bash
curl "http://localhost:8080/query?q=What%20is%20the%20pricing%20for%20Enterprise"
# → "The Enterprise plan costs $50/user/month."
```

**Scenario B — "Slack" check:**
```bash
curl "http://localhost:8080/query?q=What%20about%20the%20memory%20leak"
# → Agent searches Slack history for context
```

---

## 📁 Project Structure

```
Mcp-Core/
├── docker-compose.yml       # Connects 3 services
├── rag-agent/               # The Brain
│   ├── Dockerfile
│   ├── server.py            # FastAPI server
│   ├── agent.py             # LangChain tool routing
│   ├── mcp_client.py        # MCP connection manager
│   └── requirements.txt
└── mcp-knowledge/           # The Toolbox
    ├── Dockerfile
    ├── main.py              # Mock Google/Slack tools via FastMCP
    └── requirements.txt
```

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| AI Protocol | Model Context Protocol (MCP) |
| LLM | Llama 3.2 via Ollama |
| Agent Framework | LangChain |
| API Server | FastAPI |
| MCP Server | FastMCP (SSE transport) |
| Infrastructure | Docker Compose |
