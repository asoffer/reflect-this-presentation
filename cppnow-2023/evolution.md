## Evolution

> "Weird things are happening with C++20."
<!-- .element class="blockquote1" -->

NOTES:

It turns out that some of the mechanisms we were using for field counting broke in C++20.

We supported counting fields even when the elements were of reference type.

But the technique we were using meant that some compilers would instantiate constexpr copy constructors during our detection even if they were never used.

For types like `std::vector` of something uncopyable this was a hard compilation error.

C++20 posed the choice between supporting uncopyable types and reference types.

We still don't know a way to support both of these.

This is where evolution is critical. We had users that had references in their structs. We were able to "just add the field count."
Not ideal, but the best we could do. And certainly better than having to leave the framework entirely.

@@@

## Evolution
