# Why naming matters

We all have opinions on how to write better code. The debates go on interminably. 

Usually, these opinions revolve around meaty concepts like frameworks, patterns, and architectures. But, these concepts usually come with a price: They're large in scope. 

You can't just paste a framework into an existing codebase -- it's _intensive_ work. It's also hard to unwind that work once you've started. You might spend days implementing something only to realize the result wasn't nearly worth the effort.

On the other hand, changing the name of something is fast. It gives you immediate feedback. It doesn't require additional infrastructure. If a name doesn't feel right, you can change it again. You can continually improve a name without being stuck with your original decision. 

The returns are equally enticing.

Renaming something well can clarify what was previously ambiguous about it. A descriptive name might expose a problem you didn't see before. The right name makes it easier for someone else to figure out what your intentions were when you originally wrote the code. That someone else might even be you a few months from now. 

Scrutinizing names is the first thing I do when I want to improve a codebase. This book is a small collection of examples on how I approach naming (and renaming) things.

---

***A quick note:** All of the examples I'll discuss are written in Microsoft's C# language and use object-oriented principles. But, those are just tangential details and shouldn't get in the way even if you've never programmed in C# before. Regardless of what language or style of programming you use, it's the thought-process that I hope you take away from each example.*

*Many of my examples also come from my work developing DoneDone, an issue tracker and customer support tool. In a book like this, It's critical to see real examples of imperfect code rather than fabricated ones.*
