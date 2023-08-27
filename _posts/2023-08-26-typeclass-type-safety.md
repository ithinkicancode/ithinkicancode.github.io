# Regarding typeclass's type safety

---

## This is a header

Tom Kerckhove (a Haskeller) wrote a [blog post](https://cs-syd.eu/posts/2023-08-25-ad-hoc-polymorphism-erodes-type-safety).

```rust
struct Settings {
    allow_list :: Option<Vec<ClientIdentifier>>
}
```

```rust
struct AllowList(
    Option<Vec<ClientIdentifier>>
)

struct Settings {
    allow_list: AllowList
}
```
