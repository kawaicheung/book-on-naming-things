If Sivers were to write a notes page about this book, it would be like this:


**Tactics**
* Code shape is important. May mean sacrificing meaning of a variable if that var is scoped to a small area and/or it isn't a 'main player'. (CodeShape)
* Breaking a method apart and impart meaning into the name itself rather than into a more obscure variable. Makes the use of that method far more clear, even though it may mean more methods. That's a good tradeoff. (BreakingMethodsApart)
* Premature abstractions -be OK with the concreteness of a name. It's ok to say exactly what it does if thats what it does -- it's easy to want to get that abstraction too early because that's how we are taught to code. But, it's not worth the cost of understanding the system as it currently exists. (BadAbstractions)
* Edit down names - get rid of words that aren't useful or are unnecssarily embellishing. Only worth embellishing if it distinguishes it from something else in the code. (EditingDownNames)
* Sometimes a method name can go one of two directions -- choose the one where you're more likely to read the method from. (LeadTheNameToTheWellWornPath)
* Verbs in methods can be much more than GET CREATE UPDATE DELETE. (MethodsAsVerbs)



**
* What is the role of delight in code? Argument that delight isn't just a silly ambition - there's real value to it. (AestheticsAndStyle)
* English is a strange language in that we describe the thing before we detail the thing (or we go from most specific to most general in how we name people or dates). It would be better to keep things always general -> specific... that's how we order things in folders. But, we have to live with this weirdness. Fortunately seraching is not as much of a pain because we can search everything with the right tools. (English++)

**On writing process**

* Refactoring is often started or driven by name changes. Here's an example of a typical refactorig to clarify a method and the thought process -- how I get from point a to b via naming (Refactoring)
* Writing ending before beginning - a lot of programming paradigms enforce this. But, it's an unnatural way to create things. (BeginningBeforeEnding)


