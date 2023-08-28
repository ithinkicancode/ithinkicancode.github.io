# To Test, or not to test

The debate over the necessity of tests in a PL equipped with a robust type system (e.g. Haskell, Scala, Rust, etc) seems to be a recurring topic. I read a long thread on this very topic just a couple weeks ago. I believe that, even in the context of such languages, a comprehensive testing approach functions both as an adjunct measure and as a countermeasure safeguard, essential for upholding system reliability. Undoubtedly, the encapsulation of constraints within the type system plays a vital role in guaranteeing encoding correctness, minimizing human errors, and facilitating fearless refactoring. While the desire to delegate extensive verification to the type level is admirable, it's crucial to acknowledge the inherent limitations of this approach, particularly concerning intricate semantics and nuanced business logic.

A prime illustration of this notion is aptly showcased in the following blog post (with examples in both Haskell and Rust), where a small, seemingly innocuous refactoring can pass compilation and yet break business logic silently, underscoring the imperative to recognize that relying solely on type-checking might not yield the perceived depth of validation.

[Ad-hoc polymorphism erodes type-safety](https://cs-syd.eu/posts/2023-08-25-ad-hoc-polymorphism-erodes-type-safety)


Assuming you finish reading the article, it is clear that the Iterator impl/instance for the `Option` type, as provided by Rust, falls short of delivering the intended semantics for the given use case. A more appropriate implementation would need to account for both the `None` variant and the `Vec` contained by the `Some` variant. However, due to Rust's orphan rule, which maintains typeclass coherence, overriding the existing implementation is not possible. One common strategy to overcome this hurdle involves employing a newtype, represented by a tuple struct, to wrap `Option<Vec<_>>`. If this is the route taken, compilation will then fail post-refactoring, as the Iterator for the newtype hasn't been implemented.

While this represents a sensible solution, it does demand devs to have the foresight: an understanding of the implications when wrapping data in `Option`, while simultaneously grasping the semantics offered by the existing Iterator impl. In the absence of such considerations, as demonstrated in the article, the program will still successfully type-check, and any issues will only manifest at runtime.

In summary, strive for type-level correctness to avoid/eliminate redundant tests, while also using tests to ensure the accuracy of your business logic.
