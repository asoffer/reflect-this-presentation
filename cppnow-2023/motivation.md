## What is a mixin?

NOTES:

Actually, we can't start here. You may already know what a mixin is, but I want
to spend some time motivating why mixins are valuable, by first talking about...


@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

```cc[|1-3,7|5-6,9-41]
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

// Objectionable on both readability and moral grounds!
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
<!-- .element style="font-size:6pt; width:37%;" class="fragment" data-fragment-index="1" -->

@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a mixin</span></span>?
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

@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

### Boilerplate

@@@

## What <span style="color:#a00000;text-decoration:line-through"><span style="color:black">is a mixin</span></span>?
### <span class="edited_title">problem are we solving</span>

<img src="img/printing-plate.jpg" class="bordered" width="60%" />

NOTES:

* Early newspare syndication was done by mailing around printing plates.
* They resembled plates used to make boilers and so they were often called "boilerplates." The text on them was called "boilerplate text."
* Usually full of fluff pieces (timeliness) so boilerplate text became synonymous with "filler."
* Easy to make copy-paste errors.
* Hard to keep up to date.
* Reduced readability.
* Aside on why it's called boilerplate.

@@@

## Boilerplate reduction techniques

<div class="fragment">

* Code generators
* Macros
* Actual magic
* Mixins
</div>

@@@

## Boilerplate reduction techniques

* <span style="color:#a00000;text-decoration:line-through"><span style="color:black">Code generators</span></span>
* Macros
* Actual magic
* Mixins

NOTES:
I'm going to rule out code generation immediately. Code generators are hostile to refactoring at scale.

@@@

## Boilerplate reduction techniques

* <span style="color:#a00000;text-decoration:line-through"><span style="color:black">Code generators</span></span>
* <span style="color:#a00000;text-decoration:line-through"><span style="color:black">Macros</span></span>
* Actual magic
* Mixins

NOTES:
Macros are a form of code generator.

@@@

## Boilerplate reduction techniques

* <span style="color:#a00000;text-decoration:line-through"><span style="color:black">Code generators</span></span>
* <span style="color:#a00000;text-decoration:line-through"><span style="color:black">Macros</span></span>
* <span style="color:#a00000;text-decoration:line-through"><span style="color:black">Actual magic</span></span>
* Mixins

NOTES:

Code generators -- solve the problem for code writers, but not for code readers.
They're also hostile to automatic refactoring tools.
Macros -- Macros are just poorly designed code generators
Magic -- This is actually a real option in some cases.
