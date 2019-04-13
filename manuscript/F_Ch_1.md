# When CRUD is crud

**Being Blunt**

[Delete vs Incinerate vs Remove - should talk about Remove since its relevant -- RemoveTagsFromProject]

Many of us have trouble communicating directly. We don't want to offend. We don't want to come across as having an unwavering opinion. We try to dampen our real intentions behind a fog of words. In code, this can be  dangerous.

Consider what it means when we see a method like `Delete()`. It's a loaded term. On the surface, we know it means something like "get rid of this thing and never let me see it again." And, that's generally good enough for users. 

But, programmatically, this could mean a number of things. We might be merely placing a flag on a record in a database or removing a relationship between items to prevent access. It's also possible we're wiping the record for good.

When we use the word `Delete` as part of a method name, we usually don't care about the implementation --- we only care about the meaning in the scope of the class we're working in. In a method like `Delete(int project_id)` on a `ProjectsController` class, we only care that executing this method means a user can no longer access it. Whether aspects of the project still remain intact behind-the-scenes is irrelevant.

However, it's smart to make an exception when a method does perform a true, irreversible delete on data that's not easily recreatable. Because the word `Delete` is such a ubiqutious term in programming, it might not convey that meaning appropriately.

Choosing such a word is difficult. It should have even more cautionary weight than `Delete`.

David Heinemeier Hansson (Basecamp CTO and Ruby on Rails creator) reserves the term `Incinerate` in Basecamp's codebase to distinguish the "sweaty-palms" kind of delete from the softer, "just put a flag on it" type. "When we talk about incineration within the app, it means this one, specific thing."[^dhh1] 

_Destroy_ or _eradicate_ might also work. Both have more gravitas than _delete_.

Contrast this to a word like "Remove" which feels softer -- as if you can bring it back if you had to. Use "Remove" for things that could easily be brought back (or recreated from scratch if need be). I would be nervous to name a method "RemoveUser" that deletes all user data but OK with "RemoveUser" if it untied a user from an account which can easily be re-attached.

Be blunt with your words. If something permanent is going to happen in your method, make sure your name makes that obvious. Being clear about these actions adds another layer of defense beyond testing. You can squash any potential misfortunes much earlier in the QA process with strong words. 

[^dhh1]: [On Writing Software Well #6: Actually deleting data, not just pretending to](https://www.youtube.com/watch?v=AoxoPfilKqE)

**Dusting off the Thesaurus**

There are so many wonderfully descriptive, particular, just-right words in the English language to describe nearly every kind of scenario we face in programming. It's a shame we usually limit ourselves to the most generic kinds. The advent of the CRUD acronym (Create, Read, Update, and Delete) might be partially to blame. After all, even the acronym itself suggests there's something unsavory in these words choices.

When I first started learning to program, I didn't think that a good thesaurus would be one of my most valued programming possessions. But it is. This chapter is about finding those word-gems in the rough. The ones that you look back with fresh eyes the next morning with quiet satisfaction. 

We've already covered a few better replacements for many overly-used terms. We aren't just creating an API key, we're generating one. We don't get emails from a string -- we extract them. We aren't simply deleting an account, we're incinerating it. Those vivid words aren't just memorable, they describe a lot about what's going on behind-the-scenes without us having to look under the hood.



|Verb   |Replacement     |Use case                                  |Example                               |
|-------|----------------|------------------------------------------|--------------------------------------|
|Create |Generate   	 |Creating something with randomness.		|GenerateAPIKey()				   	   |
|Create |Initialize   	 |											|								   	   |
|Get    |Extract, Expose |Exposing something that already exists. 	|string.ExtractAnyEmails()	   		   |
|Get    |Convert         |Getting a manipulation of an object.		|date.ConvertToShortDate()			   |
|Get    |Filter          |Passing a list of items and returning less. |filterProjects(List<projects>, UserID)			   |
|Get    |Decrypt         |Decrypting a token and returning a value/object |decryptRegistrationToken(string token) 
|Update |Overwrite       | 										    |									   |
|Update |Replace 		 |								            |									   |
|Update |Merge	 		 |								            |									   |
|Update |Sync	 		 |(With 3rd party)						    |									   |
|Delete |Remove	 		 |										    |									   |
|Delete |Incinerate	 	 |For hard or permanent deletes.	    	|project.Incinerate()				   |
 
ALSO: POP (e.g. get and delete), PUSH...
