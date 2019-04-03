# Preface

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

