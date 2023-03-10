## What is a Mixin?

NOTES:
"A style of software development, in which units of functionality are created in
a class and then mixed in with other classes."

Generic programming:
"Generic programming is a style of computer programming in which algorithms are
written in terms of types to-be-specified-later that are then instantiated when
needed for specific types provided as parameters."

@@@

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

```py
class Eq:
  def __eq__(self, other):
    return self.__dict__ == other.__dict__

class Person(Eq):
  def __init__(self):
    self.name = ""
    self.email = ""
```

NOTES:
In python
@@@

NOTES:

Overview of section:
* wiki explanation
* example in python and a few other languages
* default operator==, <=> in c++
* generic programming wiki explanation.
    mixins are to types what algorithms are to containers!
