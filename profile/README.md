# Polvo 🐙

**The API integration toolkit that respects production realities**

## Why Another API Client?

Let's be honest: most API client libraries fall into two camps:

1. **Too Simple**: Thin wrappers around HTTP libraries that leave you to handle OAuth flows, rate limiting, retries, and token storage yourself. Fine for demos, painful in production.

2. **Too Complex**: Enterprise service mesh solutions that require infrastructure changes, configuration management, and a PhD in distributed systems. Overkill for most teams.

Polvo sits in the middle. It handles the patterns that cause **real pain in production** without requiring you to change how you build applications.

## The Problem We're Solving

Every team building API integrations faces the same challenges:

- **OAuth2 Token Management**: Tokens expire. Refresh tokens expire. Multiple tenants need different tokens. Most libraries make you handle this yourself.
- **Rate Limiting**: APIs have limits. Good libraries respect them. Great libraries adapt to them automatically.
- **Resilience**: Networks fail. Services go down. Retry logic with proper backoff should be built-in, not bolted on.
- **Multi-tenancy**: SaaS applications need to manage credentials for multiple customers. This shouldn't require a database redesign.
- **Security**: Tokens in plain text files or environment variables? That's a security audit waiting to happen.

## Design Philosophy

### 1. Progressive Disclosure
Simple cases should be simple. Complex cases should be possible.

```python
# Simple
api = polvo.API("https://api.example.com")
response = api.get("/users")

# Complex (but still readable)
api = polvo.API(
    "https://api.example.com",
    auth=polvo.auth.oauth2(...),
    retry=polvo.retry.exponential_backoff(),
    rate_limit=polvo.rate_limit.adaptive()
)
```

### 2. Batteries Included, But Replaceable
We provide sensible defaults for everything, but you can swap out any component.

### 3. Production-First
Every feature is designed with production use in mind. Encrypted token storage isn't an afterthought—it's the default.

### 4. Familiar Interface
If you know `requests` (Python) or `axios` (JavaScript), you already know 80% of Polvo.

## What Makes Polvo Different?

### The OAuth2 "Crown Jewel"
Our OAuth2 implementation isn't just another auth strategy. It's a complete solution for production OAuth2 flows:

- **Automatic token refresh** before expiration
- **Thread-safe** token management
- **Multi-tenant** support out of the box
- **Persistent storage** with encryption
- **Graceful degradation** when storage fails

### Adaptive Rate Limiting
Instead of hard-coding rate limits, Polvo reads API response headers and adapts:

- Reads `X-RateLimit-*` headers automatically
- Adjusts request rate based on remaining quota
- Prevents the thundering herd problem with jitter
- Works with GitHub, Twitter, and custom header formats

### Smart Retries
Not all errors are worth retrying. Polvo knows the difference:

- Retries network errors and 5xx responses
- Respects `Retry-After` headers
- Uses exponential backoff with jitter
- Configurable per-request or globally

## Language Support

### Available Now
- **Python**: Full-featured implementation (v2.0.0)
  - `pip install usepolvo`

### Coming Soon
- **JavaScript/TypeScript**: In development
- **Go**: Planned
- **Rust**: Under consideration

Each implementation follows the same design principles and provides a consistent interface across languages.

## Trade-offs and Honest Limitations

We believe in transparency. Here's what Polvo **doesn't** do:

1. **Not a Service Mesh**: We don't handle service discovery, load balancing, or distributed tracing. Use Istio or Linkerd for that.

2. **Not a Gateway**: We don't proxy requests or provide a central point of control. Each application instance manages its own connections.

3. **Not Magic**: We can't make slow APIs fast or broken APIs work. We just handle the complexity around them.

4. **Opinionated Defaults**: Our defaults prioritize security and reliability over performance. You can change them, but you need to opt-in to less secure options.

## When Should You Use Polvo?

✅ **Use Polvo when:**
- You're integrating with multiple third-party APIs
- You need OAuth2 with token refresh
- You're building a multi-tenant SaaS application  
- You want production-ready patterns without the complexity
- You value security and reliability

❌ **Don't use Polvo when:**
- You only make occasional API calls
- You need a full service mesh solution
- You're building a high-frequency trading system (we prioritize reliability over microsecond latency)
- You want to proxy or transform requests

## Contributing

We welcome contributions, but we're opinionated about our design principles:

1. **Simplicity over features**: We'd rather do fewer things well
2. **Production-first**: Every feature must consider production use
3. **Consistent interface**: All language implementations should feel the same
4. **No magic**: Behavior should be predictable and debuggable

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## The Name

"Polvo" means octopus in Portuguese. Like an octopus with multiple tentacles reaching out to different services, Polvo helps you integrate with multiple APIs. The original v1 took this metaphor too literally with its "Brain/Tentacles" architecture. v2 keeps the name but drops the complexity.

## License

MIT License - see [LICENSE](LICENSE) for details.

---

**Questions? Criticism? Ideas?**

We built Polvo because we were tired of solving the same problems in every project. If you think we're wrong about something, we want to hear it. Open an issue and let's discuss.

Remember: The best API client is the one that stays out of your way until you need it.
