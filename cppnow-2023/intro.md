[https://github.com/asoffer/reflect-this-presentation](https://github.com/asoffer/reflect-this-presentation)

NOTES:

* Follow along.
* Questions during.
* Slide numbers.
* If you're looking to find after the talk, I don't really have a social media presence, so you're going to have to find me in person.

@@@

## <span style="color:#a00000;text-decoration:line-through"><span style="color:black">How I think about design</span></span>
### <span class="edited_title">Talk outline</span>


1. What problem are we solving, and why?
1. What are our priorities?
1. Evaluate the design space.
1. Implement it!
1. Iteratively improve upon our design.

NOTES:

* Talk structured in the same way I think about design.
* Problem: If you can't articulate what the problem is *and* why it's a problem, you'll never convince anyone it's worth solving. -- If you're familiar with mixins, you already understand this, but bear with me.
* Priorities: Important to state explicitly. Tradeoffs.
* Evaluate: Because tradeoffs, you'll want to explore several different directions. This is where you start to make some design decisions.
* Implement: Only once you understand the problem space.
* Iterate: You'll get it wrong the first try.

@@@

## "I did it all myself!"
### <span class="edited_title">...said no one ever.</span>


<div>
<div style="width:50%; float:left">

> Sam Benzaquen
<!-- .element class="name1" -->

> Neema Ebrahim-Zadeh
<!-- .element class="name2" -->

> Greg Falcon
<!-- .element class="name1" -->

</div>
<div style="width:50%; float:right">

> Daisy Hollman
<!-- .element class="name2" -->

> Richard Smith
<!-- .element class="name1" -->

> Zie Weaver
<!-- .element class="name2" -->
</div>
</div>

NOTES:

* I'd be remiss if you came away from the talk thinking this work was just mine. That couldn't be further from the truth.
* In reality this work is the culmination of tons of discussions and design and implementation work from many engineers.
* These are the people who have really contributed in a way that fundamentally changed the direction of the library we're going to talk about.
* There are many others who have contributed via bug fixes or implementation improvements.

* I want to call out Zie Weaver in particular. She has done a huge amount here and is responsible for the migration of an existing library to this one.
* She's speaking immediately after this about the migration. If you're interested in learning about what I believe is the most complex large scale refactoring effort ever attempted in C++, I'd encourage you to attend her talk.
