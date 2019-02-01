## INCOMPLETE: Fluent interfaces

* Defined by Fowler in 2005: https://martinfowler.com/bliki/FluentInterface.html
* Great to use (in certain scenarios) but the names make no sense in isolation.
* Favors the USE scenario vs. editing scenario (it makes more sense to use it than when you're editing the code)
* So, probably has good scenarios where to employ them:
  * Timespan stuff
* Method chaining vs. fluent interface (List manipulation is still more method chaining...but what if we converted it to more fluenty like myList.OrderBy(a=>a.).ThenBy(a=>a.)....IEnumerable on first, IOrderedEnumerable on second.
* What's the point? Programmer happiness... DHH.
* a.forty-two
* Really creating a language for the end user (the programmer using your library) even though it's a strange concept to maintain as the creator.
