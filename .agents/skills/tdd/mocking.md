# When to Mock

Mock at **system boundaries** only:

- External APIs (Azure OpenAI, Document Intelligence, etc.)
- Databases (sometimes - prefer test DB)
- Time/randomness
- File system (sometimes)

Don't mock:

- Your own classes/modules
- Internal collaborators
- Anything you control

## Designing for Mockability

At system boundaries, design interfaces that are easy to mock:

**1. Use dependency injection**

Pass external dependencies in rather than creating them internally:

```python
# Easy to mock
def process_payment(order: Order, payment_client: PaymentClient) -> Receipt:
    return payment_client.charge(order.total)

# Hard to mock
def process_payment(order: Order) -> Receipt:
    client = StripeClient(settings.STRIPE_KEY)  # hidden dependency
    return client.charge(order.total)
```

**2. Prefer SDK-style interfaces over generic fetchers**

Create specific functions for each external operation instead of one generic function with conditional logic:

```python
# GOOD: Each method is independently mockable
class ApiClient:
    def get_user(self, user_id: str) -> User: ...
    def get_orders(self, user_id: str) -> list[Order]: ...
    def create_order(self, data: OrderData) -> Order: ...

# BAD: Mocking requires conditional logic inside the mock
class ApiClient:
    def fetch(self, endpoint: str, method: str = "GET", body: dict | None = None) -> dict: ...
```

The SDK approach means:
- Each mock returns one specific shape
- No conditional logic in test setup
- Easier to see which endpoints a test exercises
- Type safety per method
