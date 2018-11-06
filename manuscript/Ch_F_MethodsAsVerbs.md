
## Delete 

Now let's consider `Delete`. It's, indeed, a loaded term. On the surface, we know it means something like “get rid of this thing and never let me see it again.” And, that's generally good enough for users. But, programmatically, this could mean a number of things. I might be placing a flag on a record in a database, removing a relationship between items to prevent access, or, actually hard deleting the record for good.

When I use the word `Delete` as part of a method name, I _usually_ don't care about the implementation --- I only care about the meaning in the scope of the class I'm working in. In a method like `Delete(int project_id)` on a `ProjectsController` class, I only care that executing this method means a user can no longer access it. Whether aspects of the project still remain intact behind-the-scenes is irrelevant.

However, I think it's prudent to make an exception when a method does perform a _true, irreversible hard-delete on data that's not easily recreatable_ --- when I'm not softly removing, dropping, clearing, hiding, or deactivating, but in fact, I'm permanently destroying something, for good. [Why? We'll use it with care. Another level of defense.]

Choosing such a word is difficult. It should have even more cautionary weight than _Delete_. 

David Heinemeier Hansson (Basecamp CTO and Ruby on Rails creator) reserves the term _Incinerate_ in Basecamp's codebase to distinguish the "sweaty-palms" kind of delete from the softer, "just put a flag on it" type.  By doing so, it's become part of the technical lexicon for the Basecamp development team. “When we talk about incineration within the app, it means this one, specific thing.”[^dhh1] _Destroy_ or _Eradicate_ could also work. 


[Contrast to a word like "Remove" which feels softer -- as if you can bring it back if you had to. Use "Remove" for things that could easily be brought back (or recreated from scratch if need be). I would be nervous to name a method "RemoveUser" that deletes all user data but OK with "RemoveUser" if it untied a user from an account which can easily be re-attached (assuming other things don't blow away with it)]

[^dhh1]: [On Writing Software Well #6: Actually deleting data, not just pretending to](https://www.youtube.com/watch?v=AoxoPfilKqE)

## Alternative verb ideas

In the introduction, I mentioned that this book is not a reference manual. But, I thought it would be useful to list a bunch of synonyms I frequently use as replacements to conventional action terms. Hopefully, this list inspires you to think of more informative method names.

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

