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
template <typename... Us, typename... Ps>
constexpr auto FindAllDependencies(
    TypeList<Us...> unprocessed, TypeList<Ps...> processed) {
  if constexpr (sizeof...(Us) == 0) {
    return processed;
  } else {
    using first_t = First<Us...>;
    using tail_t = Tail<Us...>;
    if constexpr ((std::is_same_v<first_t, Ps> || ...)) {
      return FindAllDependencies(tail_t{}, processed);
    } else if constexpr (requires { typename first_t::deps; }) {
      return FindAllDependencies(tail_t{} + typename first_t::deps{},
                                 TypeList<first_t, Ps...>{});
    } else {
      return FindAllDependencies(tail_t{}, TypeList<first_t, Ps...>{});
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
## Implementation: Field counting

* [Better C++14 Reflections](https://youtu.be/UlNUNxLtBI0)
* [Cute C++ Tricks: Part 2 of N](https://youtu.be/EwEppzQe5Oc)
