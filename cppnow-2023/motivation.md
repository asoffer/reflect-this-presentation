## What is a mixin?

NOTES:

* This is a talk about a mixin library, so I think I'm contractually obligated to show you a picture of ice cream at this point.
* But I'm not going to do that, because asking "what's a mixin" is getting way ahead of ourselves.
* My goal is to walk you through the design process, and if you start here, you're starting with a solution even before you've stated the problem.

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

```cc[|1-3,7|5-6,9-35]
struct Person {
  std::string name;
  std::string email;

  friend bool operator==(const Person&, const Person&) = default;
  friend bool operator!=(const Person&, const Person&) = default;
};

template <>
struct std::hash<Person> {
  size_t operator()(const Person& p) const {
    return hash_combine(std::hash<std::string>{}(p.name),
                        std::hash<std::string>{}(p.email));
  }
};

template <>
struct std::less<Person> {
  size_t operator()(const Person& lhs, const Person& rhs) const {
    if (lhs.name < rhs.name) return true;
    if (rhs.name < lhs.name) return false;
    return lhs.email < rhs.email;
  }
};

std::ostream& operator<<(std::ostream& os, const Person& p) {
  return os << "Person"
            << "[ name: " << std::quoted(p.name)
            << ", email: " << std::quoted(p.email) << " ]";
}

std::string ToJson(const Person& p) {
  return absl::StrFormat(R"({ "name": %v, "email": %v })",
                         std::quoted(p.name), std::quoted(p.email));
}
```
<!-- .element style="font-size:6pt; width:38%;" class="fragment" data-fragment-index="1" -->

NOTES:

* We need to start by understanding the problem we're trying to solve.
* You may already have some idea of what the problem is, but it's important to state it explicitly.

---

* This is the problem we're solving.
* If you can read this, your eyes are better than mine. It's intentionally small so I could fit it all on one slide, but let me walk you through it.
* This is a "simple struct" named `Person`. It has two string fields representing the two properties that every person has: A name and an email.
* That alone isn't really enough to make this `Person` usable, so I've added equality comparisons, a specialization of `std::hash` so it can be put in unordered containers, a specialization of `std::less` so it can go in ordered containers, an `ostream` operator for printing, and a `ToJson` function.

---

* The thing is, this is the only interesting part.

---

* The rest is basically just noise. It's not that it's unimportant, it's just repetitive and not interesting.
* And this is not a C++ specific problem. This comes up in other languages too.

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

```py[|1-4|5-30]
class Person:
    def __init__(self, name: str, email: str):
        self.name = name
        self.email = email

    def __eq__(self, other: Any) -> bool:
        if not isinstance(other, Person):
            return False
        return (self.name == other.name 
               and self.email == other.email)

    def __ne__(self, other: Any) -> bool:
        return not (self == other)

    # Objectionable on both readability and moral grounds!
    def __lt__(self, other: Any) -> bool:
        if self.name < other.name:
            return True
        elif self.name > other.name:
            return False
        return self.email < other.email

    def __hash__(self) -> int:
        return hash((self.name, self.email))

    def to_dict(self) -> Dict[str, Any]:
        return {"name": self.name, "email": self.email}

    def to_json(self) -> str:
        return json.dumps(self.to_dict())
```
<!-- .element style="font-size:7pt; width:38%;" -->

NOTES:

Here's equivalent code in python. It's slightly shorter on account of python being a terser language, but honestly not significantly.

---

And once again, only a small portion is important.

---

Yet we have to write a lot of repetitive code.

We have a word for this problem.

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

<img src="img/rolled-steel.jpg" class="bordered" width="53%" />

NOTES:

* Boilerplate.
* Boilerplate is a problem for both code writers and code readers.
* For writers, it's easy to make copy-and-paste errors. It's also hard to keep up to date as the surrounding code changes.
* For readers, it's noise that takes their attention away from the interesting and meaningful problems.

* This is a picture of actual boilerplate. Rolled steel used to create hot water boilers.
* And of course, this has absolutely nothing to do with the way we use the term "boilerplate" today.
* And that's because it's a twice-borrowed term.

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

<img src="img/printing-plate.jpg" class="bordered" width="60%" />

NOTES:

* Boilerlate really refers to these printing plates used early on for newspapers.
* These printing plates looked a lot like boiler plates which is how the term came to be used for them.
* In the early days of newspaper syndication, you couldn't just email a pdf of an article or advertisement to a newspaper across the country, so you'd physically assemble these plates and ship them.
* But of course, mailing something across the entire United States at that time might take weeks. So you'd never print anything time-sensitive on these boilerplates. You'd only ever mail around "fluff pieces." Things that no one really needed to read.
* And so boilerplates, in reference to these text became synonymous with "filler material."

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

> **What:** Boilerplate
<!-- .element class="blockquote1" style="font-size:20pt !important;" -->

> **Why:** Readability, brittleness, development speed
<!-- .element class="blockquote2" style="font-size:20pt !important;" -->

NOTES:

And that's the problem we're trying to solve. We want to reduce boilerplate as much as possible.
