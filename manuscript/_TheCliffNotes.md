If Sivers were to write a notes page about this book, it would be like this:

**Why naming matters**
* The basics of naming: Clarity. Constantly has to morph to the changes being  made to the codebase.
* Maybe bring back those 5ish examples from history? (WhyNamingMatters)

**Tactics**
* Code shape is important. May mean sacrificing meaning of a variable if that var is scoped to a small area and/or it isn't a 'main player'. (CodeShape)
* Breaking a method apart and impart meaning into the name itself rather than into a more obscure variable. Makes the use of that method far more clear, even though it may mean more methods. That's a good tradeoff. (BreakingMethodsApart)
* Premature abstractions -be OK with the concreteness of a name. It's ok to say exactly what it does if thats what it does -- it's easy to want to get that abstraction too early because that's how we are taught to code. But, it's not worth the cost of understanding the system as it currently exists. (BadAbstractions)
* Edit down names - get rid of words that aren't useful or are unnecssarily embellishing. Only worth embellishing if it distinguishes it from something else in the code. (EditingDownNames)
* Sometimes a method name can go one of two directions -- choose the one where you're more likely to read the method from. (LeadTheNameToTheWellWornPath)
* Don't use confusing vague verbs (MethodsAsVerbs)
* Use a strong verb to 'scare' the programmer -- Incinerate (MethodsAsVerbs)
* One or two other ideas from this chapter to be split out? (MethodsAsVerbs)
* Avoid method names that are tautologous -- they require you to go into the code because the extracted bit is named for when it should be called. (TheTautologousCodeTrap)
* Memorable names - to avoid the mundanness of a pattern and to help jog your memory of what something is better. (MemorableNames)
* Naming objects/projections. What do manager, helper mean anyway? (ObjectsAsNouns)
* The beauty of booleans -- adds a cleanliness to your object model. Negative booleans not always bad if they are primarily used in that way (OnBooleans)
* Pattern hypnosis -- the danger of a pattern is we just go to it without thinking about a better way... m:m tables. We could be missing an opportunity for clarity (PatternHypnosis)
* Ugly lengthy names in a refactoring are a sign the method is doing too much but we shouldnt mask it with a 'vague name' (UglyNames)
* Teaching section (TBD) 
  * foo/bar/baz are bad. 
  * object myObject = new Object(); is bad
  * params same as input are bad... project.Create(string name, int type) -> can be this.Create(my_id, my_type) so people don't think names need to align.
  

**Non-naming related ideas -> Code delight. Aesthetics. **
* Anemic domain model
* Static helper class as a good placeholder til you find a home
* Ordering things in a method (same param order, validation check, use, etc. in a method) (OrderingThingsInMethods.md)

**Final thoughts**
* What is the role of delight in code? Argument that delight isn't just a silly ambition - there's real value to it. (AestheticsAndStyle) / Programming as an artform (Programming as art)
* English is a strange language in that we describe the thing before we detail the thing (or we go from most specific to most general in how we name people or dates). It would be better to keep things always general -> specific... that's how we order things in folders. But, we have to live with this weirdness. Fortunately seraching is not as much of a pain because we can search everything with the right tools. (English++)

**On writing process**

* Refactoring is often started or driven by name changes. Here's an example of a typical refactorig to clarify a method and the thought process -- how I get from point a to b via naming (Refactoring)
* Writing ending before beginning - a lot of programming paradigms enforce this. But, it's an unnatural way to create things. (BeginningBeforeEnding)

* What makes a name good then? ending piece? (WhatMakesANameGoodThen)

