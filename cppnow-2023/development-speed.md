## Development Speed

> "Can you make &nbsp;`Extend`&nbsp; print its field names?"
<!-- .element class="blockquote2" -->

@@@

## Development Speed

```cc[]
template <typename T>
struct PrintingExtension {
  friend std::ostream& operator<<(std::ostream& os, const T& value) {
    std::string_view separator = "{ ";

    std::apply([&](auto&... fields) {
      ((os << std::exchange(separator, ", ") << fields), ...);
    }, Unpack(value));

    return os << " }";
  }
};
```
<!-- .element style="font-size:14pt;" -->

NOTES:

* Our original implementation looked like this, but it was definitely lacking.
With TUPLE_DEFINE_STRUCT, people had access to the field names.

* Had some ideas, but they needed compiler hooks, and Clang's hooks were not
  sufficiently robust that we were comfortably relying on them.

* Aside: `std::exchange` trick. First application writes the open brace. After
  that `separator` is repeatedly overwritten with the comma.

@@@

## A wild zygoloid appeared!

<img 
  style="margin:0.2em; border:1px solid #000; border-radius:0.2em; width:60%" 
  src="img/builtin_dump_struct_1.png" />
<img
  class="fragment"
  style="margin:0.2em; border:1px solid #000; border-radius:0.2em; width:60%" 
  src="img/builtin_dump_struct_2.png" />

<!--<a href="https://www.reddit.com/r/cpp/comments/100x37a/clang15_builtin_dump_struct_got_a_much_needed/">Reddit</a>-->

NOTES:

* `__builtin_dump_struct` was something we considered using but in was not very robust.
* It gives a debug representation of the fields in a struct by repeatedly calling a user-provided printf-like function.
* It has access to to the field names at compile-time!

---

* I love these comments so much. Yes you can use this for reflection. Yes, you
  do need to parse the input.

@@@

## Development Speed

```cc[4,10-14]
template <typename T>
struct PrintingExtension {
  friend std::ostream& operator<<(std::ostream& os, const T& value) {
    static const auto field_name_info = GetFieldNameInfo(value);

    std::string_view separator = "{ ";

    if (field_name_info.success) {
      std::apply([&](auto&... fields) {
        size_t i = 0;
        ((os << std::exchange(separator, ", ") 
             << field_name_info[i++] << " = "
             << fields),
         ...);
      }, Unpack(value));
    } else {
      ...
    }

    return os << " }";
  }
};
```
<!-- .element style="font-size:10pt; width:64%;" -->

NOTES:

* Why not constexpr? We need a value to be passed and we don't have a good way to produce one otherwise. Default construction is maybe reasonable, but we don't want this extension to require constexpr default constructibility.

@@@

## Development Speed

```cc[|6]
template <typename T>
auto GetFieldNameInfo(const T& value) {
  std::array<std::string_view, kFieldCount> field_names;
  ParsingState state;
  __builtin_dump_struct(std::addressof(value),
                        PrintfHijack
                        state,
                        field_names);
  return FieldNameInfo{
    .field_names = field_names,
    .success = (state.index = kFieldCount),
  }
}();
```

NOTES:

* Pass our version of printf in which updates parsing state and fills in field names.

@@@

## Development Speed

```cc[]
int PrintfHijack(ParsingState& state,
                 std::span<std::string_view> fields,
                 const char *format, ...) {
  std::va_list va;
  va_start(va, format);
  ...
  fields[state.index++] = va_arg(va, const char*);
  ...
}
```

NOTES:

I'm leaving a lot out here, because it's not worth diving into the precise details of how `__builtin_dump_struct` works, but I hope this is enough to whet your appetite for the approach.
