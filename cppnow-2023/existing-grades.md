## Evaluating the Options

1. Evolution
1. Understandability
1. Safety & Testing
1. Development Speed

NOTES:

* Not enough to just have designed something.

* We're going to look at these examples and assign them grades according to this rubric.

* Having the rubric ahead of time allows us to think methodically and communicate
  to our teammates about why we believe one option is better than another.

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

<br/>

| Goal              | Grade |
| ----------------- | ----- |
| Evolution         | <span class="fragment grade" data-fragment-index="1">B+</span> |
| Understandability | <span class="fragment grade" data-fragment-index="2">A+</span> |
| Safety & Testing  | <span class="fragment grade" data-fragment-index="3">D </span> |
| Dev. Speed        | <span class="fragment grade" data-fragment-index="4">C-</span> |

NOTES:

* Evolution -- Requires lots of changes.
* Understandability -- By definition, nothing tricky going on here.
* Safety & testing -- Easy to forget to add fields or order them incorrectly. But possibly worse is if you forget to add a field into your hasher. Now you're going to have lots of hash collisions and suddenly your constant-time lookups become linear time. If an attacker has control over the values being inserted, you have a security vulnerability in the form of a denial of service attack.
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

| Goal              | Grade |
| ----------------- | ------ |
| Evolution         | <span class="fragment grade" data-fragment-index="1">C </span> |
| Understandability | <span class="fragment grade" data-fragment-index="2">A-</span> |
| Safety & Testing  | <span class="fragment grade" data-fragment-index="3">A+</span> |
| Dev. Speed        | <span class="fragment grade" data-fragment-index="4">A+</span> |
</div>

NOTES:

C because you can add fields easily, but you are forced to make everything
public. Maybe C is to harsh here. And users get everything/anything they might
ever want.

If you ever need to specify one of the functions manually, you need to do so with all of them.

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

| Goal              | Grade  |
| ----------------- | ------ |
| Evolution         | <span class="fragment grade" data-fragment-index="1">B-</span> |
| Understandability | <span class="fragment grade" data-fragment-index="2">C-</span> |
| Safety & Testing  | <span class="fragment grade" data-fragment-index="3">A+</span> |
| Dev. Speed        | <span class="fragment grade" data-fragment-index="4">B </span> |

</div>

NOTES:

* Evolution -- Improvement over Boost.PFR in that the capabilities are listed explicitly, but it only supports structs.
* Understandability -- Macro is magic to most users. We are routinely asked why std::pair doesn't work.
* Safety & Testing -- no problems here.
* Dev speed -- Slowed down just due to the unnatural syntax.
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

| Goal              | Grade  |
| ----------------- | ------ |
| Evolution         | <span class="fragment grade" data-fragment-index="1">A </span> |
| Understandability | <span class="fragment grade" data-fragment-index="2">A-</span> |
| Safety & Testing  | <span class="fragment grade" data-fragment-index="3">A+</span> |
| Dev. Speed        | <span class="fragment grade" data-fragment-index="4">A </span> |

</div>

NOTES:

* Evolution -- Users state exactly which capabilities they want to provide. If any one operation needs to be specified manually, that's possible. Their struct can become a class. 
* Understandability -- Pretty good. Users are already familiar with some functionality being "hidden" in a base class, which is what it looks like here.
* Safety & Testing -- 
* Development speed -- 

This is unfair. I set the goals. This is okay... even expected.

Now that we understand and can articulate the problem we have and why our
proposed solution improves on our goals over other techniques, we can start
talking about the implementation.
