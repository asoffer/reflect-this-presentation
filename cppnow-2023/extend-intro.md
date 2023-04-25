## Extend

```cc[]
struct Person : Extend<Person>::With<EqualityExtension,
                                     OrderingExtension,
                                     PrintingExtension,
                                     JsonExtension> {
  std::string name;
  std::string email;
};
```

@@@

## Extend

```cc[]
struct Person : Extend<Person>::With<OrderingExtension,
                                     PrintingExtension,
                                     JsonExtension> {
  std::string name;
  std::string email;
};
```

@@@

## Extend

```cc[|8]
class Person 
    : public Extend<Person, 2>::With<EqualityExtension,
                                     PrintingExtension> {
 public:
  explicit Person(std::string name, std::string email);

 private:
  friend EnableExtensions;

  std::string name_;
  std::string email_;
};
```
