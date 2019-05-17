# Naming Things

Here's why I wrote this book.

So I finally got around to working on this data migration feature--something I had put off for months. But, finally, it crept to the top of my to-do list. That, and some consistent nudging from my business partner, Mike. OK--it was mainly the consistent nudging. Sometimes you build things so your business partner will stop bugging you.

For the past twelve months, I had been developing a new version of DoneDone, an issue tracking and customer support tool that I originally created almost ten years ago. It's kind of a rarity in this industry, but I've been fortunate enough to work on this one product for a very long time. Though the codebase has had a handful of contributors in its life, I've been there since day one. I know how this code works intimately.

This new version wasn't a rewrite; I branched off the existing version as the starting point for what would be a year's worth of refactorings, tweaks, removals, and new feature additions. By the end of it, it essentially _was_ a complete rewrite. More on this later.

Anyways, the point of this data migration feature was to move data from our original version (which we now called "Classic") to the new version ("DoneDone 2"). A user could login with their old credentials, pick the projects they wanted to move, and then queue the projects for migration. It was a great way to get our current users to adopt the new version quicker. 

I had the feature nearly polished in about six days--the backend components, the UI, the out-of-band service that handled the data migration, everything.

But, during the process of building this thing, I agonized over the name. We had been calling it a _migration tool_ forever -- starting the months prior when Mike began his initial subtle nudging that this was an important thing to have.

So, at first, it made sense to use the word `migrate` (and all of its derivatives) in my code. The migration service was named the `ClassicMigrator`. There were methods named `MigrateProjects()`, `QueueMigrationRequests()`, and so forth. The URL routes and MVC components all used the word _migrate_ somewhere.

But, when I took a fresh look at it, something felt wrong with this name.

Migrate means to move something from one place to another, like a flock of birds migrating south for the winter. They're not copying themselves over to the south--they're leaving the north.

However, that's not exactly what the migration feature did. We were leaving the original data in Classic untouched. This way, Classic users would have a chance to play around with the new system using their existing data but they wouldn't be required to leave Classic if they didn't like the change. They could just go back to using the old system.

Migrate was a very misleading word. Using that word could confuse our customers and any other future developers of the app down the road.

So, next, I thought of replacing it with the word _copy_ instead. But, this also had a few drawbacks.

First, the word copy makes it seem like a literal duplication -- a CTRL + C -- this wasn't that. There were all sorts of differences between Classic and DoneDone 2, so some aspects of a Classic issue didn't translate identically in DoneDone 2. There were some assumptions I had to make in code to translate things as closely as we could.

I didn't like Migrate. I didn't like Copy. I decided to get some input from Mike. It was his insistence on this feature after all.

At first, Mike asked, "Why is this name so important? It's just you looking at it." I ranted a bit about how, because it was me and only me -- code that I alone would be maintaining for quite some time -- that it was even more important that we found the perfect word. Then, he suggested we call this an import.

Now, import is a very interesting word. I feel like it has a very specific meaning in technical speak -- there's some engine involved in importing data, usually with metal gear icons spinning slowly. There's a heftiness to the word. Even when you import things in the real world, it has the same feeling -- cargo ships coming to port across the ocean and offloading goods.

The other rather interesting thing with the word is, unlike migrating, it doesn't strike me as leaving one place and going to another. Unlike the migrating birds, importing data doesn't make me feel like I'm losing the old data. I don't quite know why -- after all, in the real world, if I import some goods, the goods are no longer where they started.

But, for some reason, in the technical sense, import doesn't give me that same feeling. Similarly, exporting doesn't either. When I export data from a system to a file, I don't expect the data to leave the system - I just expect a copy of that data onto the file.

I think we use the words import and export incorrectly in the technical world...but, it's become so ingrained in the technical vocabulary that it works. It has a different meaning than the literal idea of moving things to and fro. Import is still a "copy" but it has the right connotations for the task I was doing.

So, import was the verb I used in code. The ClassicImporter, ImportProjects(), QueueImportRequests(). Sometimes, the right word is more about how it feels rather than whether it's literally the correct word.


