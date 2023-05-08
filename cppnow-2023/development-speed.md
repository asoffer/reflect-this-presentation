## Development Speed

> "Can you make &nbsp;`Extend`&nbsp; print its field names?"
<!-- .element class="blockquote2" -->

NOTES:

* Because the existing TUPLE_DEFINE_STRUCT library is a macro that wraps the struct fields, it has access to the field names when printing.
* The `PrintingExtension` doesn't have that luxury.

@@@

## Development Speed

```cc[]
template <typename T>
struct PrintingExtension : Extension<PrintingExtension, T> {
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

* This was our original implementation. It would print the fields as a comma-separated list.

* Aside: `std::exchange` trick. First application writes the open brace. After
  that `separator` is repeatedly overwritten with the comma.

* So this is where we were. Then we found out about ...

@@@

## Development Speed


<div style="column-count: 2">

```cc[]
__builtin_dump_struct(&some_struct,
                      some_printf_func,
                      args...);
```
<!-- .element style="margin-top:7em; width:100%;" -->

<img src="img/clever.webp" class="bordered" style="width:80%" />

</div>

NOTES:

* ...Clang's `__builtin_dump_struct`, and we got really excited. This is a compiler intrinsic used primarily for debugging.
* it works by repeatedly invoking a user-provided printf-like function.
* And I'm more than happy to write something that looks like printf, but actually just stashes information passed to it.
* The problem was, it wasn't terribly robust. If you tried to do anything remotely interesting, the compiler would crash.
* But then...

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

* ...`__builtin_dump_struct` improved significantly, to the point where we were comfortable relying on it.

---

* When I took this screeshsot, this was the top voted comment, and I love it so much. READ COMMENTS
* This is almost exactly what we're going to do.

@@@

## Development Speed

```cc[4,10-14]
template <typename T>
struct PrintingExtension : Extension<PrintingExtension, T> {
  friend std::ostream& operator<<(std::ostream& os, const T& value) {
    static const auto field_name_info = GetFieldNameInfo(value);

    std::string_view separator = "{ ";

    if (field_name_info.success) {
      std::apply([&](auto&... fields) {
        size_t i = 0;
        ((os << std::exchange(separator, ", ") 
             << field_name_info.field_names[i++] << " = "
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

* This is how we modified the printing extension. We call `GetFieldnameInfo` to extract the field names and then iterate through them in the fold expression.
* It's worth noting that we're not doing this at compile-time. It's a function-local static.
* The reason for this is that we do not know if the type is default constructible. We need to pass a pointer to a valid object to `__builtin_dump_struct`, but we don't know how we could construct one. So we wait until the first time this is called with an actual value and use that. We don't actually care what the value is, we just need someone else to construct one for us.

@@@

## Development Speed

```cc[]
template <typename T>
auto GetFieldNameInfo(const T& value) {
  std::array<std::string_view, FieldCount<T>> field_names;
  ParsingState state;
  __builtin_dump_struct(std::addressof(value),
                        FieldNameExtractingPrintf
                        state,
                        field_names);
  return FieldNameInfo{
    .field_names = field_names,
    .success = (state.index = kFieldCount),
  }
}();
```

NOTES:

* This is what `GetFieldNameInfo` looks like. We create an array that is going to hold the field names and a `ParsingState` object. And then we call `__bultin_dump_struct` passing it `FieldNameExtractingPrintf`
* `__builtin_dump_struct` will repeatedly call `FieldNameExtractingPrintf` with these parameters followed by a format string and arguments to pass to the format string.

@@@

## Development Speed

```cc[]
int FieldNameExtractingPrintf(
    ParsingState& state,
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

* I'm leaving a lot off this slide, but I hope this gives you the gist of whats happening.
* Each time our function gets called with a field name we update the index corresponding to the field we are processing, and stash its field name in `fields`.
* In reality there's a lot more going on. Because `__builtin_dump_struct` recurses into all sub-structs, but we only want to consider the top level field names, we need to track our depth in that instantiation as well.
* But I hope this whets your appetite for the approach.
