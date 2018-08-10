# Why naming matters

One of the main goals of good software writing should be clarity. We write code that eventually needs to be understood by someone else, even if that someone else is us a few months from now. Clear code makes figuring out the programmer's original intent less daunting.

But, there are other goals we strive for. Sometimes, by their very nature, these goals pull us away from clarity. We choose them to gain something at the cost of some amount of misdirection and obfuscation. But, we reason that the tradeoff is worth it. This is where the heated debates begin.

In a keynote speech at RailsConf 2014, David Heinemeier Hansson argues that many of these goals aren't worth trading away clarity. We blindly overvalue their worth because we are taught that writing code is, first and foremost, a _science_. We are, first and foremost, _engineers_.

This leads us to dogmatic reasoning. If you aren't writing tests first, you aren't writing professional code. Test coverage and other quantifiable metrics dictate the quality of your codebase. Good coding is governed by hard rules like the SOLID principles or the Law of Demeter.

Hansson argues that the kind of programming we do -- building information systems maintained over time -- is rarely science. We are far closer to _writers_  than we are to _scientists_. The principle goal of all good code is clarity -- and clarity isn't a metric achieved by following a set of laws.

---


This is a book about how to name constructs in code better. It is _not_ a book about writing perfect, pristine code itself. In fact, most of the examples in this book show code that is far less than perfect -- realistic code you've probably written yourself or inherited from others. 

Through renaming classes, methods, and properties of imperfect code, you'll see how we can improve upon it immediately. I believe iteratively improving names over time, much like iteratively refactoring code over time, is the only practical way to maintain a viable codebase.

While this book is specifically dedicated to naming, naturally, other programming concepts will occasionally surface. For instance, naming properties in a bloated class in a certain way might expose an opportunity to create a new class. Or, naming a method accurately (if in an ugly way) might reveal that the method is doing too much. Sprinkled throughout this book are other programming principles, but all through the lens of naming things.

## Why does naming matter?

If you've picked up this book already, I probably don't have to convince you why naming things well is a critical aspect of good software writing. But, you may need to convince your manager or other stakeholders who might not reap its immediate benefits. This section might not fully convince them either, but we can both say we tried our best.

Here are a few reasons to invest the time in naming things well.

### A timeless skill

Naming is an essential aspect of every programming language. It doesn't matter where you are in the stack or what kind of programming you do. If you name things well, you write better code. It's a skill set that will never become outdated over time (and if it does, it would mean the robots have officially taken everything over anyways).

### Speed and accuracy

The usual discussions around coding speed and accuracy revolve around quantitative or binary ideas. Have you written tests? How many have you written? What's your code coverage? Why aren't you using that pattern? Why are you using that pattern? We're hypnotized into believing that good code is code that meets a certain score.  We hardly ever talk about the actual words we use to describe the things we build.

But, good naming is another way to make your code less susceptible to human error. The more clear, accurate, and memorable the names of your structures are, the harder it is to mistake the intent of these structures.

### A safe entry-way into refactoring

Renaming exisiting constructs is the simplest kind of refactoring -- the focus is purely on how to describe what's already in place in a more digestible way. It's often the first step I take to any larger refactoring because it both improves readability and exposes painpoints without disturbing any mechanics. Renaming confusing names before you dive further into a refactoring can clarify exactly how to approach that refactoring, or confirm whether it's even worthwhile.

Besides, nowadays, renaming is incredibly quick. There was a time in programming where you would have to find and replace any references to objects if they were renamed. It was a tedious and dangerous task. Nowadays, renaming can be done quickly and accurately in most languages (especially typesafe ones) -- most development environments offer some sort of renaming tool that will update all references to the updated construct automatically. 

### Better communication with everyone in your organization

Another important aspect of naming well is to, as best you can, name concepts in code as they'd be discussed in practice. For instance, if I were building internal software to track the organizational aspects of law firm, I'd want the nomenclature of elements of my software system to be as closely aligned to the words law professionals use in their profession. PracticeAreas instead of Topics. PracticeGroups instead of Teams. Clients instead of Customers. Attorneys and Paralegals instead of, simply, Employees.

Business concepts can also dictate the kind of structures introduced into a codebase. If I were building a patient management system for a medical clinic, I might start with a concept like `List<Patient>` to describe a doctor's patient list. But, I've come to find that doctors don't call these things patient lists, they call them panels. Doctors often talk about how full their panels are or when new openings in their panel will open up.

So, I'm swayed to create a Panel class which would house the List<Patient> collection and any other attributes specific to a doctor's panel. I might also start with a method like Panel.Add(Patient p). But, doctors usually talk about patients subscribing to a panel. So, Panel.Subscribe(Patient p) is the more fluent approach.

By naming concepts in code the way in which real software users would talk about them, we remove a layer of interpretation.  Our code becomes less foreign to people that aren't directly working on it. Software writers can have more productive discussions with other teams within the organization. And, perhaps most importantly, we have a better grasps of the business concepts we have to maintain.

### Another step toward maintainable code

The ultimate long-term indicator of a well-written codebase is its maintainability. And maintainable code hinges on two main things: scalability and digestibility. Scalable code allows us to support growing demand. We largely achieve this through higher-level architectural decisions. 

Digestible code enables developers to update logic more readily because it's easier to read. And, few practices make for digestible code better than good naming. When these two forces merge, high-level architectural planning and low-level practices, we maximize maintainability.

### Delightful code

Designers use the term “delight” a lot when describing how they want their users to react to their designs. Yet, I've never heard someone say “that code was a delight to read.” Why not? After all, we look at our code for much longer stretches than the average user hangs around an app.

Good writing makes the experience of consuming information more enjoyable. Imagine if a lengthy legal document were written by Hemingway himself -- unadorned with all the usual trappings of “legalese.” Maybe we'd actually read them rather than have a lawyer tell us that “all this means is X, Y, and Z.”

Good code-writing is not a far reach from good prose-writing. We can make our lines more delightful by the way we implement functionality and by the way we name things. A codebase that's already well written has a much greater chance of staying that way; one that's poorly written will most likely continue to get worse over time.

[Software developers are more diverse than ever -- it's no longer just ‘science geeks'. The culture of development is changing (open source too). Need to emphasize human communication more than ever.]


### What this book is not...
[I don't know what it is about us software developers. We argue about good code vs bad code by referencing principles...]


* Recipe / Rules
* Consistency -- All names should have this form... --- rather it's about creating more readability (use these and APPLY them consistently)


## Coding as writing

In code, objects and methods create actions. Actions, when stitched together in a certain way, shape larger components. Components then become the building blocks for the entire application. 

Coding sounds an awful lot like writing, doesn't it? Nouns and verbs create sentences, sentences shape paragraphs, paragraphs build chapters, and so forth.

This section approaches better naming by thinking about it in the way we'd think about writing good prose. Is the name of an object clear and unambiguous? While the individual words feel right, does the entire line read coherently?


## DHH Rails 2014 keynote:


Forensics, much more like reading literature than a 'hard science'.

Discovering Laws of Programming -> "Law of Demeter" . Lull into belief that what we do is 'science'.

PseudoScience.

We are all insecure about parts of our codebases.

TDD -> 

Engineering/Computer Science lens...makes 'laws' and 'rules' more important.

Programming information systems...  METRICS. BOOM. 

"Just because somethig is easy to measure doesnt mean its important"

Science is clear objective truth.

Software engineering is a hard-hat you wear occasionally. Performance testing for example.

"Software writer"

CLARITY OF THE CODEBASE ABOVE ALL ELSE.

There's no one formula -- "I'll know it when i see it." "Develop an eye."


Writing code is just a 'draft'.

