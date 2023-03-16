## What's next?

* Improved testing facilities.
* C++23, Deducing `this`, removes most need for CRTP.
* Want: Robust and portable access to field names?
* Basis for a strong types library.
* Improved field-counting.

@@@

## Strong types

```cc[]
template <typename T,
          typename UnderlyingType,
          template <typename> typename... Extensions>
class StrongType : Extend<T, 1>::template With<Extensions...> {
 public:
  explicit StrongType(T value) : value_(std::move(value)) {}

  const T& value() const { return value_; }

 private:
  friend EnableExtensions;
  T value_;
};
```

@@@

## Strong types

```cc[]
struct MyIdType : StrongType<MyIdType, int,
                             EqualityExtension> {
  using StrongType::StrongType;
};
```

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

Might be worth saying something about why this approach over a macro or straight
`using`. This allows the user to add their own member functions. It gives an
easier off-ramp.
