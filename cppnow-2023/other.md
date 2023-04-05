## Field Counting

NOTES:

This has been done before, so I won't spend too much time on it.

* Certainly auto-counting fields makes things easy to use, but it's not always
  possible.
* Not super important, because we can detect if the number of fields specified
  by a user is wrong.
* Locality between where the user specifies the fields and where they would need
  to update the field count.
* Given that we can't always count, having an easy off-ramp is vital. It'd be a
  shame to say "you can't use any extensions if we can't count the fields."

@@@

## Field Counting

```cc[]
template <typename T>
constexpr int NumFields() {
  return NumInitializers<T>() - 1;
}
```

NOTES:

* Why -1? because we're going to attempt to use aggregate initialization, but we
do have a base class.
* This already is broken for c-style arrays and bit-fields.

@@@

## Field Counting

```cc[]
template <typename T, typename... Args>
constexpr int NumInitializers()
  if constexpr (BraceInitializableWith<T, AnythingExcept<T>, Args...>::value) {
    return sizeof...(Args);
  } else {
    return NumInitializers<T>(args..., Anything());
  }
}
```

NOTES:

TODO: At some point you should mention the C++20 fiasco with uncopyable types in
a vector.
