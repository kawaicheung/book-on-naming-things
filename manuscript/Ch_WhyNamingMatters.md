# Why naming matters

One of the goals of good software writing is clarity. We write code that someone else must understand. That "someone else" might even be us a few months from now. Clear code makes it easier to figure out the programmer's original intent.

If clarity is the end goal, then naming things well is its primary path; essential no matter where you are in the stack, what language you use, or how nicely you think the code is otherwise written. If you name things well, you write clearer code. 

### Reduced human error

The usual discussions around creating bug-free code revolve around quantitative ideas. Have you written tests? How many have you written? What's your code coverage? We're hypnotized into believing that good code is code that meets a certain metric.

But, well-tested code can also be hard to digest. If a method is named ambiguously, a developer might use it incorrectly. And, if we happened to not cover that case, they've introduced a bug. This makes fundamentals like good encapsulation, rigid access modifiers, and adhering to other first principles essential.

But, good naming is another way to make your code less susceptible to human error. The more clear, accurate, and memorable the names of your constructs are, the harder it is to mistake the intent of these constructs.

### A first-step into refactoring

Renaming existing constructs is the simplest kind of refactoring. It's often the first step we take to any larger refactoring because it both improves readability and exposes painpoints without disturbing any mechanics. Renaming bad names before you dive further into a refactoring can clarify exactly how to approach that refactoring, or confirm whether a deeper dive is even necessary.

Besides, nowadays, renaming is incredibly quick. There was a time where you would have to find and replace any references to objects if they were renamed. It was a tedious and error-prone task. Nowadays, renaming can be done quickly and accurately in most languages (especially typesafe ones) since most development environments offer some sort of renaming tool that will rename all references to the updated construct automatically. 

### Better organizational communication

Good naming helps improve the communication of a larger team. For instance, if I were building internal software to track the organizational aspects of law firm, I'd want the names of elements of my software system to be as closely aligned to the words law professionals use in their profession. `PracticeAreas` instead of `Topics`. `PracticeGroups` instead of `Teams`. `Clients` instead of `Customers`. `Attorneys` and `Paralegals` instead of `Employees`.

Business concepts can also dictate the kind of structures introduced in a codebase. If I were building a patient management system for a medical clinic, I might start with a concept like `List<Patient>` to describe a doctor's patient list. But, I've come to find that doctors don't call these things patient lists, they call them panels. Doctors often talk about how full their panels are or when new openings in their panel will open up.

Knowing this, I'd opt to create a `Panel` class which would house the `List<Patient>` collection and any other attributes specific to a doctor's panel. I might also start with a method like `Panel.Add(Patient p)`. But, doctors usually talk about patients _subscribing_ to a panel. So, `Panel.Subscribe(Patient p)` is the more fluent approach.

By naming concepts in code the way in which real software users would talk about them, we remove a layer of interpretation.  Our code becomes less foreign to people that aren't directly working with it. Software writers can have more productive discussions with other teams within the organization. And, perhaps most importantly, we have a better grasps of the business concepts we have to maintain.

### Maintainability

The ultimate long-term indicator of a well-written codebase is its maintainability. And maintainable code hinges on two main things: scalability and digestibility. Scalable code allows us to support growing demand. We largely achieve this through higher-level architectural decisions. 

Digestible code enables developers to update logic more readily because it's easier to read. And, few practices make for digestible code better than good naming. When these two forces merge, high-level architectural planning and low-level practices, we maximize maintainability.

### Delightful code

Designers use the term “delight” a lot when describing how they want their users to react to their designs. Yet, I've never heard someone say “that code was a delight to read.” Why not? After all, we look at our code for much longer stretches than the average user hangs around an app.

Good writing makes the experience of consuming information more enjoyable. Imagine if a lengthy legal document were written by Hemingway himself -- unadorned with all the usual trappings of “legalese.” Maybe we'd actually read them rather than have a lawyer tell us that “all this means is X, Y, and Z.”

Good code-writing is similar to good prose-writing. A codebase that's well-written is more delightful to read. If it's more delightful to read, a programmer has more intrinsic motivation to work on it and keep it that way.

### The promise

One of the hardest parts about explaining coding concepts is choosing the right examples.  A simple example might not do justice to the reason a concept is worthwhile. A too-complex example might not be practical to explain in just a few pages. Examples can be _too_ contrived -- they fit an idea a little too perfectly, when most of programming is trying to negotiate round pegs into square holes.

I wanted to approach this book a bit differently. Naming things well is not about coming up with perfect, pristine names -- it's about making existing code clearer whether or not the code itself is in good shape to begin with. Sometimes, already bad code forces us to name things a certain way even if that way initially feels wrong.  

In this book, we'll look at real-world examples (most coming from code I've personally maintained). There will be a mix of code written from scratch and flawed code that we can push toward better health through better naming. In all the examples, our goal is to fulfill as many of the benefits described above.

Let's begin.
