# 🐙 Polvo (alpha)

**Grab a CSV. Talk to it in 60 seconds.**
Polvo (Portuguese for *octopus*) is an open‑source micro‑stack that bulk‑loads a local CSV, turns every row into an OpenAI vector, and spins up a chat API—all from a single CLI.
This is the **α‑MVP**: one source, one model, one command path. Perfect for proofs‑of‑concept; ready to sprout extra tentacles in future releases.

---

## ⚡ Try it in 5 minutes

```bash
# 1  Spin up Postgres + runtime (Docker)
git clone https://github.com/usepolvo/polvo-stack && cd polvo-stack
cp env.example .env      # add your OPENAI_API_KEY
docker compose up -d

# 2  Install the CLI (Go binary)
brew install usepolvo/tap/polvo   # or `go install github.com/usepolvo/polvo-cli@latest`

# 3  Create a project & load a file
polvo init demo
polvo ingest data/faq.csv          # Polars ➜ PostgreSQL COPY
polvo embed faq                    # batch‑embed 128 rows

# 4  Chat with your data
polvo serve --port 8000 &          # opens http://localhost:8000/docs
polvo chat "How do I reset my password?"
```

---

## 🗺 What ships in the alpha

| Piece                    | Status | Notes                                         |
| ------------------------ | ------ | --------------------------------------------- |
| **CSV → COPY loader**    | ✅      | 200 k rows/s via Polars & binary COPY         |
| **Embeddings**           | ✅      | Synchronous batches, `text‑embedding‑3‑small` |
| **Vector store**         | ✅      | Plain pgvector (L2 distance)                  |
| **Chat & search API**    | ✅      | FastAPI + simple RAG prompt                   |
| **CLI verbs**            | ✅      | `init · ingest · embed · serve · chat`        |
| **OpenTelemetry traces** | ✅      | Console exporter on by default                |
| Airbyte / dbt / Dagster  | ⏳      | Planned for β                                 |
| HNSW ANN indexes         | ⏳      | Planned for β                                 |

---

## 🏗 Tiny mental model

```
CSV  ─▶  Polars loader  ─▶  PostgreSQL (COPY)  ─▶  OpenAI embeddings  ─▶  pgvector table  ─▶  /chat endpoint
```

Everything happens **in‑process** for now—fast enough for \~1 M rows on a laptop.  Future releases will add queues and workers.

---

## 🔧 Frequently‑used CLI flags

```bash
polvo ingest <file.csv>           # --table, --id-column optional
polvo embed <source>              # --model, --batch‑size 128
polvo serve --port 8000           # default 0.0.0.0
polvo chat "question"             # omit text for REPL‑style prompt
```

---

## 🤝 Use with your agent stack

```python
from langchain_postgres import PGVector
store = PGVector.from_connection_string(
    os.getenv("POLVO_DB_DSN"),
    table_name="vector.docs")
```

Need CrewAI?  `from polvo_tools.crewai import PGSearchTool` and drop it into your agent.

---

## 🔭 Roadmap (public)

1. **β – Data fabric** : Airbyte sources, dbt transforms, async embedding workers, HNSW indexes.
2. **v1 – Enterprise** : multi‑tenant, Grafana dashboards, SaaS wizard UI.

Track progress in our [changelog](https://github.com/usepolvo/polvo-core/releases) and vote on features in GitHub Discussions.

---

## 🗂 Repo quick links

* **polvo‑core** – FastAPI runtime & loader
* **polvo‑cli** – CLI utility (Go)
* **polvo‑stack** – Docker compose bundle

---

## 📜 License

MIT.  The vectors—and your data—remain **yours**.

---

**Questions?**  Join our [Discord](https://discord.gg/polvo) or open an issue.
