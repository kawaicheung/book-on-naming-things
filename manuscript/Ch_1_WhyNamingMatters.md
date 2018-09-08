# Why naming matters

One of the goals of good software writing is clarity. Clear code makes it easier for someone else to figure out what our intentions were when we wrote the code. That "someone else" might even be us a few months from now.

If clarity is the end goal, then naming things well is its primary path; essential no matter where you are in the stack, what language you use, or how nicely you think the code is otherwise written. If you name things well, you write clearer code. 

### Reduced human error

There are many well-documented cases of tragic medical errors that were easily preventable. Often, the error was due to poor handwriting on a prescription or confusing labeling on a medical device. We can put the blame on the pharmacist or medical assistant for misreading things, or we can fix the problem by the way we write and present information.

In the same fashion, many easily preventable bugs are due to poor naming. The more clear, accurate, and memorable we name our constructs now, the harder it is to misuse them later.

When I present this argument, the usual rebuttal is that adequate testing should solve this. Did you write a test for it? Why didn't QA catch this? 

But, code that's well-tested doesn't equate to code that's easy to understand. If a method is named ambiguously, we might use it incorrectly. And, if we happened to not cover that case, we've now introduced a bug. If we have covered that case, we've wasted time and energy that could've been spent elsewhere. 

### The power of renaming

While naming a new construct is important, in practice, we _rename_ existing constructs far more often. 

Ironically, I find this more difficult to do the more familiar I am with a codebase. When I'm _so_ familiar with code, I don't read it in a purely literal way anymore. I've become so accustomed to the names I've chosen that I automatically know what they refer to even if their literal meaning might be misleading. I've become blinded by my own understanding.

Renaming is often better done by someone who hasn't been working with the code intimately. When I inherit an unfamiliar codebase, I start the process of understanding it by reading it literally. As I step through the code to confirm my understanding, I'll go back and rename things that were initially confusing. 

Renaming is also the first step I take in any refactoring because it can expose painpoints without disturbing any mechanics. Here's an egregiously simple example:

```C#
int three = 3;
int four = 5;

assert.IsTrue(three + four == 7); // Why isn't this passing?
```

The names of these variables mislead us into believing the assertion should work. The bug is obvious, but we can expose it without affecting any of the code's mechanics with renaming the variable `four`.

```C#
int three = 3;
int five = 5;

assert.IsTrue(three + five == 7); // Of course this won't pass!
```

Renaming bad names before I dive further into a refactoring can clarify exactly how I should approach that refactoring, or confirm whether a deeper dive is even necessary.

Nowadays, renaming can be done quickly and accurately in most type-safe compiled languages since most development environments offer some sort of renaming tool that allow you to automatically rename all references to a specific construct. 

### Better organizational communication

Names can also help erase the lines between technical and non-technical folks. For instance, if I were building internal software to track the organizational aspects of a law firm, I'd be better off naming elements in my code as close to the words law professionals use in their daily speak. `PracticeAreas` instead of `Topics`. `PracticeGroups` instead of `Teams`. `Clients` instead of `Customers`. `Attorneys` and `Paralegals` instead of `Employees`.

Business concepts also help dictate the kind of structures I introduce. If I were building a patient management system for a medical clinic, I might start with a concept like `List<Patient>` to describe a doctor's patient list. But, if you talk to doctors, they don't call these things patient lists, they call them panels. Doctors talk about how full their _panels_ are or when new openings in their _panel_ will open up.

Knowing this, I'd opt to create a `Panel` class which would house the `List<Patient>` collection and any other attributes specific to a doctor's panel. I might also start with a method like `Panel.Add(Patient p)`. But, doctors usually talk about patients _subscribing_ to a panel. So, `Panel.Subscribe(Patient p)` is the more fluent approach.

By naming concepts in code the way in which real software users would talk about them, we create a ubiquitous language between us and our users.  Our code becomes more approachable to people that aren't directly working with it. More importantly, we can have more productive discussions with other team members on how to improve the software. 

### Maintainability

The ultimate long-term indicator of a well-written codebase is its maintainability. And maintainable code hinges on two main things: scalability and digestibility. Scalable code allows us to support growing demand. We largely achieve this through higher-level architectural decisions. 

Digestible code enables developers to update logic more readily because it's easier to read. And, few practices make for digestible code better than good naming. When these two forces merge, high-level architectural planning and low-level practices, we maximize maintainability.

### Delightful code

Designers use the term “delight” a lot when describing how they want their users to react to their designs. Yet, I've never heard someone say “that code was a delight to read.” Why not? After all, we look at our code for much longer stretches than the average user hangs around an app.

Good writing makes the experience of consuming information more enjoyable. Imagine if a lengthy legal document were written by Hemingway himself -- unadorned with all the usual trappings of “legalese.” Maybe we'd actually read them rather than have a lawyer tell us that “all this means is X, Y, and Z.”

Good code-writing is similar to good prose-writing. A codebase that's well-written is more delightful to read. If it's more delightful to read, a programmer has more intrinsic motivation to work on it and keep it that way.

### The promise

The tricky thing about reading a book on programming is that the book usually focuses on a very specific aspect of programming. When we're coding _for real_, many aspects of programming reveal themselves simultaneously. Like authoring other things, the process of authoring software is circuitous. On the other hand, a book about code uses examples tailor-made to fit the narrative. The argument is usually heavily biased toward that narrative. It's up to us to catalog and consider those examples when we're coding something real.

A book like this is particularly difficult for that reason. Naming things weaves its way through all other aspects of programming -- refactoring, considering what objects should exist, and so forth.

---

One of the hardest parts about explaining coding concepts is choosing the right examples.  A simple example might not do justice to the reason a concept is worthwhile. A too-complex example might not be practical to explain in just a few pages. Examples can be _too_ contrived -- they fit an idea a little too perfectly, when most of programming is trying to negotiate round pegs into square holes.

I wanted to approach this book a bit differently. Naming things well is not about coming up with perfect, pristine names -- it's about making existing code clearer whether or not the code itself is in good shape to begin with. Sometimes, already bad code forces us to name things a certain way even if that way initially feels wrong.  

In this book, we'll look at real-world examples (most coming from code I've personally maintained). There will be a mix of code written from scratch and flawed code that we can push toward better health through better naming. In all the examples, our goal is to fulfill as many of the benefits described above.

Let's begin.
