## <span style="font-size:42pt;">How should we design a mixin library?</span>

NOTES:
This is the wrong question to ask first.

We know what the problem is and the tool we're going to use to address it, but no solution is perfect. We want to have an understanding ahead of time of what is critical and where we can compromise.

@@@

## <span style="font-size:42pt;"><span style="color:#a00000;text-decoration:line-through"><span style="color:black">How should we design a mixin library?</span></span></span>
### <span class="edited_title">What are our design priorities?</span>

<img class="fragment bordered" width="80%" src="img/chandler-priorities.png" />

NOTES:

There's a lot in here that generally matches my own design sensibilities, but not everything applies. I want to highlight 3 of these in particular.

@@@

## <span style="font-size:42pt;"><span style="color:#a00000;text-decoration:line-through"><span style="color:black">How should we design a mixin library?</span></span></span>
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
* Copy-paste-able

Safety & Testing
* Pit of success

Development speed
* ???

I don't claim that these should be everyone's priorities.
It's worth highlighting that I took performance off the list. It's not that I don't care about performance. I do. It's that runtime performance did not play into any of our design decisions.
Compile-time performance was a concern, but ultimately was not bad enough to be problematic.
