## What is the competition?

NOTES:

By now you know the drill. I start with a title indicating a common way we
sometimes think about design that's not super healthy.

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

NOTES:

Then I cross it out and replace it with a better framing.

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Boilerplate reduction techniques

* Code generators
* Macros
* Actual magic
* Mixins
<li style="visibility:hidden;">Language reflection support</li>

NOTES:

* Now we understand the what and why. We need a plan of attack.
* Scope is too broad.

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Boilerplate reduction techniques

* <span class="crossed_out"><span class="wrong_content">Code generators</span></span>
* Macros
* Actual magic
* Mixins
<li style="visibility:hidden;">Language reflection support</li>

NOTES:
I'm going to rule out code generation immediately. Code generators are hostile to refactoring at scale.

Solve the problem for code writers, but not for code readers.

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Boilerplate reduction techniques

* <span class="crossed_out"><span class="wrong_content">Code generators</span></span>
* <span class="crossed_out"><span class="wrong_content">Macros</span></span>
* Actual magic
* Mixins
<li style="visibility:hidden;">Language reflection support</li>

NOTES:
Macros are a form of code generator.

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Boilerplate reduction techniques

* <span class="crossed_out"><span class="wrong_content">Code generators</span></span>
* <span class="crossed_out"><span class="wrong_content">Macros</span></span>
* <span class="crossed_out"><span class="wrong_content">Actual magic</span></span>
* Mixins
<li style="visibility:hidden;">Language reflection support</li>

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Boilerplate reduction techniques

* <span class="crossed_out"><span class="wrong_content">Code generators</span></span>
* <span class="crossed_out"><span class="wrong_content">Macros</span></span>
* <span class="crossed_out"><span class="wrong_content">Actual magic</span></span>
* Mixins
* Language reflection support

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Boilerplate reduction techniques

* <span class="crossed_out"><span class="wrong_content">Code generators</span></span>
* <span class="crossed_out"><span class="wrong_content">Macros</span></span>
* <span class="crossed_out"><span class="wrong_content">Actual magic</span></span>
* Mixins
* <span class="crossed_out"><span class="wrong_content">Language reflection support</span></span>

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Mixins in&nbsp;![Python](img/python.svg) <!-- .element style="height:64px; position:relative; top:1em;" -->

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

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Mixins in&nbsp;![Javascript](img/javascript.png) <!-- .element style="height:64px; position:relative; top:1em;" -->

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

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Mixins in&nbsp;![Rust](img/rust.svg) <!-- .element style="height:64px; position:relative; top:1em;" -->

```rust[|1]
#[derive(Eq)]
struct Person {
  name: String,
  email: String,
}
```
<!-- .element: style="width:30%;" -->

@@@

## What <span class="crossed_out"><span class="wrong_content">is the competition</span></span>?
### <span class="edited_title">can we leverage/learn from</span>

#### Mixins in&nbsp;![C++](img/cpp.png) <!-- .element style="height:64px; position:relative; top:1em;" -->


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
