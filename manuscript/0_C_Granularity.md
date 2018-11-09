## Granularity

While removing extraneous words is important, writing better names isn't always about concision. We want each part of a name to clarify what something does. Sometimes, this requires a more descriptive verb or adjective. Here are a couple of examples.

I've written an extension method off of the `string` class which pulls out all email addresses for the given string using some magical regular expressions, and returns them in a `List`. It's a nifty method I can use to notify anyone mentioned in a user's comment. 

`GetEmails()` is my first attempt at a concise name. But, it doesn't quite make sense in context:

```C#
string comment = "Hey bill@microsoft.com, can I get a raise?";
List<string> emails_in_comment = comment.GetEmails();
```

The method feels out of place. It suggests that `comment` already has knowledge of some emails in tow. When I read the line `comment.GetEmails()` my first thought is, how exactly do you get emails from a piece of text? There's a better verb we can use.

The method is really _extracting_ emails, not merely _getting_them. And since I can't guarantee there will be emails in the string at-hand, I can further clarify that the method will "extract any emails" rather than simply "extract emails." Let's see the result:

 ```C#
string comment = "Hey bill@microsoft.com, can I get a raise?";
List<string> emails_in_comment = comment.ExtractAnyEmails();
```

I think it's a considerable improvement.

----------------

Now, let's look at a creation example. I've written a method that returns a unique API key for a given user. For this method, I could come up with a name like `CreateAPIKey()`. But, the word _create_ doesn't tell me a whole lot about _how_ the key is created. 

Is it pulled from some list of available keys in a database? Is it randomly generated? Is it somehow derived from other data a user owns?
 
In my case, the key is generated using a random GUID. So, instead, the name `GenerateAPIKey()`conveys the idea much more clearly. Instantly, I know the key creation involves some random generation rather than a replayable set of steps. It hints at the implementation detail just enough.

Why is this important? Suppose that later on, I may want to introduce a way to revoke and issue a new. I might have a complementary method whose implementation looks like this:

 ```C#
public string RegenerateAPIKey() 
{
  RevokeAPIKey();
  return GenerateAPIKey();
}
```

If, instead, I had settled for the more generic `Create`, a consistently named sibling method might look like this:
 
```C#
public string RecreateAPIKey() 
{
  RevokeAPIKey();
  return CreateAPIKey();
}
```

 `RecreateAPIKey()` could make me think, incorrectly, that an already-used API key will be reinstated. `RegenerateAPIKey()` makes me think, correctly, that the new API key won't be revived from the ashes.
 
 
### Alternative verb ideas

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
 
 ### Some sort of conclusion here
