## Let's start designing!

NOTES:
Now that we know what the problem is we might be tempted to start thinking about solutions.
But we're not ready for that yet.

@@@

##  <span class="crossed_out"><span class="wrong_content">Let's start designing!</span></span>
### <span class="edited_title">What are our design priorities?</span>

<img class="fragment bordered" width="80%" src="img/chandler-priorities.png" />

NOTES:

We know what the problem and why it's worth addressing, but no solution is perfect.
We want to have an understanding ahead of time of what is critical and where we can compromise.

The good news is you usually only have to do this once. Design goals can often be carried over from project to project.

---

* Chandler Carruth and Titus Winters spoke in 2019 about their design priorities for C++.
* There's a lot in here that generally matches my own design sensibilities, but not everything applies. 
* Starting point.
* Highlight 4 of these in particular.

@@@

##  <span class="crossed_out"><span class="wrong_content">Let's start designing!</span></span>
### <span class="edited_title">What are our design priorities?</span>

* Evolution
* Understandability
* Safety & Testing
* Development Speed

NOTES:

Evolution:
* For the library author
* For the library user

Understandability
* Familiar if you only have a basic knowledge of C++

Safety & Testing
* Pit of success

Development speed
* The primary goal

I don't claim that these should be everyone's priorities.

It's worth highlighting that I took performance off the list. It's not that I don't care about performance. I do. It's that runtime performance did not play into any of our design decisions.
Compile-time performance was a concern, but ultimately was not bad enough to be problematic.
