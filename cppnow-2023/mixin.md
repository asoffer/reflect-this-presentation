## Mixins in&nbsp;![Python](img/python.svg) <!-- .element style="height:64px; position:absolute; top:-0.2em;" -->

```py[|3]
class Eq:
  def __eq__(self, other):
    return self.__dict__ == other.__dict__

class Person(Eq):
  def __init__(self):
    self.name = ""
    self.email = ""
```

@@@

## Mixins in&nbsp;![Javascript](img/javascript.png) <!-- .element style="height:64px; position:absolute; top:-0.2em;" -->

```javascript[|4]
let toStringMixin = {
  toString() {
    let s = "{\n";
    for (const property in this) {
      if (!(this[property] instanceof Function))
        s += `  ${property}: ${this[property]}\n`;
    }
    s += "}";
    return s;
  }
};

class Person {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}
Object.assign(Person.prototype, toStringMixin);
```
<!-- .element: style="font-size:12pt; width:57%;" -->

@@@ <!-- element: data-auto-animate -->

## Mixins in&nbsp;![Rust](img/rust.svg) <!-- .element style="height:64px; position:absolute; top:-0.2em;" -->

```rust[|1]
#[derive(Eq)]
struct Person {
  name: String,
  email: String,
}
```
<!-- .element: style="width:30%;" -->

@@@

## Mixins in&nbsp;![C++](img/cpp.png) <!-- .element style="height:64px; position:absolute; top:-0.2em;" -->

```cc[|5-6]
struct Person {
  std::string name;
  std::string email;

  friend bool operator==(const Person&, const Person&) = default;
  friend bool operator!=(const Person&, const Person&) = default;
};
```
<!-- .element style="font-size:14pt;" -->

NOTES:

Only really have language support.
The goal is to fill in the gap until we can have actual language support.
