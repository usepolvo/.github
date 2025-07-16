# Polvo 🐙

**The missing layer between simple HTTP clients and enterprise service meshes**

## The Problem

Every Python developer knows this progression:

1. **Start with `requests`**: Works great for simple APIs
2. **Add authentication**: Now you're managing tokens manually
3. **Add retry logic**: Copy-paste that exponential backoff function again
4. **Add rate limiting**: Hope your hand-rolled solution actually works
5. **Add OAuth2**: Spend days debugging token refresh
6. **Add multi-tenancy**: Rewrite everything

By step 6, you've built a fragile, ad-hoc API client framework. We've been there. That's why we built Polvo.

## Our Solution

Polvo provides production patterns without the complexity:

```python
import polvo

# Still simple for simple things
response = polvo.get('https://api.example.com/data')

# But handles the hard stuff when you need it
session = polvo.Session(
    auth=polvo.auth.oauth2(...),  # Automatic token refresh
    retry=True,                    # Exponential backoff built-in
    rate_limit='adaptive'          # Reads API headers
)
```

## Core Design Decisions

### 1. **Module Functions First, Sessions Second**

We follow Python's principle of progressive disclosure:

```python
# 90% of use cases - simple module functions
data = polvo.get(url).json()

# 10% of use cases - sessions for advanced features
with polvo.Session() as session:
    session.auth = polvo.auth.oauth2(...)
    data = session.get(url).json()
```

### 2. **Explicit Token Storage**

Security shouldn't be magic. You decide where tokens live:

```python
# You see exactly where tokens are stored
oauth = polvo.auth.OAuth2(
    client_id='id',
    client_secret='secret',
    token_url='https://auth.example.com/token',
    token_cache='~/.myapp/tokens.json',  # Explicit path
    cache_encryption=True                 # Explicit encryption
)
```

No hidden files. No surprise locations. No "where did my tokens go?"

### 3. **Failures Are Explicit**

When things go wrong (and they will), you need clarity:

```python
try:
    response = polvo.get(url)
except polvo.RateLimitError as e:
    print(f"Rate limited. Retry after: {e.retry_after}")
except polvo.TokenRefreshError as e:
    print(f"OAuth2 refresh failed: {e.reason}")
```

### 4. **Composable, Not Configurable**

Instead of a thousand configuration options, we provide composable components:

```python
# Compose the behavior you need
session = polvo.Session(
    auth=polvo.auth.bearer('token'),
    retry=polvo.retry.ExponentialBackoff(max_attempts=5),
    rate_limit=polvo.ratelimit.TokenBucket(rate=10)
)

# Or use smart defaults
session = polvo.Session(retry=True, rate_limit='adaptive')
```

## What We Do Differently

### OAuth2 That Actually Works

Most libraries treat OAuth2 as "just another auth method". We treat it as the complex beast it is:

- **Automatic refresh**: Tokens refresh before expiration, not after failure
- **Thread-safe**: No race conditions during refresh
- **Multi-tenant**: Built-in support for SaaS applications
- **Resilient**: Graceful degradation when token storage fails

### Adaptive Rate Limiting

We don't just respect rate limits, we adapt to them:

```python
# Polvo reads headers like X-RateLimit-Remaining
session = polvo.Session(rate_limit='adaptive')

# Makes requests at optimal rate
for item in items:
    response = session.post('api/process', json=item)
    # Automatically slows down as limit approaches
```

### Production Defaults

Our defaults assume you're running in production:

- ✅ Encryption on by default for token storage
- ✅ Retries with exponential backoff and jitter
- ✅ Timeouts configured (no infinite hangs)
- ✅ Circuit breakers for cascading failures
- ✅ Structured logging ready

## When To Use Polvo

### ✅ Use Polvo when:

- You're integrating with REST APIs that use OAuth2
- You need reliable retry and rate limiting logic
- You're building multi-tenant SaaS applications
- You want `requests` simplicity with production patterns
- You value explicit, debuggable behavior

### ❌ Don't use Polvo when:

- You're making a few simple API calls (just use `requests`)
- You need a full service mesh (use Istio/Linkerd)
- You're building high-frequency trading systems
- You need GraphQL/gRPC/WebSocket support (not our focus)

## Real-World Example

Here's how a real team uses Polvo in production:

```python
# config.py
def create_api_client():
    """Create production-ready API client for Acme API."""
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
        retry=polvo.retry.ExponentialBackoff(
            max_attempts=3,
            retry_on=[500, 502, 503, 504, 429]
        ),
        rate_limit=polvo.ratelimit.TokenBucket(
            rate=100,  # 100 requests per second
            capacity=500  # Burst of 500
        ),
        timeout=30,
        circuit_breaker={
            'failure_threshold': 5,
            'recovery_timeout': 60
        }
    )

# worker.py
def process_customer_data(customer_id):
    """Process data for a specific customer."""
    with api_client() as api:
        # Set tenant context
        api.set_tenant(customer_id)
        
        # Fetch data - all the complexity is handled
        orders = api.get('orders').json()
        
        for order in orders:
            # Process each order
            result = process_order(order)
            
            # Update - automatic retry on failure
            api.patch(f'orders/{order["id"]}', json=result)
```

## Philosophy

1. **Simple things should be simple**: One-line API calls when that's all you need
2. **Complex things should be possible**: Full control when you need it
3. **Explicit is better than implicit**: You should know where your tokens are
4. **Errors should be actionable**: Clear exceptions with solutions
5. **Production is the default**: Secure, reliable, observable out of the box

## The Name

"Polvo" means octopus in Portuguese. Like an octopus with multiple tentacles reaching out to different services, Polvo helps you integrate with multiple APIs.

## License

MIT License. Use it, fork it, sell it – just don't blame us if it breaks.

---

**Still not convinced?**

Try this: Take your most complex API integration. The one with OAuth2, retry logic, rate limiting, and multi-tenant support. Replace it with Polvo. We bet you'll delete more code than you add.

If not, open an issue. We'd love to know what we missed.
