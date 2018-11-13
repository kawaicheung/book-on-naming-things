## Objects as Nouns

In the previous chapter, we took a look at picking the right names to describe methods. Now, let's talk about naming objects.

## What do managers do all day?

There are nouns in the English language that have taken on designated roles in "software speak". Even if you aren't intimately familiar with their meanings, you know they exist. I'm talking about the _Handlers_, _Managers_, and _Helpers_ of the world.

These terms have an appeal because they fit that perfect intersection of being somewhat ambiguous, sounding fairly technical, and having a history of use cases across various languages and frameworks. If other people are using them, they must be good, right? The problem is they provide little useful meaning. 

They are the words we use to name a class that has some collection of methods behind it, but really isn't a tangible “thing” unto itself. For example, a class isn't specifically a `Product`, but it does things to `Products`, so, `ProductManager` feels appropriate. It's just the right word to encapsulate the idea completely, but it doesn't reveal much more about why we'd want to use that class.

Sometimes, these generically-named classes are a code smell. Perhaps, the methods within a `TaskManager` class are better suited inside a `Task` class that likely holds most of the relevant properties the `TaskManager` class needs anyways. Let the object do its own work rather than delegate that work to some extraneous, ill-defined manager!  

The side-effect of building classes like this is we end up with other classes that hold nothing more than a bunch of properties. Martin Fowler has a name for this type of pattern. He calls it the anemic domain model. I best leave it to Martin to describe the problem:

> The basic symptom of an Anemic Domain Model is that at first blush it looks like the real thing. There are objects...and these objects are connected with the rich relationships and structure that true domain models have. The catch comes when you look at the behavior, and you realize that there is hardly any behavior on these objects, making them little more than bags of getters and setters.

> The fundamental horror of this anti-pattern is that it's so contrary to the basic idea of object-oriented design; which is to combine data and process together...What's worse, many people think that anemic objects are real objects, and thus completely miss the point of what object-oriented design is all about.

Admittedly, many generic software patterns promote this style of separation, like the Service/Repository pattern. In this case, the naming convention for the various players follow forms like `TaskService` or `AccountRepository`.

When the convention and pattern is already built-in, it makes sense to follow a naming convention. The use of generic extenders like `Service` and `Repository` on classes informs the role that class plays in the pre-defined architecture. Those words have a specific meaning -- they aren't just an empty label.

However, when there's no explicit architecture in place and we're building constructs custom to the application we're working on, using generic extenders can hurt. If we can't easily refactor our way out of it (i.e. moving `TaskManager` methods into the `Task` itself), we can at least be particular about the names we choose. 

I found a good example of this, taken from a David Heinemeier Hannson blog post on building Basecamp. In his example, ….

If you're struggling to think of a more descriptive name for a helper class, it may be a sign the class contains methods that are doing too much. Look for natural groupings of functionality, split the methods out into separate classes, and name each class appropriately. 

### An homage to the static helper class

Often times, I'll start with a `Helper` class when I'm not exactly sure where to park some bit of functionality. But, once that class grows to more than a few methods, I'll look back to see what can be extracted away from it. 

*Can we split up a bucket of methods into smaller objects that have a more specific purpose?






