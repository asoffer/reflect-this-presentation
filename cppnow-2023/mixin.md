## What is a Mixin?

NOTES:

Actually, no. That's not the right question.

@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a Mixin</span></span>?
### <span style="color:#a00000;font-family:serif;position:absolute;top:1.2em;left:2em;">problem are we solving</span>

@@@

## A simple Person struct.

<pre style="font-size:6pt"><code data-line-numbers class="cc hljs cpp">struct Person {
  std::string name;
  std::string email;

  friend bool operator==(const Person&, const Person&) = default;
  friend bool operator!=(const Person&, const Person&) = default;
};

template &lt;&gt;
struct std::hash&lt;Person&gt; {
  size_t operator()(const Person& p) const {
    return hash_combine(std::hash&lt;std::string&gt;{}(p.name),
                        std::hash&lt;std::string&gt;{}(p.email));
  }
};

template &lt;typename H&gt;
H AbslHashValue(H h, const Person& p) {
  return H::combine(std::move(h), p.name, p.email);
}

template &lt;&gt;
struct std::less&lt;Person&gt; {
  size_t operator()(const Person& lhs, const Person& rhs) const {
    if (lhs.name &lt; rhs.name) return true;
    if (rhs.name &lt; lhs.name) return false;
    return lhs.email &lt; rhs.email;
  }
};

std::ostream& operator&lt;&lt;(std::ostream& os, const Person& p) {
  return os &lt;&lt; "Person"
            &lt;&lt; "[ name: " &lt;&lt; std::quoted(p.name)
            &lt;&lt; ", email: " &lt;&lt; std::quoted(p.email) &lt;&lt; " ]";
}

std::string ToJson(const Person& p) {
  return absl::StrFormat("{ name: %v, email: %v }",
                         std::quoted(p.name), std::quoted(p.email));
}
</code></pre>

@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a Mixin</span></span>?
### <span style="color:#a00000;font-family:serif;position:absolute;top:1.2em;left:2em;">problem are we solving</span>

Boilerplate.

@@@

NOTES:

Aside on why it's called boilerplate.

@@@

## What is the Problem We're Solving?

Boilerplate.

* Easy to make copy-paste errors.
* Hard to keep up to date.
* Reduced readability.

NOTES:

Aside on why it's called boilerplate.

@@@

> "A style of software development, in which units of functionality are created
> in a class and then mixed in with other classes."

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
