# English++

When we say _readable_ code, we mean code that has a certain familiarity to us. Famliarity usually comes about from our expectations of normal written language.

I grew up with English as my first language. Naturally, code that reads more like English is more readable to me. Of course, most programming language constructs are written using English words to begin with (`for`, `if`, `class`, `string`, `interface`, `public`, and so forth). But, words are just one part of a language.

Just as important to the fluidity of code is how it mimics the rules of phrases and sentence -- how adjectives, nouns, verbs, and all the other parts of speech play off of each other.

The first catch is that code also comes with its own unique set of rules. For instance, English doesn't use parentheses, curly brackets, or periods the way we do in code. 

`document.Write(\"Hello World\");` certainly reads more fluidly than `d.w(\"Hello World\");` but not as fluidly as "Write \"Hello World\" to the document." As programmers, however, we've assimilated to code's unique rules -- to the point where we might actually intuit the meaning of `document.Write(\"Hello World\");` even faster than the sentence "Write \"Hello World\" to the document."

So, when we write readable code, we have to take readability with a grain of salt. Optimal readability happens when we can balance these two worlds.

The second catch is that ordering rules in English are bizarre. It’s just not odd to us because we’re so accustomed to it.

Consider dates. In most areas of the world, dates are written in the form year-month-day or day-month-year. The units of measure logically narrow or broaden as the date is read across.

Americans write dates as month-day-year. The largest unit of measure is last, the second largest is first, and the smallest is in the middle. Why? Why not!

Next, consider how we name people in the Western world. _John Doe_ is written with the most specific word (John) first, and the least specific word (Doe, shared by other members of his family) second. 

In Chinese, the family name is spoken or written first, followed by a name that is shared by siblings of the same gender and a name that uniquely identifies the individual. As such, names in some cultures are written from a broad to narrow scope. My Chinese name is spoken as _Cheung Ka Wai_, from the most broad identifier to the most specific. In English, it is spoken as _Ka Wai Cheung_, from second-least to least to most broad.

It's no surprise then that when we native English speakers name constructs, we tend to choose a pattern that follows English rules rather than logical ordering rules. For example, in a typical MVC framework, a controller is named something like `AccountController.cs`. This forces us to think about the class as relating to an `Account` first, then as a type of `Controller` second.

Yet, we typically organize all controllers, models, and views within a single `Controllers`, `Models`, and `Views` folder rather than within folders named by business entity.

* `Controllers`
  * `AccountController.cs`
  * `ProjectController.cs`
  * `UserController.cs`
* `Models`
  * `Account`
    * `Index.cs`
    * `Create.cs`
* `Views`
  * `Account`
    * `Index.cshtml`
    * `Create.cshtml`



Yellow-Callout
Blue-Callout

Callout-Yellow
Callout-Blue

What do you care about? English says “yellow callout” but really you care about the callout first, and then the style underneath second.


Scannability!

00 END WITH THE trick is deciding what aspects of organization we want to lean on english for vs. programming. it's the art of it all.

