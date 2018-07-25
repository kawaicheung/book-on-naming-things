## The dogma of software principles

When I talk to other programmers about why code should be written in one way or another, I often get a response like “If you don’t do it this way, you violate the Single Responsibility Principle” or “Because no method should be longer than 20 lines of code.”

Following someone’s law or principle shouldn’t be the explanation for taking an approach. This kind of reasoning is dogmatic -- it doesn’t help a programmer understand “why”. Some will argue that these well-known principles became well-known principles because they almost always lead to more readable, maintainable code. But, the discussion should first be about why an approach is more readable.

For example, a method here’s a method doing too much:

[Show example]

After some iterations of refactoring, we get to this:

[Show refactored example]

Why is this better exactly? In part, it’s because we’ve reduced the lines of code down a bit. But, the primary gain is that the method now reads more fluently. Instead of being mired in implementation logic, we’ve wrapped up this logic into smaller methods. Smaller methods give us the opportunity to apply a name to this logic. We can call it something tangible.

[Sometimes, it may not be worth ‘the principle’...tradeoff is its more complex and depth-oriented]

There are times where such refactorings aren’t so clean. A codebase that’s already gone off the tracks might require a lot of small, sequential steps, slowly molding the shape of the code back to something elegant. What do we do then?

[A small step would be using something like “#regions” in C# to give more meaning and apply names to areas of code before we go thru the extensive refactoring]

[Sometimes, I stop at regions...I actually like having everything flat in one place and i’m not yet ready to start driving things into separate smaller constructs. Naming is what drives my decisions, my opinions on how easy the code is to work with]

The reason we choose one approach over another should not be about a principle -- it should be about why one approach makes the code easier to read and maintain. Principles might be nice reference points -- they offer a quick way of expressing a common approach. But, they are too often employed as the reason an approach is better without considering all the other real-world tradeoffs. In some cases, they might reshape a simple codebase into something unnecessarily complex. In other cases, they just might not give a return worth the effort.  When the discussion is about readability and maintainability, then, we make good, thoroughly considered decisions.
