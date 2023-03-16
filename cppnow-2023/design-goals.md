## <span style="font-size:42pt;">How should we design a mixin library?</span>

NOTES:
This is the wrong question to ask first.

@@@

## <span style="font-size:42pt;"><span style="color:#a00000;text-decoration:line-through"><span style="color:black">How should we design a mixin library?</span></span></span>
### <span style="color:#a00000;font-family:serif;position:absolute;top:0.8em;left:1em;">What are our design priorities?</span>

* Performance-critical software
* Software and language evolution
* Code that is easy to read, understand, and write
* Practical safety and testing mechanisms
* Fast and scalable development
* Modern OS platforms, hardware architectures, and environments
* Interoperability with and migration from existing C++ code

NOTES:

I don't claim that these should be everyone's priorities.

Evolution:
* For the library author
* For the library user

Fast scalable development:
* Familiar if you only have a basic knowledge of C++
* Copy-pastable
