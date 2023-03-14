## `Extend`

```cc[]
struct Person : Extend<Person>::With<EqualityExtension> {
  std::string name;
  std::string email;
};
```

@@@

## `Extend`

```cc[]
struct Person : Extend<Person>::With<EqualityExtension,
                                     DebugPrintingExtension> {
  std::string name;
  std::string email;
};
```

@@@

## `Extend`

```cc[]
class Person : public Extend<Person, 2>::With<EqualityExtension,
                                              DebugPrintingExtension> {
 public:
  explicit Person(std::string name, std::string email);

 private:
  friend EnableExtensions;

  std::string name_;
  std::string email_;
};
```

@@@

## `Extend`

```cc[]
template <typename NameType>
struct Person : Extend<Person, 2>::template With<EqualityExtension,
                                                 DebugPrintingExtension> {
  NameType name;
  std::string email;
};
```

NOTES:

@@@

## Extensions

```cc[]
template <typename T>
struct SerializeExtension {
  friend void Serialize(Serializer& s, const T& value) {
    std::apply([&] (auto&... fields) {
      (s.Serialize(fields), ...);
    }, this->UnpackThis());
  }

  friend bool Deserialize(Deserializer& d, const T& value) {
    return std::apply([&] (auto&... fields) {
      return (d.Deserialize(fields) && ...);
    }, this->UnpackThis());
  }
};
```

@@@

## Extensions

NOTES:
Add something using dependencies.

@@@

## Implementation

```cc[]
template <typename T, int FieldCount>
struct Extend final {
  template <template <typename> typename... Extensions>
  struct With : ExtensionSet<T, FieldCount, Dependencies<Extensions<T>...> {
  };
};
```

@@@

```cc[]
template <typename T, int FieldCount, typename ExtensionSpec>
struct ExtensionSet;

template <typename T, int FieldCount, typename... Extensions>
struct ExtensionSet <T, FieldCount, TypeList<Extensions...>>
  : Extensions... {};
```

NOTES:

Extend from all simultaneously rather than a hierarchy.

@@@

```cc[]
template <typename... Ts>
using Dependencies = decltype(internal::FindAllDependencies(
    /*unprocessed=*/TypeList<Ts...>{},
    /*  processed=*/TypeList<>{}));
```

@@@

```cc[]
template <typename... As, typename... Bs>
constexpr auto FindAllDependencies(
    TypeList<As...> unprocessed, TypeList<Ts...> processed)) {
  if constexpr (sizeof...(As) == 0) {
    return processed;
  } else {
    using first_t = First<As...>;
    using tail_t = Tail<As...>;
    if constexpr (std::is_same_v<first_t, Ts> || ...) {
      return FindAllDependencies(tail_t{}, processed);
    } else if constexpr (requires { first_t::deps; }) {
      return FindAllDependencies(tail_{} + first_t::deps{},
                                 TypeList<first_t, Bs...>{});
    } else {
      return FindAllDependencies(tail_t{}, TypeList<first_t, Bs...>{});
    }
  }
}
```
