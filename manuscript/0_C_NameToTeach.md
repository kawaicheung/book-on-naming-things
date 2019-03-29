## INCOMPLETE: Name to Teach

I have a very simple thing to say about how code is taught. The more advanced you are, the more likely you teach the craft poorly. As you become more advanced in a field, more basic concepts in that field become so familiar that it becomes harder to recall what it was like before you understood those concepts. 

Assuming you actually know how to ride a bike--and have known how to do so for years--imagine the feeling you had of getting on a bike for the very first time. You tried to pedal and keep the handlebars straight, but you _still_ fell down. I can remember this struggle when I was five years old. 

If I got on a bike today, I would have to fight my instincts to try to pedal and fall down. It would actually be _hard_ for me to fall off my bike now. The old saying "it's like riding a bike" isn't just about something being intuitive, it's about it being really hard to recall the feeling of that thing before it became intuitive.

Having worked with object-oriented programming languages for over twenty years, to me, the concept of a class constructor is obvious--it's a method which instantiate an instance of the object a class defines. Like riding a bike.

But, when I was first taught about class construction, I was at a complete and utter loss. The concept was one of those fancy italian racing bikes with those clip-on pedals. I was a mermaid. 

Looking back, it was unclear to me because I couldn't grasp the idea that a class definition and an instance of that class were two entirely different things. I also couldn't visualize what "instantiation" was. Is this a term ever used outside of programming?

A significant--no, perhaps the entire--part of the problem was how the concept was introduced. I recall seeing one of these example lines of code in my introductory class. It still brings shivers to me today.

```C#
Object myObject = new Object();
myObject.DoSomething();
```

A line of code like this has no relatable concept outside of the programming world to latch onto. This example is a problem with naming. The word `Object` appears three times in one line. It creates that dizzying effect I mentioned in _Code Shape_. A knowledgeable programmer knows that they each stand for something different.

* The first `Object` refers to the type of the instance we're instantiating.
* The second `Object` is part of the name of this instance `myObject`.
* The third `Object` is invoking the constructor.
* The fourth `Object` is part of the now-instantiated instance `myObject`, which is now performing the very detailed action of `DoSomething()`.

But, for a programming newbie, this made as much sense as this equally sensible line.

```C#
Xy8lksp0 myXy8lksp0 = new Xy8lksp0();
myXy8lksp0.DoSomething();
```

Some code writers will "improve" upon this example by introducing meaningless variable names (`foo`, `bar`, `baz`, and so forth).

```C#
Object foo = new Object();
foo.DoSomething();
```

This helps rid some of the dizzying effects. At least the instance name is more readily discernable. But, the newbie is still left wondering what this all means.


```C#
Object foo = new Object();
```

At this level, we often teach concepts too abstractly -- as if diving directly into concrete examples that demonstrate a concept would somehow mask the underlying purpose of the concept.

[Culprit #1 -- Foo / Bar]

[Culprit #2 -- "My" or "1/2"]

http://www.deitel.com/books/cpphtp5/cpphtp5_03_sample.pdf

[Culprit #3 -- Real-world examples vs. realistic examples.Dogs/Cats or car parts.] 


