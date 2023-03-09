## What is a Mixin?

```cc[]
struct Person {
  std::string name;
  std::string email;
};
```

@@@

## What is a Mixin?

```cc[]
struct Person {
  std::string name;
  std::string email;

  friend bool operator==(const Person&, const Person&) = default;
  friend bool operator!=(const Person&, const Person&) = default;
};
```

@@@

NOTES:
