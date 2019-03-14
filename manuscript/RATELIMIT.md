
When I'm building a new feature, I tend to write code like a sculptor. I start with something I need to build. If I'm lucky, I intimately know the details of what this code should ultimately do. I start with the big picture--sculpting out what feel like the necessary objects and methods that will get me where I need to go.

Once I have the big pieces in place, I start to mold them. There's no precise order of execution during this initial build phase. I mold here, morph over there. I generally let my programming senses guide me for awhile, kneading the thing until the code does what I want it to do.

But, does the code feel right? Code that works and code that feels right aren't the same thing. There's no definitive checklist for when my code feels right. I know it when I see it. It's usually heavily driven by how these things are named.

I've always wanted to capture this actual act of morphing code into the right shape live somehow. But, a live feed into my screen might be boring. Like filming in the safari, it might be hours before the slightest interesting thing happens. Or, all at once, the meat of the action comes together -- of course, when the crew isn't filming.

Nevertheless, I'll give this an attempt in written words.

---

I need to set a rate limit on API usage. I want to prevent an authenticated user from making more than 1000 requests to the API in any one hour period. There are more than a thousand ways to implement this. And so, it's a good example of having a piece of clay you can mold in many ways. Here's how I went about it. Along the way, pay attention to how naming and the actual code drive each other.

To start, I alreadt know I want some kind of object to track rate limits. The key components for such an object are the current number of requests, the maximum number allowed, and the timeframe it's allowed within.
