# 🐙 Polvo – Eight arms. Infinite insight.

**Polvo** (**pōl·voh**, Portuguese for *octopus*) is the multi‑armed data & AI platform that grabs every source you need, enriches it with Retrieval‑Augmented Generation (RAG) super‑powers, and serves it back through an open MCP server you control.

> **Elevator pitch (20 sec)**
> *Plug in any database or SaaS, vector‑index it with one flag, and chat or automate actions over your fresh context – all from a single CLI.*

---

### 1. Start PostgreSQL with pgvector

```bash
docker run -d \
  --name polvo-postgres \
  -e POSTGRES_DB=polvo \
  -e POSTGRES_USER=polvo \
  -e POSTGRES_PASSWORD=polvo_password \
  -p 5432:5432 \
  pgvector/pgvector:pg16
```

### 2. Start the MCP / Runtime server

```bash
cd polvo-runtime
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn server.main:app --port 8787
```

### 3. Build the CLI

```bash
cd polvo-cli
go build -o polvo .
cd ..
```

### 4. Initialize a project & wire a source

```bash
./polvo-cli/polvo init my-project
cd my-project

# Add a CSV, Markdown folder, Postgres table… you name it
../polvo-cli/polvo source add docs --driver csv --spec data/product_docs.csv

# Crunch vectors & ask a question
../polvo-cli/polvo run
```

### 5. Chat with your data

Visit **[http://localhost:8787/chat](http://localhost:8787/chat)** and start asking away!

</details>

---

## 📂 Project Structure (generated)

```
my-project/
├── polvo.yaml            # Central config
├── data/                 # Raw files or mounted volumes
│   └── product_docs.csv
└── .env                  # Secrets & URLs
```

---

## 🔧 Configuration (`polvo.yaml`)

```yaml
project: my_project

# Where Polvo stores embeddings
database:
  provider: postgres
  url: postgresql://polvo:polvo_password@localhost:5432/polvo

sources:
  - name: product_docs
    driver: csv
    spec: data/product_docs.csv
    sync: full

embedding:
  table: documents
  column: content
  model: text-embedding-3-small
  chunk_size: 1000
  chunk_overlap: 200

retriever:
  k: 5        # Top‑K chunks
  threshold: 0.7
```

---

## 🛠️ CLI Cheatsheet

| Command                | What it does                     |
| ---------------------- | -------------------------------- |
| `polvo init <project>` | Scaffold a new workspace         |
| `polvo db create`      | Set up pgvector & tables         |
| `polvo source add`     | Register a connector             |
| `polvo run`            | Execute the full pipeline        |
| `polvo chat`           | Open a TUI chat in your terminal |

---

## 🔌 API Endpoints (FastAPI)

* `GET /health` – Liveness probe
* `POST /v1/chat` – RAG chat completions
* `POST /v1/tools/vector_search` – Semantic chunk search
* `GET /mcp` – Natural‑language MCP server

---

## 🧪 Testing Your Setup

```bash
# Check DB
psql "$POSTGRES_URL" -c "SELECT COUNT(*) FROM documents;"

# Health probe
curl http://localhost:8787/health

# Vector search
curl -X POST http://localhost:8787/v1/tools/vector_search \
  -H "Content-Type: application/json" \
  -d '{"query":"security features","k":3}'
```

---

## 🌊 Philosophy

Polvo is **curious**, **helpful**, **transparent**, and a bit **playful**. Every default is surfaced, every provider is swappable, and no black‑box SaaS stands between you and your data.

---

## 🖋️ License

MIT
