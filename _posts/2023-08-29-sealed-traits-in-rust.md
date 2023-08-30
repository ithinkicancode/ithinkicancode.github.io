# Sealed traits in Rust

TIL🦀: Rust has "sealed trait" too!

There isn't a `sealed` keyword in Rust; instead it's a pattern. While its concept is similar to Scala’s, it serves a different purpose.

In Scala, the `sealed` modifier is used to prevent a class or trait from being extended externally. Its most common use case in Scala 2 is to define enums or ADTs. (Scala 3 has enums.)

In Rust, trait is really synonymous with typeclasss in Haskell and Scala (I’m going to skip all the glory details for Scala here). And Rust enforces typeclass coherence, which can be summarized as:

A trait can be implemented for a data type as long as either we own the trait or we own the type. In other words, we’re not allowed to implement an external trait (defined elsewhere) for an external type, i.e. we own *neither*.

Sealed traits adds further restriction: traits that are sealed cannot be implemented even when we own the type. The main intention is information hiding, to facilitate future evolutions.

The pattern goes like this:

```rust
pub struct Foo(String);

mod private {
    pub trait SealedMarker {}

    impl SealedMarker for super::Foo {}
}

pub trait TodayILearned: private::SealedMarker {

    fn surprise_me(self) -> String;
}

impl TodayILearned for Foo {
    fn surprise_me(self) -> String {
        self.0.clone()
    }
}
```

Refer to this [API Guidelines](https://rust-lang.github.io/api-guidelines/future-proofing.html#sealed-traits-protect-against-downstream-implementations-c-sealed) for more information.
