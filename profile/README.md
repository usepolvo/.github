# Polvo 🐙

**The missing layer between simple HTTP clients and enterprise service meshes**

Polvo is a production-ready API integration toolkit available for Python and Node.js. It bridges the gap between basic HTTP wrappers (like `requests` in Python or `fetch`/Axios in Node.js) and complex infrastructure solutions, handling OAuth2, rate limiting, retries, and multi-tenancy with explicit, debuggable patterns.

## The Problem

Developers in both Python and Node.js ecosystems face similar challenges when building robust API integrations:

1. **Start simple**: Use `requests` (Python) or `fetch`/Axios (Node.js) for basic calls.
2. **Add authentication**: Manually manage tokens and headers.
3. **Add retry logic**: Implement exponential backoff from scratch.
4. **Add rate limiting**: Build custom throttlers that often fail under load.
5. **Add OAuth2**: Debug token refresh cycles for days.
6. **Add multi-tenancy**: Rewrite core logic for tenant isolation.

By this point, you've created a brittle, custom framework. We've experienced this in both languages—that's why Polvo exists as cross-platform libraries.

## Our Solution

Polvo delivers production patterns without unnecessary complexity, with consistent APIs across languages:

### Python
```python
import polvo

# Simple module functions
response = polvo.get('https://api.example.com/data')

# Advanced session
session = polvo.Session(
    auth=polvo.auth.oauth2(...),  # Automatic token refresh
    retry=True,                    # Exponential backoff
    rate_limit='adaptive'          # Header-based adaptation
)
```

### Node.js
```javascript
import polvo from 'polvo';

// Simple functions
const response = await polvo.get('https://api.example.com/data');

// Advanced session
const session = polvo.create({
    auth: polvo.auth.oauth2(...),  // Automatic token refresh
    retry: true,                    // Exponential backoff
    // Rate limiting coming soon; use custom for now
});
```

Install via `pip install usepolvo` (Python) or `npm install polvo` (Node.js).

## Core Design Decisions

Polvo's philosophy is shared across implementations, with language-appropriate adaptations.

### 1. **Module Functions First, Sessions Second**

Progressive disclosure for simplicity:

- Python: Module functions for 90% of cases; sessions for advanced.
- Node.js: Function calls for basics; configurable sessions for production.

### 2. **Explicit Token Storage**

No magic—specify where tokens live for security and control:

- Both: File, memory, or custom storage; encryption by default.
- Python example: `token_cache='~/.myapp/tokens.json', cache_encryption=True`.
- Node.js example: `tokenCache: '~/.myapp/tokens.json', cacheEncryption: true`.

### 3. **Failures Are Explicit**

Clear, actionable errors:

- Python: `polvo.RateLimitError`, `polvo.TokenRefreshError`.
- Node.js: `PolvoHTTPError`, `TokenRefreshError` with `reason` and `config`.

### 4. **Composable, Not Configurable**

Build what you need with defaults for production:

- Python: Compose retry/rate_limit strategies.
- Node.js: Configure sessions; composable auth handlers.

## What We Do Differently

### OAuth2 That Actually Works

Treated as a first-class citizen in both libraries:

- Automatic proactive refresh.
- Thread-safe (Python) / Promise-safe (Node.js).
- Multi-tenant support.
- Graceful fallback on storage issues.

### Adaptive Rate Limiting

Respect and adapt to API headers:

- Python: Built-in 'adaptive' mode.
- Node.js: Header parsing with manual throttling (adaptive in progress).

### Production Defaults

- Encrypted storage.
- Retries with backoff/jitter.
- Timeouts to prevent hangs.
- Circuit breakers (Python; planned for Node.js).

## When To Use Polvo

### ✅ Use Polvo when:

- Integrating REST APIs with OAuth2.
- Needing reliable retries/rate limiting.
- Building multi-tenant apps.
- Valuing `requests`/Axios simplicity with production features.
- Prioritizing explicit behavior.

### ❌ Don't use Polvo when:

- Simple calls only (use `requests`/fetch).
- Full service mesh needed (Istio/Linkerd).
- High-frequency trading.
- GraphQL/gRPC/WebSockets (not focused).

For Python: If you love `requests` but need more.
For Node.js: If Axios falls short on auth/resilience.

## Real-World Example

### Python
```python
# config.py
def create_api_client():
    return polvo.Session(
        base_url='https://api.acme.com',
        auth=polvo.auth.OAuth2(
            flow='client_credentials',
            client_id=os.environ['ACME_CLIENT_ID'],
            client_secret=os.environ['ACME_CLIENT_SECRET'],
            token_url='https://auth.acme.com/oauth/token',
            token_cache=f'{Config.DATA_DIR}/acme_tokens.json',
            cache_encryption=True
        ),
        retry=polvo.retry.ExponentialBackoff(max_attempts=3),
        rate_limit=polvo.ratelimit.TokenBucket(rate=100, capacity=500),
        timeout=30
    )

# worker.py
def process_customer_data(customer_id):
    with api_client() as api:
        api.set_tenant(customer_id)
        orders = api.get('orders').json()
        for order in orders:
            result = process_order(order)
            api.patch(f'orders/{order["id"]}', json=result)
```

### Node.js
```javascript
// config.js
function createApiClient() {
    return polvo.create({
        baseURL: 'https://api.acme.com',
        auth: polvo.auth.oauth2({
            flow: 'client_credentials',
            clientId: process.env.ACME_CLIENT_ID,
            clientSecret: process.env.ACME_CLIENT_SECRET,
            tokenUrl: 'https://auth.acme.com/oauth/token',
            tokenCache: `${Config.DATA_DIR}/acme_tokens.json`,
            cacheEncryption: true
        }),
        retry: { maxAttempts: 3 },
        timeout: 30000
    });
}

// worker.js
async function processCustomerData(customerId) {
    const api = createApiClient();
    // Tenant support in development
    const orders = (await api.get('orders')).data;
    for (const order of orders) {
        const result = processOrder(order);
        await api.patch(`orders/${order.id}`, result);
    }
}
```

## Philosophy

1. **Simple things should be simple**: One-line calls.
2. **Complex things should be possible**: Full control.
3. **Explicit is better than implicit**: Know your token locations.
4. **Errors should be actionable**: Clear fixes.
5. **Production is the default**: Secure by design.

## The Name

"Polvo" means octopus in Portuguese—like tentacles reaching multiple services.

## License

MIT. Use, fork, or sell—your risk.

---

**Still not convinced?**

Replace your complex integration with Polvo in Python or Node.js. You'll likely delete more code than add.

Open an issue in the relevant repo: [Python](https://github.com/usepolvo/polvo-python) or [Node.js](https://github.com/usepolvo/polvo-js). We'd love feedback.
