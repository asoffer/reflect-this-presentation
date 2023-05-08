## Follow along!

[https://asoffer.github.io/reflect-this-presentation/cppnow-2023](https://asoffer.github.io/reflect-this-presentation/cppnow-2023)


<span class="edited">(link available on the conference schedule)</span>

NOTES:

* Follow along.
* Slide numbers.
* If you're looking to find me after the talk, I don't really have a social media presence, so you're going to have to find me in person.

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

If you haven't seen Jurassic Park, I apologize; the memes throughout this talk are probably not going to be meaningful to you.

---

But there's a famous line in which Jeff Goldblum's character says "your scientists were so preoccupied answering whether they could, they didn't stop to think if they should."

And I think this is somewhat common with design. We all get excited about how we're going to use or abuse some cool new feature, and we lose sight of whether the thing we built is addressing the problem we set out to solve in the first place. So when I refer to dinosaurs metaphorically, I'm talking about libraries that are clever and interesting science experiments, but ultimately might do more harm to users than the value they provide.

I want to be clear that I fully endorse trying new things. Designing dinosaurs is okay. This sort of experimentation really pushes the state of the art forward. But it's important that we don't conflate this sort of experimentation with a more user-centric design focus. We don't want to accidentally let dinosaurs out into the wild.

So this talk is about that other kind of design. It's about designing something when you specifically aim to not make a dinosaur. We're going to walk through the design process of a mixin library we built. We are going to dive into the implementation, but we're going to spend a significant amount of time thinking about the higher-level design.

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
