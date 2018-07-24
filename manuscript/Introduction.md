# Introduction

## Why does naming matter?

If you've picked up this book already, I probably don't have to convince you why naming things well is a critical aspect of good software writing. But, you may need to convince your manager or other stakeholders who might not reap its immediate benefits. This section might not fully convince them either, but we can both say we tried our best.

Here are a few reasons to invest the time in naming things well.

### A timeless skill

Naming is an essential aspect of every programming language. It doesn't matter where you are in the stack or what kind of programming you do. If you name things well, you write better code. It's a skill set that will never become outdated over time (and if it does, it would mean the robots have officially taken everything over anyways).

### Speed and accuracy

The usual discussions around coding speed and accuracy revolve around quantitative or binary ideas. Have you written tests? How many have you written? What's your code coverage? Why aren't you using that pattern? Why are you using that pattern? We're hypnotized into believing that good code is code that meets a certain score.  We hardly ever talk about the actual words we use to describe the things we build.

But, good naming is another way to make your code less susceptible to human error. The more clear, accurate, and memorable the names of your structures are, the harder it is to mistake the intent of these structures.

### A safe entry-way into refactoring

There was a time in programming where you would have to find and replace any references to objects if they were renamed. It was a tedious and dangerous task. Nowadays, renaming can be done quickly and accurately in most languages (especially typesafe ones) -- most development environments offer some sort of renaming tool that will update all references to the updated construct automatically. 

This makes renaming variables, methods, classes, and other constructs is an easy and productive first step toward any larger code refactoring. Renaming confusing names before you dive into the actual refactoring can clarify exactly how to approach a refactoring, or confirm whether a larger update is even needed.

### Better communication with everyone in your organization

Another important aspect of naming well is to, as best you can, name concepts in code as they'd be discussed in practice. For instance, if I were building internal software to track the organizational aspects of law firm, I'd want the nomenclature of elements of my software system to be as closely aligned to the words law professionals use in their profession. PracticeAreas instead of Topics. PracticeGroups instead of Teams. Clients instead of Customers. Attorneys and Paralegals instead of, simply, Employees.

Business concepts can also dictate the kind of structures introduced into a codebase. If I were building a patient management system for a medical clinic, I might start with a concept like List<Patient> to describe a doctor's patient list. But, I've come to find that doctors don't call these things patient lists, they call them panels. Doctors often talk about how full their panels are or when new openings in their panel will open up.

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

```C#
public int Count 
{
	get 
	{
		return 1;
	}
}
```


