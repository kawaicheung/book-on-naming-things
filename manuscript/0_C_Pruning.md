## Pruning

In school, writing assignments always had some sort of minimum length requirement. Five paragraphs. 2000 words. Ten pages. 

I understood why there was a requirement -- it gave us a general guideline for how much we should write. But, that requirement became the thing all of us gravitated toward. “Can I get to ten pages if I increase the font size by one pixel?” was more important than “Does that additional paragraph really strengthen my argument?”.

I wonder what would happen if teachers gave these limits in reverse -- no more than five paragraphs, 2000 words, or ten pages. Placing a maximum constraint would focus the writer on what they're writing about, all the while mitigating unnecessarily long sentences and double spacing.

Editing, in school, usually meant bloating your words. It wasn’t until I left school that I discovered that the true value of editing is reducing words. 

When I edit, I almost always end up with something _shorter_ than what I started with. I don't aim for a word count; I aim to make each word count. Prune what's not essential.

Luckily, we don’t stigmatize concise code. Code brevity has always been seen as a good thing in our industry. No sane programmer would tinker with a small bit of logic just to get it to be more wordy.

If you can, spend an hour just editing down names. Nothing more. Examine every method, class, variable, and property you maintain and figure out where you can take away words without losing meaning.

A property like `InitialCreationDate` doesn’t say anything more useful than `CreationDate`. It’s more verbose and, if anything, makes me question how there would be another date that something could’ve been created after the first time. (OK, if we’re going there, then we ought to add a new property like an `InitialResurrectionDate`).

A method in a `Customer` class like `GetAllSubscriptions()` is appropriately named if there are other related methods like `GetArchivedSubscriptions()` or `GetExpiredSubscriptions()`.  If not, `GetSubscriptions()` is sufficient. It says all it needs to say without falsely hinting that there are other related methods to consider.

When we conjure up names, adjectives like _all_ or _initial_ seem helpful at first. But, unless they provide additional meaning that the other words don’t, remove them. Each word should be there for a reason. The less words we need to describe something, the more value those descriptors will add. 

Treat words like currency.
