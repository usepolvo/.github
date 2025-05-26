# 🐙 Polvo

**AI-ready database schemas tailored to your data in seconds**

Polvo (Portuguese for *octopus*) analyzes your CSV files and automatically generates optimized database schemas with intelligent embedding recommendations. Upload your data, let our AI decide what to normalize, embed, and index for semantic search, benchmark multiple embedding models, and deploy the perfect schema to Neon or Supabase—all through an intuitive web interface.

Perfect for **Vertical AI applications** that need both traditional relational data and vector search capabilities.

---

## ⚡ Try it now

**Web Interface (Recommended)**
```bash
# Visit usepolvo.com and:
# 1. Upload your CSV
# 2. Review AI-generated schema recommendations  
# 3. Test embedding models in the playground
# 4. Deploy to Neon or Supabase with one click
```

---

## 🎯 What Polvo solves

Building AI applications requires both **relational tables** and **vector indexes**—but deciding what data to embed vs. what to keep traditional is complex and expensive to get wrong.

**Before Polvo:**
- Manual schema design for AI applications
- Guessing which columns need embeddings
- Trial-and-error with embedding models
- Expensive mistakes in production

**With Polvo:**
- 🧠 **AI analyzes your data** and recommends optimal schema
- 🎯 **Smart embedding decisions** - knows when to embed vs. index
- 🏆 **Model benchmarking** - test multiple models on your actual data
- 🚀 **Production deployment** - validated schemas ready for AI applications

---

## 🗺 Current features

| Feature | Status | Description |
|---------|--------|-------------|
| **Smart Schema Analysis** | ✅ | AI identifies keys, relationships, and text patterns |
| **Embedding Recommendations** | ✅ | Decides which columns need vector embeddings |
| **Model Benchmarking** | ✅ | Test OpenAI, Cohere, and local models on your data |
| **Playground Testing** | ✅ | Validate queries before production deployment |
| **One-click Deployment** | ✅ | Deploy to Neon, Supabase, or self-hosted PostgreSQL |
| **Project Management** | ✅ | Handle multiple schema projects with progress tracking |
| **Chat Interface** | ✅ | Test semantic search on deployed schemas |
| Advanced transforms | ⏳ | dbt integration planned for β |
| Real-time collaboration | ⏳ | Team features planned for β |

---

## 🏗 How it works

```
CSV Upload  ➜  AI Analysis  ➜  Schema + Embeddings Plan  ➜  Model Benchmarking  ➜  Production Deployment
```

1. **Upload your CSV** - Drag and drop your data file
2. **AI generates schema** - Smart tables, relationships, and embedding recommendations  
3. **Benchmark models** - Test embedding quality and costs on your actual data
4. **Deploy optimized schema** - Push to Neon/Supabase with pgvector support
5. **Test with chat** - Validate semantic search works as expected

---

## 🎨 Perfect for

- **RAG applications** that need document embeddings + metadata tables
- **E-commerce search** with product embeddings + structured attributes  
- **Knowledge bases** with semantic search + categorical filtering
- **Customer support** with FAQ embeddings + ticket metadata
- **Content platforms** with article embeddings + user data

---

## 🤝 Integrate with your AI stack

**LangChain**
```python
from langchain_postgres import PGVector
store = PGVector.from_connection_string(
    "your_deployed_polvo_database_url",
    table_name="your_table_name"
)
```

**CrewAI**
```python
from polvo_tools.crewai import PolvoSearchTool
tool = PolvoSearchTool(database_url="your_polvo_db")
```

**Direct SQL**
```sql
-- Your relational queries work normally
SELECT * FROM products WHERE category = 'electronics';

-- Plus semantic search via pgvector
SELECT content, embedding <-> embedding('search query') as similarity
FROM products ORDER BY similarity LIMIT 10;
```

---

## 🔬 Why embeddings need smart schema design

**The Problem:** Not all text should be embedded
- ❌ Short labels ("Active", "Pending") → waste money, poor results
- ❌ IDs and codes → breaks semantic search  
- ❌ Structured data → better with traditional indexes
- ✅ Long descriptions → perfect for embeddings
- ✅ User questions → ideal for semantic similarity
- ✅ Article content → enables RAG applications

**Polvo's Solution:** AI analyzes your data patterns and recommends the optimal mix of relational tables and vector indexes, then lets you test and validate before deploying.

---

## 🔭 Roadmap

**β Release (Q2 2024)**
- **Advanced data sources** - Databases, APIs, real-time streams
- **Team collaboration** - Shared projects and schema reviews  
- **Advanced indexing** - HNSW, hybrid search, custom distance functions
- **dbt integration** - Transform data before embedding
- **Cost optimization** - Smart batching and model selection

**v1 Release (Q3 2024)**
- **Enterprise features** - SSO, audit logs, fine-grained permissions
- **SaaS platform** - Hosted version with managed infrastructure
- **Advanced AI** - Custom embedding fine-tuning and schema optimization

---

## 📜 License

MIT. Your data and schemas remain **yours**.

---

**Ready to build better AI applications?**  
[Try Polvo](https://usepolvo.com) • [Join Discord](https://discord.gg/7vcz73Nm)
