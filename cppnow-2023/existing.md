## TUPLE_DEFINE_STRUCT

```cc[]
struct Person {
  TUPLE_DEFINE_STRUCT(
    Person, (eq, ne, stream),
  (std::string, name),
  (std::string, email));
};
```

NOTES:

* Similar to BOOST_FUSION_DEFINE_STRUCT or HANA_DEFINE_STRUCT

@@@

## Boost.PFR

```cc[]
struct Person {
  std::string name;
  std::string email;
};
BOOST_PFR_FUNCTIONS_FOR(Person)
```

NOTES:

Many other talks here.

@@@

## Extend

```cc[]
struct Person : Extend<Person>::With<EqualityExtension,
                                     PrintingExtension> {
  std::string name;
  std::string email;
};
```

NOTES:

Probably also mention Odin Holmes' library?
https://www.youtube.com/watch?v=VMOsz61Hg84
