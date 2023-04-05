## Evaluating the Options

*  Evolution
*  Understandability
*  Safety & Testing
*  Development Speed


@@@

## Write it yourself 

<div style="column-count: 2">

```cc[]
struct Person {
  std::string name;
  std::string email;

  friend bool operator==(const Person& lhs, const Person& rhs) {
    return lhs.name == rhs.name && lhs.email == rhs.email;
  }
  friend bool operator!=(const Person& lhs, const Person& rhs) {
    return !(lhs == rhs);
  }
};

std::ostream& operator<<(std::ostream& os, const Person& p) {
  return os << "Person"
            << "[ name: " << std::quoted(p.name)
            << ", email: " << std::quoted(p.email) << " ]";
}

```
<!-- .element style="font-size:8pt;" -->

| Goal              | Grade |
| ----------------- | ----- |
| Evolution         | D  <!-- .element class="fragment" data-fragment-index="1" --> |
| Understandability | A+ <!-- .element class="fragment" data-fragment-index="1" --> |
| Safety & Testing  | C- <!-- .element class="fragment" data-fragment-index="1" --> |
| Dev. Speed        | C  <!-- .element class="fragment" data-fragment-index="1" --> |

@@@

## Boost.PFR

<div style="column-count: 2">

```cc[]
struct Person {
  std::string name;
  std::string email;
};
BOOST_PFR_FUNCTIONS_FOR(Person)
```
<!-- .element style="font-size:16pt;" -->

<br/>
<br/>
<br/>

| Goal              | Grade |
| ----------------- | ------ |
| Evolution         | C  <!-- .element class="fragment" data-fragment-index="1" --> |
| Understandability | A  <!-- .element class="fragment" data-fragment-index="1" --> |
| Safety & Testing  | A+ <!-- .element class="fragment" data-fragment-index="1" --> |
| Dev. Speed        | A+ <!-- .element class="fragment" data-fragment-index="1" --> |
</div>

NOTES:

D- because you can add fields easily, but you are forced to make everything
public. Maybe D- is to harsh here. And users get everything/anything they might
ever want.

Clarify long-term evolution isn't the goal of Boost.pfr. It's primary focus is
dev speed.

@@@

## TUPLE_DEFINE_STRUCT

<div style="column-count: 2">

```cc[]
struct Person {
  TUPLE_DEFINE_STRUCT(
    Person, (eq, ne, stream),
  (std::string, name),
  (std::string, email));
};
```

<br/>

| Goal              | Grade |
| ----------------- | ----- |
| Evolution         | B+ <!-- .element class="fragment" data-fragment-index="1" --> |
| Understandability | C- <!-- .element class="fragment" data-fragment-index="1" --> |
| Safety & Testing  | A+ <!-- .element class="fragment" data-fragment-index="1" --> |
| Dev. Speed        | B  <!-- .element class="fragment" data-fragment-index="1" --> |

</div>

NOTES:

* Similar to BOOST_FUSION_DEFINE_STRUCT or HANA_DEFINE_STRUCT

@@@

## Extend

<div style="column-count: 2">

```cc[]
struct Person
  : Extend<Person>::With<
      EqualityExtension,
      PrintingExtension> {
  std::string name;
  std::string email;
};
```

<br/>

| Goal              | Grade |
| ----------------- | ----- |
| Evolution         | A  <!-- .element class="fragment" data-fragment-index="1" --> |
| Understandability | A  <!-- .element class="fragment" data-fragment-index="1" --> |
| Safety & Testing  | A+ <!-- .element class="fragment" data-fragment-index="1" --> |
| Dev. Speed        | A  <!-- .element class="fragment" data-fragment-index="1" --> |

</div>

NOTES:

This is unfair. I set the goals.
