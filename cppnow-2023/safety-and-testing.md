## Safety and Testing

```cc[|11]
class SafeString : public Extend<SafeString, 2>::With<EqualityExtesnion> {
   explicit SafeString(std::string s) : s_(std::move(S)) {}

   std::string copy() const;

   void update(std::invocable<std::string&> auto&& f) { ... }

  private:
   friend EnableExtensions;

   std::mutex m_;
   std::string s_;
};
```
<!-- .element style="font-size:12pt;" -->

NOTES:

With a naive implementation, this will compile-just fine up until someone
actually goes to use equality. We added hooks so users could instantiate
dependent expressions in the types constructor. Not perfect, but will be
surfaced earlier (when the type is constructed, even if the extension isn't
used.

@@@

## Safety and Testing

```cc[]
template <typename T>
struct EqualityExtension : Extension {
  friend bool operator==(const T& lhs, const T& rhs) { ... }
  friend bool operator!=(const T& lhs, const T& rhs) { ... }





};
```
<!-- .element style="font-size:12pt;" -->

@@@

## Safety and Testing

```cc[]
template <typename T>
struct EqualityExtension : Extension {
  friend bool operator==(const T& lhs, const T& rhs) { ... }
  friend bool operator!=(const T& lhs, const T& rhs) { ... }

  constexpr void ForceInstantiateDependentExpressions() const {
    (void)(static_cast<const T&>(*this) == static_cast<const T&>(*this));
    (void)(static_cast<const T&>(*this) != static_cast<const T&>(*this));
  }
};
```
<!-- .element style="font-size:12pt;" -->

@@@

## Safety and Testing

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
<!-- .element style="font-size:12pt;" -->

NOTES:

Credit: Daisy Hollman.
