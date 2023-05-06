## TUPLE_DEFINE_STRUCT

```cc[]
struct Person {
  TUPLE_DEFINE_STRUCT(
    Person, (eq, ne, stream),
  (std::string, name),
  (std::string, email));
};
```

NOTES:

* What we already had internally
* Similar to BOOST_FUSION_DEFINE_STRUCT or HANA_DEFINE_STRUCT
* Repetition of struct name.
* Capabilities listed explicitly.
* Fields parenthesized.

@@@

## Boost.PFR

```cc[]
struct Person {
  std::string name;
  std::string email;
};
BOOST_PFR_FUNCTIONS_FOR(Person)
```

NOTES:

* Everything handled for you with just one small macro.

@@@

## Extend

```cc[]
struct Person : Extend<Person>::With<EqualityExtension,
                                     PrintingExtension> {
  std::string name;
  std::string email;
};
```

NOTES:

* What we ended up designing.
* Capabilities are listed as extensions in the template parameter.

@@@

## Extend

```cc[]
struct Person : Extend<Person>::With<EqualityExtension,
                                     OrderingExtension,
                                     PrintingExtension,
                                     JsonExtension> {
  std::string name;
  std::string email;
};
```

NOTES:

* They can be user defined.

@@@

## Extend

```cc[]
struct Person : Extend<Person>::With<OrderingExtension,
                                     PrintingExtension,
                                     JsonExtension> {
  std::string name;
  std::string email;
};
```

NOTES:

* They can specify dependencies

@@@

## Extend

```cc[|8]
class Person 
    : public Extend<Person, 2>::With<EqualityExtension,
                                     PrintingExtension> {
 public:
  explicit Person(std::string name, std::string email);

 private:
  friend EnableExtensions;

  std::string name_;
  std::string email_;
};
```

NOTES:

* Works with classes.

---

* Though you have to add us as a friend.

@@@

<!-- .element data-auto-animate -->
## Extensions

<pre data-id="eq-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct EqualityExtension : Extension&lt;EqualityExtension, T&gt; {


    // Implement me.






};
</code></pre>

@@@
<!-- .element data-auto-animate -->

## Extensions

<pre data-id="eq-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct EqualityExtension : Extension&lt;EqualityExtension, T&gt; {

  friend bool operator==(const T& lhs, const T& rhs) {
    // Implement me.
  }

  friend bool operator!=(const T& lhs, const T& rhs) {
    return !(lhs == rhs);
  }

};
</code></pre>

@@@
<!-- .element data-auto-animate -->

## Extensions

<pre data-id="eq-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct EqualityExtension : Extension&lt;EqualityExtension, T&gt; {

  friend bool operator==(const T& lhs, const T& rhs) {
    return Unpack(lhs) == Unpack(rhs);
  }

  friend bool operator!=(const T& lhs, const T& rhs) {
    return !(lhs == rhs);
  }

};
</code></pre>

@@@
<!-- .element data-auto-animate -->
## Extensions

<pre data-id="serial-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct SerializeExtension : Extension&lt;SerializeExtension, T&gt; {

  friend void Serialize(Serializer& s, const T& value) {
    // Implement me.


  }

  friend bool Deserialize(Deserializer& d, T& value) {
    // Implement me.


  }

};
</code></pre>

@@@
<!-- .element data-auto-animate -->
## Extensions

<pre data-id="serial-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct SerializeExtension : Extension&lt;SerializeExtension, T&gt; {

  friend void Serialize(Serializer& s, const T& value) {
    std::apply([&] (auto&... fields) {
      (s.Serialize(fields), ...);
    }, Unpack(*this));
  }

  friend bool Deserialize(Deserializer& d, T& value) {
    return std::apply([&] (auto&... fields) {
      return (d.Deserialize(fields) && ...);
    }, Unpack(*this));
  }

};
</code></pre>

@@@

## Extensions

```cc[]
template <typename T>
struct OrderingExtension : Extension<OrderingExtension, T> {
  using deps = TypeList<EqualityExtension<T>>;

  friend bool operator<(const T& lhs, const T& rhs) {
    return Unpack(lhs) < Unpack(rhs);
  }
  // ...
};
```

@@@

## And many more

|                                            |               |
|--------------------------------------------|---------------|
| [Better C++14 Reflections](https://youtu.be/UlNUNxLtBI0) | Antony Polukhin |
| [Building a C++ Reflection System in<br />One Weekend Using Clang and LLVM](https://youtu.be/XoYVeduK4yI) | Arvid Gerstmann |
| [C++ Mixins](https://youtu.be/VMOsz61Hg84) | Odin Holmes   |
| [Cute C++ Tricks: Part 2 of N](https://youtu.be/EwEppzQe5Oc) | Daisy Hollman |
| [MFC - the M's for Mixin](https://youtu.be/fC0-Hb5HGBo) | Tobias Loew |
| [Practical (?) Applications of Reflection](https://youtu.be/JrOJ012XxNg) | Jackie Kay |
<!-- .element style="font-size:24pt;" -->
