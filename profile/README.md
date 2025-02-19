# 🐙 Polvo

Polvo is an intelligent API integration toolkit that revolutionizes how developers connect with third-party services. Whether you're building traditional integrations or AI-powered applications, Polvo eliminates complexity so you can focus on features that matter.

## Features

### Brain System
- **AI-Powered Integration**: Build intelligent applications that interact with APIs using natural language
- **Context Management**: Smart memory system for maintaining conversation state
- **Multi-Service Orchestration**: Seamlessly coordinate multiple API services
- **LLM Integration**: Built-in support for the most advanced LLM models available

### Tentacle Framework
- **Unified Async Interface**: Clean, consistent patterns across different APIs
- **Intelligent Authentication**: Seamless OAuth, JWT, and API key handling
- **Smart Rate Limiting**: Automatic request throttling and retry logic
- **Type Safety**: Full Pydantic integration for reliable data handling

## Quick Start

1. Install Polvo:
```bash
pip install usepolvo
```

2. Traditional API Integration:
```python
from usepolvo.tentacles.hubspot import HubSpotClient

# Use the Tentacle framework
client = HubSpotClient()
contacts = await client.contacts.list()
```

3. AI-Powered Integration:
```python
from usepolvo.brain import create_brain
from usepolvo.tentacles.hubspot import HubSpotClient

# Create an intelligent assistant
brain = await create_brain(
    name="CRM Assistant",
    tentacles=[HubSpotClient()]
)

# Natural language interaction
response = await brain.process(
    "Update john@example.com's phone number to +1-555-0123"
)
```

## Supported Integrations

Currently available:
- **Business Tools**: HubSpot
- **AI & ML**: Claude
- **Data & APIs**: OpenMeteo
- More coming soon!

## Why Polvo?

### For Engineers
- Choose between code-first or AI-powered integration
- Write clean, maintainable integration code
- Full type safety and modern IDE support
- Built-in best practices for security and performance

### For Teams
- Standardized patterns across integrations
- Reduced onboarding time
- Active community support
- Comprehensive documentation

### For Organizations
- Faster time-to-market
- Lower maintenance overhead
- Future-ready architecture
- Enterprise-grade security

## Documentation

- [Getting Started](https://docs.usepolvo.com)
- [API Reference](https://docs.usepolvo.com/api)
- [Examples](https://github.com/usepolvo/examples)

## Contributing

We welcome contributions! Check our [Contributing Guidelines](CONTRIBUTING.md) to get started.

## License

MIT License - see [LICENSE](LICENSE) for details.

---

Built with 💜 in Brazil
