## What's next?

* Improved testing facilities.
* C++23, Deducing `this`.
* Portable access to field names?
* Improved field-counting.
* Basis for a strong types library.

@@@

## Strong types

```cc[]
template <typename T,
          typename UnderlyingType,
          template <typename> typename... Exts>
class StrongType : Extend<T, 1>::template With<Exts...> {
 public:
  explicit StrongType(T v) : value_(std::move(v)) {}

  const T& value() const { return value_; }

 private:
  friend EnableExtensions;
  T value_;
};
```

NOTES:

* Something is simple as this would provide a really nice appoach.
* Strong types are finnicky because sometimes all you want is comparison with equality.

@@@

## Strong types

```cc[]
struct MyIdType : StrongType<MyIdType, int,
                             EqualityExtension> {
  using StrongType::StrongType;
};
```

NOTES:

Like for a type representing an identifier.

@@@

## Strong types

```cc[]
struct MyNumber : StrongType<MyNumber, int,
                             EqualityExtension,
                             ArithmeticExtension> {
  using StrongType::StrongType;
};
```

NOTES:

* But sometimes you need to do arithmetic.
* And there are many variations on this theme.
* The important point is that the capabilities you want for a type are specifiable with the extensions we've built.
* But I'm getting ahead of myself, because I'm jumping straight to the implementation without first discussing the important parts.

@@@

## How I think about design

1. What problem are we solving, and why?
1. What are our priorities?
1. Evaluate the design space.
1. Implement it!
1. Iteratively improve upon our design.

