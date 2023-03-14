## How should we design a mixin library?

NOTES:
This is the wrong question to ask first.

@@@

## What are our design priorities?

* Performance-critical software
* Software and language evolution
* Code that is easy to read, understand, and write
* Practical safety and testing mechanisms
* Fast and scalable development
* Modern OS platforms, hardware architectures, and environments
* Interoperability with and migration from existing C++ code

NOTES:

I don't claim that these should be everyone's priorities.

@@@

## Software and language evolution

* For the library author.
* For the library user.

## Fast and scalable development

* The API should feel familiar to a user with only a basic knowledge of C++.
* The API should be copy-pastable.

NOTES:

I want to dig into this one.
