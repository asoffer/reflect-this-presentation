## What is a mixin?

NOTES:

Actually, we can't start here.

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
  return absl::StrFormat("{ name: %v, email: %v }",
                         std::quoted(p.name), std::quoted(p.email));
}
```
<!-- .element style="font-size:6pt; width:38%;" class="fragment" data-fragment-index="1" -->

NOTES:

We need to start by understanding the problem.

---

* Intentionally small

---

* The rest is mostly noise. Not unimportant, just uninteresting.
* Not C++ specific

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

Repeat: There's a lot of code here.

---

Only a small portion is important

---

Yet we have to write a lot of repetitive code.

We have a word for this problem.

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

### Boilerplate

NOTES:

* Reduced readability.
* Easy to make copy-paste errors.
* Hard to keep up to date. (Brittle)
* Saps time away from more important/interesting problems.

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

<img src="img/rolled-steel.jpg" class="bordered" width="53%" />

NOTES:

* Refers to rolled steel used to make boilers.
* At first glance, has nothing to do with the repetitiveness.
* The term actually comes from...

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

<img src="img/printing-plate.jpg" class="bordered" width="60%" />

NOTES:

* ...printing plates used early on for newspapers.
* These plates were shipped around to various newspapers to syndicate content. They resembled the rolled steel used for boilers and so they were often referred to as "boilerplates." The text on them was called "boilerplate text."
* Because these plates had to be shipped around the country and that was slow, you would never print interesting or timely information on them. They typically had advertisements or filler pieces.
* So boilerplate text became synonymous with "filler" material.

@@@

## What <span class="crossed_out"><span class="wrong_content">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

> **What:** Boilerplate
<!-- .element class="blockquote1" style="font-size:20pt !important;" -->

> **Why:** Readability, brittleness, development speed
<!-- .element class="blockquote2" style="font-size:20pt !important;" -->
