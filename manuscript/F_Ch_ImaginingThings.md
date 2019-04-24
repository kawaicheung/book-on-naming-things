# Imagining Things

If you're a fan of the cult classic _Office Space_, you'll certainly remember the line in the film when the contemptuous consultant asks the agitated employee a simple--yet piercing--question.

> What would you say...ya do here?

I often channel this scene when I'm swimming my way through a large API or revisiting some code I've worked on in the past. We spend so much time and energy shaping (and reshaping) classes into neat, digestible packages; Yet, finding a name that neatly explains it's reason for being is sometimes near impossible.

This is why the term _object-oriented_ is itself a bit of a misnomer. Most classes we create don't have direct physical representations. Despite what the textbooks say, most applications do not revolve around a `Dog` and `Cat` subclassing an `Animal`.

Instead, we usually herd together groups of similar functionality into, what we'll call, an _object_. And because this object doesn't have an obvious physical translation, we lean on generic names that don't tell us much of anything. These are the classes employing names like `UserManager`, `StringHelper`, and `RequestHandler`. Like our corporate consultant, we encounter these names and ask ourselves what exactly it is they do here.

Fortunately, the English language is full of more expressive nouns. Sometimes, even when an object doesn't have an obvious physical representation, a bit of imagination can bridge the gap. Here's an example.

===

[Migrator/importer/copier example -- not as much of a visual 'leap' as officer, so a good starter piece]  -> Also, maybe an anecdote about Mike saying why do you care since its just you? YOu should ESPECIALLY care. Since you OWN the codebase, its about taking OWNERSHIP by caring about what your world will look like.

===

In DoneDone, I built a messaging service whose sole responsibility is to decide whom should receive an email based on a created or newly updated issue. The answer depends, first, on who's involved on the issue. For example, I create an issue for Aaron and also add Lindsay, Mustafa, and Penelope on the issue. They all should be notified.

The other part of this determination depends on what each person has opted into. As a DoneDone user, you can decide to be notified on every update (even on issues you're not directly added to), on every created issue, or just the issues you're added to. You can also decide to not be emailed on anything, ever. It's the messaging service's job to figure out who should receive an email notification and package up a collection of email objects to ship to the email service.

I ended up extracting out a class to handle this extra determination. The class holds various methods helpful to the messaging service. 

For instance, the `AllowEmail()` method accepts a user's ID and checks whether the user has opted out of all emails. This method is used while looping through all the folks on a newly updated issue to sift out the ones that don't want to be bothered at all. Another property of this class is a `UsersThatAlwaysWantEmail` list. The messaging service grabs this list to ship out emails to anyone else not directly on the issue.

So, what should we name an object like this?

A name like `EmailNotificationManager` is tempting. But, looking at this name in isolation, I might assume this class also manages the storing of a user's email preferences. In addition, it doesn't convey the idea that the object helps determine who should get emailed. Scratch that. How about `EmailNotificationHelper`? Still too ambiguous. What, exactly, is it trying to help with?

In scenarios like these, I imagine what the best physical parallel would be. Close your eyes and visualize the messaging service as a _real thing_. What would this object look like?

Here's what I imagined.

The messaging service is a dimly lit shipping center. Once an issue is created or updated, the messaging team receives word from the intercom system. One employee looks up the issue to see who's been added to it. He notes these folks on a clipboard and runs down to an assembly line of workers ready to assemble messages and haul them to the back of a truck for delivery. 

But, before he gives the assembly line his list, another worker stops him dead in his tracks, with the palm of her hand. She asks to see the clipboard. One by one, she makes sure none of the folks on the list have opted-out of these message deliveries. She also adds anyone else that should be on this list but isn't. Then, she hands the clipboard back to the worker and, with a nod, tells him to move along.

I imagine this person with a stern face dressed in a police uniform. And suddenly, the name `EmailNotificationOfficer` comes to mind. `Officer` conveys a kind of policing of emails -- exactly what this object's main responsibility is. It's certainly more memorable and visual than either a manager or helper. 

At first, the name feels strange. "Officer" is not a very programmatic term. But, that's _just the point_.

The more I've let the name sit, the more correct it feels. If you asked me a month later what the class `EmailNotificationOfficer` does in the DoneDone codebase, I can tell you in great detail. I can also tell you I wouldn't have had the same luck had I stuck with a name like `EmailNotificationManager`.

---
