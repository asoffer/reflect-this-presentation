## Follow along!

[https://asoffer.github.io/reflect-this-presentation/cppnow-2023](https://asoffer.github.io/reflect-this-presentation/cppnow-2023)


<span class="edited">(link available on the conference schedule)</span>

NOTES:

* Follow along.

PAUSE

* I submitted the title for this talk before I knew exactly how I wanted to format it. The title is accurate, but it also could have been...

@@@

## How to design dinosaurs
### <span class="edited_title" style="margin-top:-0.25em; padding-left:0.65em;">^</span>
### <span class="edited_title" style="padding-left:0.4em;">not</span>

<img src="img/could-should.jpeg" class="bordered fragment" data-frament-index="1" />

NOTES:

... "How not to design dinosaurs."

This is a reference to Jurassic Park.

If you haven't seen Jurassic Park, I apologize; the memes sprinkled throughout this talk are probably not going to be meaningful to you.

But there's a famous line in which Jeff Goldblum's character says...

---

... "your scientists were so preoccupied with whether or not they could, they didn't stop to think if they should."

I don't know about you, but this is a pattern I fall into. I get excited by some new language feature and I think "what can I use this for?" when I should really be thinking about the problem I'm trying to solve, rather than the solution I'm trying to apply.

So when I refer to dinosaurs, I'm talking about libraries that are clever and interesting science experiments...

@@@

## How to design dinosaurs
### <span class="edited_title" style="margin-top:-0.25em; padding-left:0.65em;">^</span>
### <span class="edited_title" style="padding-left:0.4em;">not</span>

<img src="img/gate.png" class="bordered" />

NOTES:

...but ultimately might end up being difficult or dangerous to use properly.

So this talk is two things. It's a talk about a mixin library we've designed, but I want to use that library as a case study, to go through a process that helps me make sure I'm not designing a dinosaur.

@@@

## <span style="color:#a00000;text-decoration:line-through"><span class="wrong_content">How I think about design</span></span>
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
* Evaluate: Explore what others have done, both in C++ and other languages. Start to make some decisions.
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
