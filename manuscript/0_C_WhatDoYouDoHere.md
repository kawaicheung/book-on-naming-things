## We don't need more managers

If you're a fan of the cult classic, _Office Space_, you'll certainly remember one of the more famous lines in the film, where the contemptuous consultant asks the agitated employee a simple, yet piercing, question.

> What would you say, you do here?

I channel this scene frequently when I'm finding my way through a new object library or revisiting object-oriented code I've worked on in the past. We spend so much time and energy shaping (and reshaping) classes into neat little packages; Yet we can't come up with a name that neatly explains it's reason for being.

This is why the term _object-oriented_ is itself a bit of a misnomer. Most classes we create don't have a direct physical representation. Despite what the textbooks say, most applications do not involve `Dog` and `Cat` objects subclassing an `Animal`.

Instead, we usually herd together groups of similar functionality into, what we'll call, an _object_. And because this object doesn't have an obvious physical translation, we lean on names that do not lie, but also don't tell us much of anything. These are the classes employing names like `UserManager`, `StringHelper`, and `RequestHandler`. Like our corporate consultant, we encounter these names and ask ourselves what exactly it is they do here.

Fortunately, the English language is full of more expressive nouns. Sometimes, even when an object doesn't have an obvious physical representation, a bit of clever thinking can bridge the gap.

In DoneDone, I've built a messaging service whose sole responsibility is to decide whom should and shouldn't receive an email based on a newly updated or created issue. The answer depends, first, on who's involved on the issue. For example, I create an issue for Aaron and also add Jason, Mustafa, and Penelope on the issue. They all should be notified.

However, the other part of this determination relies on what each person has opted into. You can decide to be notified on every update (even on issues you're not directly added to), on every created issue, just the issues you're added to, or on nothing. It's the messaging service's job to figure out who should receive an email notification and package up a collection of objects to ship to an email service.



* Often a smell to break things out.

