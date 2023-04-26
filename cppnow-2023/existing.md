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

* What we already had internally
* Similar to BOOST_FUSION_DEFINE_STRUCT or HANA_DEFINE_STRUCT
* Repetition of struct name.
* Capabilities listed explicitly.
* Fields parenthesized.

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

* Everything handled for you with just one small macro.

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

* What we ended up designing.
* Capabilities are listed as extensions in the template parameter.

@@@

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

NOTES:

* They can be user defined.

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

NOTES:

* They can specify dependencies

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

NOTES:

* Works with classes.

---

* Though you have to add us as a friend.

@@@

## And many more

|                                            |               |
|--------------------------------------------|---------------|
| [Better C++14 Reflections](https://youtu.be/UlNUNxLtBI0) | Antony Polukhin |
| [Building a C++ Reflection System in<br />One Weekend Using Clang and LLVM](https://youtu.be/XoYVeduK4yI) | Arvid Gerstmann |
| [C++ Mixins](https://youtu.be/VMOsz61Hg84) | Odin Holmes   |
| [Practical (?) Applications of Reflection](https://youtu.be/JrOJ012XxNg) | Jackie Kay |
<!-- .element style="font-size:24pt;" -->
