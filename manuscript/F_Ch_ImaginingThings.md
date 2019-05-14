# Imagining Things

If you're a fan of the cult classic _Office Space_, you'll certainly remember the line in the film when the contemptuous consultant asks the agitated employee a simple--yet piercing--question.

> What would you say...ya do here?

I often channel this scene when I'm swimming my way through a large API or revisiting some code I've worked on in the past. We spend so much time and energy shaping (and reshaping) classes into neat, digestible packages; Yet, finding a name that neatly explains it's reason for being is sometimes near impossible.

This is why the term _object-oriented_ is itself a bit of a misnomer. Most classes we create don't have direct physical representations. Despite what the textbooks say, most applications do not revolve around a `Dog` and `Cat` subclassing an `Animal`.

Instead, we usually herd together groups of similar functionality into, what we'll call, an _object_. And because this object doesn't have an obvious physical translation, we lean on generic names that don't tell us much of anything. These are the classes employing names like `UserManager`, `StringHelper`, and `RequestHandler`. Like our corporate consultant, we encounter these names and ask ourselves what exactly it is they do here.

Fortunately, the English language is full of more expressive nouns. Sometimes, even when an object doesn't have an obvious physical representation, a bit of imagination can bridge the gap. Here's an example.

===

One of the key features I needed to build into DoneDone 2 was a migration tool from DoneDone Classic. This would allow any of the users of our old product (some of them have been around for nearly a decade!) to import their users, projects, and issues into DoneDone 2. To be honest, it was a task I was dreading -- there were so many differences between how DoneDone 2 and DoneDone Classic stored data. I knew it would be a really daunting task.

At some point a few weeks before launch, the "Migration tool" crept up to the top of my to-do list. That, and some consistent nudging from Mike. OK, mainly it was the consistent nudging. Sometimes you build things to build things. Other times, you build things so people will stop nudging you.

Actually building and testing this thing turned out to be not as bad as I thought. I think I've created a quasi-defense mechanism as a programmer. I build up this whole story in my head about how overwhelming some task will be weeks ahead of time.

I think I inherently begin thinking about the task in random little spurts during the weeks leading up to it so that by the time I actually begin building it, all the things I thought were daunting I had already thought through. It's akin to giving myself a lot of lead time to study for a test so that the actual test is almost a formality.

This was one of those times. In about six days, I had built the entire thing top-to-bottom. A user could log into Classic (through DoneDone 2), pick the projects they wanted to import, and then queue them. I had written a separate out-of-band process to pick up queued projects on a small time interval and do the actual migration.

Now, during the process of building this thing, I agonized a bit over the name. We had been calling it a "migrator" forever -- starting the months prior when Mike began his initial subtle nudging that this was an important thing to have.

So, at first, it made sense to call this a "migrator" in my code. ClassicMigrator. MigrateProjects(). QueueMigrationRequests(). and so forth. But, something felt wrong about this name.

Migration, in the programmatic sense, means moving things from one place to another -- like a flock of birds migrating south for the winter. They're not copying themselves over to the south -- they're leaving the north.

However, our migration tool wasn't a true "migrator" in that sense. We were leaving the original data in Classic untouched. This way, Classic users would have a chance to play around with the new system against their existing data but they wouldn't be required to leave Classic if they didn't like the change.

Migrating was a very misleading word. I wouldn't want our Classic customers being timid about trying out the new system for fear they couldn't go back.

"Copy" felt like the most straightforward way of describing this tool. We were copying data from Classic to DoneDone 2. But, this also had a few drawbacks.

First, the word "copy" makes it seem like a literal duplication -- a CTRL + C -- this wasn't that. There were all sorts of differences between Classic and DoneDone 2, so some aspects of a Classic issue didn't translate identically in DoneDone 2. There were some assumptions I had to make in code to translate things as closely as we could.

I didn't like Migrate. I didn't like Copy. Mike suggested we call this an import.

Now, import is an interesting word. I feel like it has a very specific meaning in technical speak -- there's some engine involved in importing data, usually with metal gear icons spinning slowly. There's a heftiness to the word. Even when you import things in the real world, it has the same feeling -- cargo ships coming to port across the ocean and offloading goods.

The other rather interesting thing with the word is, unlike migrating, it doesn't strike me as leaving one place and going to another. Unlike the migrating birds, importing data doesn't make me feel like I'm losing the old data. I don't quite know why -- after all, in the real world, if I import some goods, the goods are no longer where they started.

But, for some reason, in the technical sense, import doesn't give me that same feeling. Similarly, exporting doesn't either. When I export data from a system to a file, I don't expect the data to leave the system - I just expect a copy of that data onto the file.

I think we use the words import and export incorrectly in the technical world...but, it's become so ingrained in the technical vocabulary that it works. It has a different meaning than the literal idea of moving things to and fro. Import is still a "copy" but it has the right connotations for the task I was doing.

So, import was the verb I used in code. The ClassicImporter, ImportProjects(), QueueImportRequests(). Sometimes, the right word is more about how it feels rather than whether it's literally the correct word.

[maybe an anecdote about Mike saying why do you care since its just you? YOu should ESPECIALLY care. Since you OWN the codebase, its about taking OWNERSHIP by caring about what your world will look like]

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
