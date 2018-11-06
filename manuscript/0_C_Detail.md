## The App

In each of the examples in the previous chapter, my revised names attempt to describe a method's implementation more clearly. Some would argue that I'm committing a cardinal programming sin -- one of the fruits of encapsulating logic in methods is to _hide_ the details of the implementation. How dare I try to expose them more!

But, one of the tricks of good method naming is to expose _as much_ detail as is useful. The more knowledge we ascertain from the name of a method, the faster we get at choosing the right methods to use or deciding to add additional ones to fulfill the task-at-hand.

For instance, in the API key generation example above, I felt that `GenerateAPIKey()` was a better choice than `CreateAPIKey()` because it describes _how_ an API key is created with a little more detail that might be useful to the implementor.  

I certainly could've carried the name even further. However, would a name like `GenerateAPIKeyUsingRandomizedGuid()` be better? If there were other API generation mechanisms at play that required alternative methods, and the mechanism mattered outside of the method itself, then absolutely. In my case, there wasn't.

A name like `GenerateAPIKeyUsingRandomizedGuid()` also presents a couple more issues:

1. It's long, and long names can reak havoc on the overall shape of code. By the way, we'll talk about long names and [code shape](#ch_shape) later -- including one case where I believe they actually would be beneficial.

2. It's fragile. If, at any point, I decide to change _how_ the API key is generated, I'd have to remember to change the name as well to avoid misleading the implementor.

In the end, exposing the appropriate level of detail is more art than science. Undoubtedly, as a codebase evolves, what's "appropriate" will also change. Naming--like coding itself--requires constant upkeep. We're never done with naming so long as the product is still evolving.
