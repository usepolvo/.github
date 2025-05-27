# 🐙 Polvo

**Stay ahead of the embedding model explosion**

New embedding models launch every week. Yesterday's best model is today's legacy tech. Polvo continuously evaluates the latest models against YOUR specific data, ensuring you're always using the optimal embeddings—not just the ones that benchmarked well on generic datasets six months ago.

---

## The Problem

The embedding landscape evolves at breakneck speed:
- **January**: You deploy with `text-embedding-ada-002`
- **March**: BGE-v1.5 launches with 20% better performance for half the cost
- **May**: A domain-specific model appears that understands your data 40% better
- **Today**: You're still using January's model, wasting money and getting worse results

**Without Polvo**: You find out about better models through HackerNews comments months later.

**With Polvo**: Get notified within 48 hours when a new model beats yours on YOUR data.

---

## Quick Start

### Web Interface (Recommended)

```bash
1. Visit usepolvo.com
2. Upload your dataset (CSV/JSON, 50+ examples)
3. View current best model for your data
4. Enable monitoring for automatic weekly updates
```

### CLI Tool

```bash
# Install
pip install polvo-eval

# One-time evaluation
polvo test data.csv --models all

# Continuous monitoring
polvo monitor data.csv --notify slack --webhook YOUR_WEBHOOK
```

---

## Key Features

### Continuous Model Tracking

We monitor releases from:
- **OpenAI** - New embedding models and updates
- **Hugging Face** - Daily model releases (BGE, E5, etc.)
- **Cohere** - Multilingual and domain updates
- **Voyage AI** - Specialized models (code, law, finance)
- **Google** - Gecko and upcoming models
- **Anthropic** - When they inevitably release embeddings

### Evaluation on YOUR Data

Generic benchmarks lie. MTEB scores don't predict performance on your:
- Legal contracts with specific terminology
- Medical notes with domain jargon
- E-commerce descriptions with brand names
- Support tickets with company-specific context

Polvo tests each model on YOUR actual data and use case.

### Actionable Intelligence

Not just scores—clear recommendations:

```
🚨 Model Alert - March 15, 2024

New model available: mxbai-embed-large-v1
Performance on your data:
- 12% better retrieval accuracy
- 30% faster inference
- 100% free (vs $2,400/mo current)

Recommendation: STRONG UPGRADE
Estimated savings: $28,800/year

[View detailed comparison] [Get migration code]
```

---

## How It Works

### 1. Initial Setup (5 minutes)

```python
# Upload your data
import polvo

client = polvo.Client(api_key="your-key")
dataset = client.upload_dataset(
    "product_descriptions.csv",
    text_column="description"
)

# Run initial evaluation
results = client.evaluate(dataset, models="auto")
print(results.best_model)  # 'mxbai-embed-large-v1'
print(results.savings)     # '$2,400/month vs OpenAI'
```

### 2. Continuous Monitoring

```python
# Enable weekly monitoring
monitor = client.monitor(
    dataset,
    notify=["email", "slack"],
    threshold=0.05  # Alert on 5% improvement
)

# Polvo now automatically:
# - Tests new models every week
# - Compares against your current model
# - Alerts only when action needed
```

### 3. Smart Recommendations

Polvo considers:
- **Performance** - Retrieval accuracy on your data
- **Cost** - API pricing vs self-hosted options
- **Speed** - Inference time for your use case
- **Compatibility** - Dimension changes, API availability
- **Stability** - Model maturity and support

---

## Example Results

### E-commerce Product Search

