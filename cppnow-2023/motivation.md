## What is a Mixin?

NOTES:

Actually, no. That's not the right question.

@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a Mixin</span></span>?
### <span style="color:#a00000;font-family:serif;position:absolute;top:0.7em;left:2em;">problem are we solving</span>

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

## A simple Person struct.

<pre style="font-size:8pt"><code data-line-numbers class="cc hljs cpp">struct Person {
  std::string name;
  std::string email;

  // Objectionable on both readability and moral grounds!
  friend bool operator<=>(const Person&, const Person&) = default;
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

NOTES:

operator<=> is no good here. It does not make sense to say one person is less
than another. But sometimes we do want to put people into a `std::set`.

@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a Mixin</span></span>?
### <span style="color:#a00000;font-family:serif;position:absolute;top:0.7em;left:2em;">problem are we solving</span>

Boilerplate.

* Easy to make copy-paste errors. <!-- .element: class="fragment" -->
* Hard to keep up to date. <!-- .element: class="fragment" -->
* Reduced readability. <!-- .element: class="fragment" -->

NOTES:

Aside on why it's called boilerplate.

@@@

## Boilerplate reduction techniques

* Code generators
* Macros
* Wizardry
* Mixins

NOTES:

Code generators -- solve the problem for code writers, but not for code readers.
They're also hostile to automatic refactoring tools.
Macros -- Macros are just poorly designed code generators
Magic -- This is actually a real option in some cases.
