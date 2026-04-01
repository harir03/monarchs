# 🏦 INTELLI-CREDIT — AI-Powered Credit Decisioning Engine

<div align="center">
  <img src="./intelli_credit_coloured_flowchart.svg" alt="Master Architecture Flowchart" width="100%" />
</div>

> An end-to-end intelligent credit appraisal system that transforms weeks of manual document review into a fully transparent, AI-driven pipeline completing in under 4 minutes.

---

## 🌟 What It Solves

<div align="center">
  <img src="./visual_selection.png" alt="What Intelli-Credit Solves" width="80%" />
</div>

Intelli-Credit automates the generation of **Credit Appraisal Memos (CAMs)** for the banking sector by ingesting corporate loan documents, performing automated deep-research, detecting fraud, and scoring borrowers—while retaining 100% visible AI reasoning.

| Traditional Process | Intelli-Credit |
| --- | --- |
| 2–3 weeks manual review | < 4 minutes end-to-end |
| Single-analyst perspective | 8 parallel workers + 5 research tracks |
| Black-box decisions | 100% fully-cited, traceable AI logic |

---

## 🏗️ Architecture & Core Layers

Intelli-Credit operates on a 9-Stage Pipeline divided into three core pillars: **Data Ingestion**, **Research Intelligence**, and the **Recommendation Engine**.

### 1. Data Ingestion & Organization Layer
*Extracts, normalizes, and resolves conflicts across unstructured documents.*

```mermaid
graph TD
    A[Raw Documents: PDFs, XML, Excel] --> B(Parallel Document Workers)
    B --> W1[Annual Report Parser]
    B --> W2[Bank Statement Analyzer]
    B --> W3[GST/ITR Matcher]
    B --> W4[Legal / Board Docs]
    W1 & W2 & W3 & W4 --> C{Agent 0.5: Consolidator}
    C -->|Detects Conflicts| D[Data Normalization]
    D --> E((Agent 1.5: Organizer & Neo4j Builder))
```

### 2. External Research & Graph Reasoning Layer
*Fetches real-time market data, scrapes government registries, and performs multi-hop reasoning.*

```mermaid
graph LR
    A[Agent 2: Research] --> B(Tavily / Exa APIs)
    A --> C(MCA21 / NJDG Scrapers)
    B & C --> D{Verification Engine}
    D --> E[Agent 2.5: Graph Reasoning]
    E -->|Circular Trade Detection| F[DOMINANT GNN]
    E -->|Narrative Risk| G[FinBERT Sentiment]
```

### 3. Recommendation & Ticketing Layer
*Scores the applicant, resolves ambiguities involving human oversight, and outputs the final CAM.*

```mermaid
graph TD
    A[Agent 2.5 Out] --> B[Evidence Package Builder]
    B --> C{Confidence Check}
    C -->|High Confidence| D[Agent 3: Scoring Engine]
    C -->|Conflicts / Low Confidence| E((Ticketing Layer))
    E -->|Human Validates| D
    D --> F[Final Credit Appraisal Memo CAM]
    D --> G[(Decision Store Postgres)]
```

### Full System Sequence Workflow

```mermaid
sequenceDiagram
    autonumber
    actor Officer as Credit Officer
    participant GW as API Gateway
    participant Workers as Celery Engine
    participant Agent as LangGraph Agents
    participant Stores as Intelligence Stores
    
    Officer->>GW: Upload Docs & Trigger Analysis
    GW-->>Workers: Spawn 8 Parallel Tasks
    Workers->>Agent: Agent 0.5 (Consolidator) Syncs Threads
    Agent->>Stores: Build Graph (Neo4j) & Embed (Chroma)
    Agent->>Stores: Fetch MCA21, SEBI, NJDG External Signals
    Stores-->>Agent: Send Verified Features + Contradictions
    Agent->>Agent: Agent 3 (Evaluator) Scores & Issues Tickets
    Agent-->>GW: Push Real-Time Thinking (WebSockets)
    GW-->>Officer: Delivery of Final CAM & Explanations
```

---

## 💬 Live Thinking Chatbot

<div align="center">
  <img src="./chatgpt_image.png" alt="Live Thinking Chatbot Demonstration" width="80%" />
</div>

The **Live Thinking Chatbot** is a real-time window into the AI's internal monologue. Through WebSockets, the credit officer sees exactly what the AI read, what it flagged, what it questioned, and what it accepted.

---

## 💻 Complete Technology Stack

| Category | Technologies Used |
|---|---|
| **Frontend Platform** | React 18, TailwindCSS 3, WebSockets |
| **API & Backend** | FastAPI, Celery, Redis, Pydantic v2 |
| **Orchestration** | LangGraph (State Machine), LangChain, LangSmith |
| **Document Processing** | Unstructured.io, Tesseract OCR, Camelot |
| **Databases** | ChromaDB (Vector), Neo4j (Graph), Elasticsearch (Search), PostgreSQL (State) |
| **AI/ML Suite** | Claude Haiku/Sonnet, PyTorch (DOMINANT GNN), FinBERT, Isolation Forest |

### Storage & Database Architecture

```mermaid
graph TD
    Engine[LangGraph Orchestration Engine]
    
    Engine -->|Semantic Embeddings| Vec[(ChromaDB)]
    Engine -->|Relationship Traversing| Graph[(Neo4j)]
    Engine -->|Full-text Regulatory Watchlist| Search[(Elasticsearch)]
    Engine -->|Permanent Audit Ledger| SQL[(PostgreSQL 15)]
    
    Vec -.->|Past Dispute RAG| Engine
    Graph -.->|Circular Fraud Graphing| Engine
```

---

## 🚀 Infrastructure & Deployment

The entire system is containerized for seamless deployment. 

```bash
docker-compose up --build -d
```

### Microservice Deployment Topology

```mermaid
graph LR
    Client([React Frontend]) <-->|HTTPS/WSS| Proxy[NGINX]
    Proxy <--> API[FastAPI Gateway]
    API <--> Broker[(Redis)]
    API <--> Postgres[(PostgreSQL)]
    Broker <--> Celery[Celery Backend]
    
    subgraph Data Layer
      Postgres
      Broker
      Neo[(Neo4j)]
      Es[(Elasticsearch)]
    end
    
    Celery --> Neo
    Celery --> Es
```

- **`frontend`** (React interface)
- **`api-gateway`** (FastAPI orchestration point)
- **`celery-workers`** (Distributed document processing)
- **`redis`** (Broker & WebSocket pub/sub)
- **`neo4j`**, **`postgres`**, **`elasticsearch`** (Data Stores)

---

> **Note:** For deep technical explorations of the AI decision matrices, Neo4j graph construction, and fraud detection algorithms, please refer to the internal technical specification documentation or source code comments.
