# Introduction

If you've written code long enough, there's a good chance you're familiar with this quote, attributed to Phil Karlton.

> There are only two hard things in Computer Science: cache invalidation and naming things.

Karlton may be the only well-known person whose fame almost entirely derives from a single quote, because other than this quote, I had never heard of Mr. Karlton before. It turns out he was a developer at Netscape back in the day and I'm sure he did great work. But, for better or worse, it's this quote that he will be forever remembered by in the programming industry.

Exactly when I first heard this quote, I do not recall. Though, I remember chuckling to myself. First, because I can think of many other things that are difficult for me in programming. Second, because naming wasn't initially among those things; I had never explicitly thought about _naming things_ as being hard. 

Of course, it can be hard--sometimes excrutiatingly so. But, unlike so many other things in programming, a bad (or good) name won't be caught by the compiler. I've never had a warning message pop up after I `CTRL+SHIFT+B`'d saying "The name of your method `foo()` leaves something to be desired". There are no metrics for naming. A bad name won't break your code. A good name won't speed up your build.

Yet, we all have written and read names that confuse, misdirect, conflate, or otherwise cause great harm to us--the humans that have to maintain all these lines of instruction. So, how do you name things well? It's something I've become incredibly passionate about over the years, so much so that I decided to dedicate an entire book to this strange niche of programming topics---all of this stemming from a quote attributed to the world-famous Phil Karlton. 

I thought for a long time about how to write a book about naming things. Most other technical books flow like a series of recipes. An abstract topic is introduced, followed by some concrete examples, and ending with some bullet-point takeaways. Each chapter tends to build upon the previous one. The author _is_ the authoritative voice on the subject, steering you away from the obvious wrongs and toward the clear rights. 

But, I don't think that approach works well for a topic like this. Naming well is elusive. It has a lot to do with gut, feel,  style and even _aesthetics_. It is, in my humble opinion, the most _non-technical_ of technical subjects. Naming is an art. Good naming has a whole lot of subjectivity.

So, the approach I've taken to this book is simply the way I would talk about naming if I were working alongside you as a fellow programmer. I've studied a handful of real problems I've had with code in recent years. Sometimes, the naming part itself was the problem; Other times, it was the naming part that got me _out_ of the problem. In each of these cases, coming up with, what I thought, was the _right_ name required all of that non-technical stuff I just mentioned. 

Naming well isn't a binary exercise, and, I won't treat it like one in this book. I hope by reading this you get some better ideas for _how_ to approach naming in your own code. I don't want (nor expect) you to agree with all of my ideas in this book. Rather, I think it's having the ideas out there for discussion that is paramount.

> There are only three hard things in Computer Science: cache invalidation, naming things, and writing a book about naming things.

-Ka Wai Cheung
March, 2019

===

Writing code is different from any other kind of writing. Other writing _ends_. A tweet is sent. A novel is published. A blog post is released. You can edit most writing from a fixed position. But, writing code is a fluid kind of act. It is constantly iterated upon even after the software finally launches. You code knowing that it may eventually change again. 

You also code for two different audiences. The first is the compiler or interpreter--the thing that is actually turning your instruction into something else. The second is the human--the person who's reading your stuff so they too can write for the first audience.

So, to get your code to "work" is one thing. To get your code to be understandable is a whole other thing. As Brandon Rhodes once said, "One of the biggest sins you can commit is to stop programming when it works."

How you name things is the largest determining factor for how understandable your code is. Because code is never done, the names of things provide all the _meaning_ of the parts in that exact moment in time. Small changes in code might require large changes in naming.

As a codebase does more complex things (and all codebases do more complex things over time) it will naturally accrue more imperfections, bottlenecks, and general "ickyness" in its architecture. The names of things become even more critical to the understanding of imperfect code. They become the guideposts to a successful refactoring.

Not all names should aim for the same goals, either. Some names might describe and guide. Some names might be easy to scan. Some names might be memorable. Some names might simply get out of the way. It all depends on how the names of things, together, piece the entire story of your codebase together. 

This makes writing code the strangest kind of art, and naming things one of the more difficult skills in the craft.


**Why naming matters**

We all have opinions on how to write better code. The debates go on interminably. 

Usually, these opinions revolve around meaty concepts like frameworks, patterns, and architectures. But, these concepts usually come with a price: They're large in scope. 

You can't just pour a new framework into an existing codebase like wet cement--it's _detailed_ work. It's also hard to unwind that work once you've started. You might spend days implementing something only to realize the result wasn't nearly worth the effort.

On the other hand, changing the name of something is fast. It gives you immediate feedback. It doesn't require additional infrastructure. If a name doesn't feel right, you can change it again. You can continually improve a name without being stuck with your original decision. 

The returns are equally enticing.

Renaming something well can clarify what was previously ambiguous about it. A descriptive name might expose a problem you didn't see before. The right name makes it easier for someone else to figure out what your intentions were when you originally wrote the code. That someone else might even be you a few months from now. 

Unlike a book or movie script, there is no final draft of a codebase. A codebase is an ever-evolving manuscript that requires as much attention over the words we use as the functions we create. Writing code is one of the most complex forms of communication today. And given the pace of change with programming languages, paying attention to naming is more critical than ever.

So, that's why I put this book together. A programming book dedicated solely to the art of naming things.

---

***A quick note:** All of the examples I'll discuss are written in Microsoft's C# language and use object-oriented principles. But, those are just tangential details and shouldn't get in the way even if you've never programmed in C# before. Regardless of what language or style of programming you use, it's the thought-process that I hope you take away from each example.*

*Many of my examples also come from my work developing DoneDone, an issue tracker and customer support tool. In a book like this, It's critical to see real examples of imperfect code rather than fabricated ones.*
