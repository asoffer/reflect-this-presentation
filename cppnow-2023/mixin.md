> "A style of software development, in which
> <span class="fragment highlight-green" data-fragment-index="2">units of functionality</span> are
> created in a class and then<br/><span class="fragment highlight-red" data-fragment-index="1">mixed
> in</span> with other classes."
> <div class="author"><a href="https://en.wikipedia.org/wiki/Mixin">Wikipedia:Mixin</a></div>

NOTES:
"A style of software development, in which units of functionality are created in
a class and then mixed in with other classes."

Generic programming:
"Generic programming is a style of computer programming in which algorithms are
written in terms of types to-be-specified-later that are then instantiated when
needed for specific types provided as parameters."

@@@

## Mixins in `Python`

```py
class Eq:
  def __eq__(self, other):
    return self.__dict__ == other.__dict__

class Person(Eq):
  def __init__(self):
    self.name = ""
    self.email = ""
```

@@@

## Mixins in `Javascript`

```javascript[]
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

class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}
Object.assign(User.prototype, toStringMixin);
```

@@@ <!-- element: data-auto-animate -->

## Mixins in `Rust`

<pre class="hljs rust" data-id="code-animation" data-noescape><code data-trim data-line-numbers>
&nbsp;
struct User {
  name: String,
  email: String,
}

impl Eq for User {
  fn eq(&self, other: &Self) -> bool {
    self.name == other.name && self.email == other.email
  }
}
</code></pre>

@@@ <!-- element: data-auto-animate -->

## Mixins in `Rust`

<pre class="hljs rust" data-id="code-animation" data-noescape><code data-trim data-line-numbers>
#[derive(Eq)]
struct User {
  name: String,
  email: String,
}
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
</code></pre>

</section>

@@@

## Mixins in `C++`

```cc[]
struct Person {
  std::string name;
  std::string email;

  friend bool operator==(const Person&, const Person&) = default;
  friend bool operator!=(const Person&, const Person&) = default;
};
```

NOTES:

Only really have language support.

@@@

NOTES:

Overview of section:
* wiki explanation
* example in python and a few other languages
* default operator==, <=> in c++
* generic programming wiki explanation.
    mixins are to types what algorithms are to containers!