```
Current: openai/text-embedding-ada-002
Dataset: 50,000 product descriptions

┏━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━┳━━━━━━━━━┳━━━━━━━━━━━┓
┃ Model             ┃ Quality ┃ Speed ┃ Cost/mo ┃ Released  ┃
┡━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━╇━━━━━━━━━╇━━━━━━━━━━━┩
│ voyage-large-2    │ 94%     │ 89ms  │ $1,200  │ Jan 2024  │
│ mxbai-embed-large │ 93%     │ 71ms  │ $0      │ Mar 2024  │ ← Recommended
│ openai-3-small    │ 91%     │ 95ms  │ $800    │ Feb 2024  │
│ openai/ada-002    │ 87%     │ 120ms │ $2,400  │ Dec 2022  │ ← Current
│ bge-large-en-v1.5 │ 86%     │ 67ms  │ $0      │ Oct 2023  │
└───────────────────┴─────────┴───────┴─────────┴───────────┘

Recommendation: Switch to mxbai-embed-large
- 6% quality improvement
- 49ms faster per query
- Save $28,800/year
```

### Legal Document Analysis

```
Dataset: 10,000 legal contracts
Detected: Legal domain content

┏━━━━━━━━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ Model           ┃ Quality ┃ Why It Wins                  ┃
┡━━━━━━━━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┩
│ voyage-law-2    │ 96%     │ Trained on legal corpus      │ ← Recommended
│ openai-3-large  │ 89%     │ Good general understanding   │
│ bge-large       │ 82%     │ Misses legal nuances        │
└─────────────────┴─────────┴──────────────────────────────┘

Domain-specific model provides 7% accuracy boost
```

---

## Supported Models

### Always Up-to-Date

As of March 2024, we track 50+ models including:

**General Purpose**
- OpenAI (ada-002, text-embedding-3-*)
- Cohere (embed-v3-*)
- BGE family (small/base/large)
- E5 family (small/base/large)
- MiniLM variants
- MPNet variants
- mxbai embeddings

**Domain Specific**
- voyage-law-2 (legal)
- voyage-code-2 (code)
- voyage-finance-2 (finance)
- PubMedBERT (medical)
- scibert (scientific)

**Multilingual**
- Cohere multilingual
- mE5 variants
- XLM-RoBERTa based

**New This Month**
- mxbai-embed-large-v1
- nomic-embed-text-v1.5
- voyage-large-2

---

## Why Continuous Evaluation Matters

### Real Customer Story

> "We deployed our RAG system in January with text-embedding-ada-002. Worked great.
>
> In June, a customer mentioned their search seemed worse than a competitor's. We investigated—turned out voyage-2 had launched in March and understood our product catalog 30% better.
>
> We'd been serving inferior results for 3 months and paying 5x more for embeddings. Never again."

— CTO, E-commerce Platform

---

## Installation

### Python SDK

```bash
pip install polvo-eval
```

### Node.js SDK

```bash
npm install @polvo/eval
```

### REST API

```bash
curl -X POST https://api.usepolvo.com/v1/evaluate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -F "file=@your_data.csv" \
  -F "models=auto"
```

---

## Pricing

**Free Tier**
- 3 evaluations/month
- Up to 1,000 documents
- Email notifications

**Pro** - $99/month
- Unlimited evaluations
- Up to 100,000 documents
- Slack/webhook notifications
- API access
- A/B testing tools

**Enterprise** - Custom
- Unlimited everything
- Custom model integration
- On-premise deployment
- SLA support

---

## FAQ

**Q: How is this different from MTEB?**
A: MTEB tests on generic datasets. We test on YOUR data. The best model for Wikipedia might be terrible for your medical notes.

**Q: How often do new models really launch?**
A: Last month: 64 new embedding models on Hugging Face alone. The pace is accelerating.

**Q: What if I've fine-tuned my model?**
A: Upload it as a custom model. We'll alert when a new pre-trained model beats your fine-tuned one.

**Q: Can I test private/proprietary models?**
A: Yes, Enterprise plan supports custom model integration.

---

## The Embedding Arms Race Is Real

Don't get left behind. What was cutting-edge last quarter is now outdated.

[Start Free Evaluation](https://usepolvo.com) • [View Live Demo](https://usepolvo.com/demo) • [Read the Docs](https://docs.usepolvo.com)

Built by engineers who were tired of discovering better models existed... six months too late.
