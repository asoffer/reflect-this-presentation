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

