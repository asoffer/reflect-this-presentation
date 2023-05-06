## We're done!

@@@

## We're <span style="color:#a00000;text-decoration:line-through"><span style="color:black">done</span></span>!
### <span class="edited_title">ready to get user feedback</span>

<div>
<div style="width:50%; float:left">

> **Evolution:**
> "Weird things are happening with C++20."
<!-- .element class="fragment blockquote1" data-fragment-index="1" style="font-size:16pt !important; width:90%"-->

> **Saftey & Testing:**
> "Adding an unusable extension still compiles."
<!-- .element class="fragment blockquote2" data-fragment-index="3" style="font-size:16pt !important; width:90%"-->

</div>
<div style="width:50%; float:right">

> **Understandability:**
> "Using &nbsp;`Extend`&nbsp; with my class gives me useless error messages."
<!-- .element class="fragment blockquote2" data-fragment-index="2" style="font-size:16pt !important; width:90%" -->

> **Development Speed:**
> "Can you make &nbsp;`Extend`&nbsp; print its field names?"
<!-- .element class="fragment blockquote1" data-fragment-index="4"  style="font-size:16pt !important; width:90%"-->

</div>
</div>

NOTES:

What I've shown here is mostly our first attempt, but there were a bunch of things that didn't quite work out as well as we wanted.

---

Some types compiled fine in C++17 but failed with C++20

---

There were common situations that produced really bad error messages.

---

We found instances where types would compile when they requested equality, even though their type had no viable equality operator.

---

And the most requested feature of all: Could we make the printing extension automatically include field names?
