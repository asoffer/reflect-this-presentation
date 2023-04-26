## Implementation: Overview

```cc[]
template <typename T, int FieldCount = -1>
struct Extend final {
  template <template <typename> typename... Extensions>
  struct With : ExtensionSet<
      T, FieldCount, Dependencies<Extensions<T>...>> {};
};
```
<!-- .element data-id="code-animation" class="fragment" data-fragment-id="1" -->

NOTES:

It's not really important from the design perspective, but when we later talk about iterative improvements, it's important that we have some understanding.

---

First define the structure that lets us write `Extend<T>::With`

Three interesting things on this slide:
* FieldCount -- `-1` specifies that we will try to deduce the count.
* Dependencies -- This allows us to compute unspecified-but-depended-on extensions as I'll show in a few slides.
* ExtensionSet -- Base class that allows us to inherit from all those extensions.

@@@ <!-- .element data-auto-animate -->

## Implementation: Overview

<pre data-id="code-animation"><code data-trim data-line-numbers>
template &lt;typename T, int FieldCount, typename ExtSpec&gt;
struct ExtensionSet;

template &lt;typename T, int FieldCount, typename... Exts&gt;
struct ExtensionSet&lt;T, FieldCount, TypeList&lt;Exts...&gt;&gt;
  : Exts... {};
</code></pre>

NOTES:

Extend from all simultaneously rather than a hierarchy.

@@@

## Implementation: Dependencies

```cc[]
template <typename... Ts>
using Dependencies = decltype(internal::FindAllDependencies(
    /*unprocessed=*/TypeList<Ts...>{},
    /*  processed=*/TypeList<>{}));
```

@@@

## Implementation: Dependencies

```cc[|4-5|7-10|11-13|15]
template <typename... As, typename... Bs>
constexpr auto FindAllDependencies(
    TypeList<As...> unprocessed, TypeList<Bs...> processed) {
  if constexpr (sizeof...(As) == 0) {
    return processed;
  } else {
    using first_t = First<As...>;
    using tail_t = Tail<As...>;
    if constexpr ((std::is_same_v<first_t, Bs> || ...)) {
      return FindAllDependencies(tail_t{}, processed);
    } else if constexpr (requires { typename first_t::deps; }) {
      return FindAllDependencies(tail_t{} + typename first_t::deps{},
                                 TypeList<first_t, Bs...>{});
    } else {
      return FindAllDependencies(tail_t{}, TypeList<first_t, Bs...>{});
    }
  }
}
```
<!-- .element style="font-size:12pt; width:80%;" -->

NOTES:

Testament to compile-time programming improvements over the last decade.

@@@

## Implementation: Overview

```cc[]
template <typename... Ts>
using Dependencies = /* see previous slides */

template <typename T, int FieldCount, typename ExtensionSpec>
struct ExtensionSet;

template <typename T, int FieldCount, typename... Extensions>
struct ExtensionSet <T, FieldCount, TypeList<Extensions...>>
  : Extensions... {};

template <typename T, int FieldCount = -1>
struct Extend final {
  template <template <typename> typename... Extensions>
  struct With : ExtensionSet<
      T, FieldCount, Dependencies<Extensions<T>...>> {};
};
```
<!-- .element style="font-size:14pt; width:80%;" -->

@@@

<!-- .element data-auto-animate -->
## Implementation: Extensions

<pre data-id="eq-animation"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct EqualityExtension : Extension&lt;EqualityExtension, T&gt; {
    // Implement me.
};
</code></pre>

@@@
<!-- .element data-auto-animate -->

## Implementation: Extensions

<pre data-id="eq-animation"><code data-trim data-line-numbers>
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

## Implementation: Extensions

<pre data-id="eq-animation"><code data-trim data-line-numbers>
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
## Implementation: Extensions

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
## Implementation: Extensions

<pre data-id="serial-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct SerializeExtension : Extension&lt;SerializeExtension, T&gt; {

  friend void Serialize(Serializer& s, const T& value) {
    std::apply([&] auto&... fields) {
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
<!-- .element data-auto-animate -->
## Implementation: Unpacking

<pre data-id="code-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T, int FieldCount, typename ExtensionSpec&gt;
struct ExtensionSet;

template &lt;typename T, int FieldCount, typename... Extensions&gt;
struct ExtensionSet &lt;T, FieldCount, TypeList&lt;Extensions...&gt;&gt;
  : Extensions... {




};
</code></pre>

@@@
<!-- .element data-auto-animate -->
## Implementation: Unpacking

<pre data-id="code-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T, int FieldCount, typename ExtensionSpec&gt;
struct ExtensionSet;

template &lt;typename T, int FieldCount, typename... Extensions&gt;
struct ExtensionSet &lt;T, FieldCount, TypeList&lt;Extensions...&gt;&gt;
  : Extensions... {
 private:
  auto UnpackThis() const & {
    return UnpackFields&lt;FieldCount&gt;(static_cast&lt;const T&&gt;(*this));
  }
};
</code></pre>

@@@

## Implementation: Unpacking

```cc[]
template <int FieldCount, typename T>
constexpr static auto UnpackFields(T&& t) {
  if constexpr (FieldCount == -1) {
    constexpr int kNumFields = NumFields<std::decay_t<T>>();
    if constexpr (kNumFields != -1) {
      return UnpackFields<kNumFields>(std::forward<T>(t));
    } else {
      return Error{};
    }
  } else if constexpr (FieldCount == 0) {
    return std::make_tuple();
  } else if constexpr (FieldCount == 1) {
    auto&& [f0] = t;
    return std::forward_as_tuple(f0);
  } else if constexpr (FieldCount == 2) {
    auto&& [f0, f1] = t;
    return std::forward_as_tuple(f0, f1);
  } else if constexpr (FieldCount == 3) {
    ...
```
<!-- .element style="font-size:12pt;" -->
