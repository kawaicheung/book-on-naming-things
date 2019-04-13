# When CRUD is crud

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
