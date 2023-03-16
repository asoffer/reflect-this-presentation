## Easy To Use
* Dependencies.
@@@

## Hard to misuse

@@@

## What could go wrong?

@@@

```cc[]
class SafeString : public Extend<SafeString, 2>::With<EqualityExtesnion> {
   explicit SafeString(std::string s) : s_(std::move(S)) {}

   std::string copy() const;

   void update(std::invocable<std::string&> auto&& f) {
    std::lock_guard lock(m_);
    f(s_);
   }

  private:
   friend EnableExtensions;

   std::mutex m_;
   std::string s_;
};
```

NOTES:

With a naive implementation, this will compile-just fine up until someone
actually goes to use equality. We added hooks so users could instantiate
dependent expressions in the types constructor. Not perfect, but will be
surfaced earlier (when the type is constructed, even if the extension isn't
used.

@@@

```cc[]
template <typename T>
struct EqualityExtension : Extension {
  friend bool operator==(const T& lhs, const T& rhs) { ... }

  constexpr void ForceInstantiateDependentExpressions() const {
    (void)(static_cast<const T&>(*this) == static_cast<const T&>(*this));
    (void)(static_cast<const T&>(*this) != static_cast<const T&>(*this));
  }
};
```

```cc[]
template <typename... Extensions>
struct ExtensionSet {
  constexpr ExtensionSet() : Extensions()... {
    if (false) {
      (ForceInstantiateDependentExpressions(static_cast<Extensions&>(*this)),
       ...);
    }
  }
};
```

NOTES:

Credit: Daisy Hollman.

@@@

## Error messages

```txt
extend.h:xx:yy: error: static assertion failed due to requirement 'kFailedToCountFields': Could not detect the number of fields. If you're using a class with private fields, you must specify the number of fields as a second parameter template to `Extend`. For example:
    ```
    class MyType : public Extend<MyType, FIELD_COUNT_GOES_HERE>::With<...> {
     public:
      ...
     private:
      friend EnableExtensions;  // Don't forget this too!
     ...
    };
    ```
See ABSEIL_TIP#extending-classes for a worked example.
```


@@@

* static_assert return tuple()? Might not be worth mentioning.

@@@

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
