# (test post) Why Your Cache Is Lying to You

Caching is the classic "easy win" — until it isn't. The moment you store a copy of something, you've created a second source of truth, and the two will eventually disagree.

## The invalidation problem

Most cache bugs aren't about speed. They're about *staleness*: a user updates their profile, the write hits the database, and the read comes back with yesterday's name because nobody cleared the key.

A few strategies, roughly ordered by how much rope they give you:

- **TTL only** — simple, predictable, always a little wrong
- **Write-through** — update cache and store together; slower writes
- **Explicit invalidation** — fast and correct, until someone forgets a call site

## A concrete example

```python
def get_user(user_id):
    if cached := cache.get(f"user:{user_id}"):
        return cached
    user = db.fetch_user(user_id)
    cache.set(f"user:{user_id}", user, ttl=300)
    return user
```

That `ttl=300` is a promise you're making to your users: *this data may be five minutes old*. Make sure that's a promise you can keep.

> The hard part was never storing the value. It was deciding when to stop trusting it.

Start with a short TTL, measure, and only add complexity when the numbers demand it.
