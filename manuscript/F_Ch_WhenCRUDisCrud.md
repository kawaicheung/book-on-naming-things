# Describing Methods

There's nothing like writing a good piece of code. It's code that, first of all, works. Not only does it work, but it _does_ its work efficiently. Not only is it efficient, but it reads beautifully. Producing code that helps the end user, the operating system, and its future authors is the ultimate accomplishment of a programmer.

But, there are two problems with this last bit. First, it's hard to measure what readable code _looks_ like. Second, if you use an object-oriented programming language, it's even harder to write it. The truth is this: object-oriented languages are fantastic for building systems--for dispersing work to various layers of a system to achieve goals like encapsulation, extensibility, and reuse. But, they aren't made for readability.

You can't sit down and read this kind of code linearly like you would a newspaper article. Instead, you're constantly directed to other methods and objects, bouncing back and forth throughout the pages of a codebase, like a _Choose Your Own Adventure_ book. It was only fun when you were a kid.

The best way to make this kind of code readable is to spare the reader from drilling down into the layers. A great place to start is by making method names exquisitely _clear_.



---

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
 
------




####


**Ambiguity**

One of the most common reasons a method name can be improved is because it's too ambiguous. Ambiguous names are often a sign that a method is doing too much. That's what made naming it difficult in the first place. 

You might have the urge to refactor such a method into smaller pieces. It will certainly make naming the smaller methods easier.

However, refactoring can be a large and unpredictable effort. You don't always need to make such a deep cut right away. The more pragmatic fix could be in a more precise name. You might still decide to refactor later, but finding that sharper name might be enough *right now*.

Here's an example of a method I stumbled across in my own code recently. 

I'm staring at a method name inside a `Project` class called `UpdateUsers()`.

```C#
public void UpdateUsers(List<Users> users) { … }
```

At first, this name seems completely reasonable. But, as you get into the details, suddenly The method should take a list of users and grant them access to the given project. But, what does the method do to users who currently have access but aren't in the passed-in list? Will the method remove them, leaving access to only the ones passed in? Or, will it keep them around, adding additional users from the list? A method named this way doesn't tell me for sure. I have to look inside and see what's going on. 

After I dig into the code, I see that existing users not in the list are removed, not spared. I start with a comment on the method to clarify what's happening.

```C#
// Overwrites project's assigned users with the passed-in users list
public void UpdateUsers(List<Users> users) { … }
```

That's better. But, comments tend to outdate themselves quickly. If there's a way I can impart the gist of the comment into the name, I should. The word _overwrite_ is the key to the comment. The method isn't just updating users, it could be removing users as well. _Overwrite_ describes that idea much more clearly. Let's see how it looks.

```C#
public void OverwriteUsers(List<Users> users) { … }
```

Another step better. Not only does this feel less ambiguous, _overwrite_ makes me feel like I ought to use this method with caution.

Here's another way to judge how the name feels. How does it look like when I pass an empty list or `null` into the method?

```C#
myProject.UpdateUsers(null);
```

In the original version, it feels like passing `null` wouldn't do anything to the project's users. It's as if I'm passing in nothing for the project to update. But, if I were to pass the same argument to...

```C#
myProject.OverwriteUsers(null);
```

...I'd expect it to do some serious damage. That's just the feeling I want to convey.

I use `Overwrite` instead of, say, `Override` because I like to avoid names that have other reserved meanings. In this case, `override` is a modifier used in many languages to extend or modify the implementation of an inherited method. It's best to stay away from using names that have a prescribed meaning in the language already.

`ReplaceUsers()` could work just as well here. There are days I might favor it, but it feels a touch subtler to me than `Overwrite`. Given the choice, I lean toward stronger words for clarity.

When I can't find that perfect word right away, I'll look up synonyms to the words that feel close. A quick Google search for alternatives to _Replace_ and _Overwrite_ gives me options like _Change_, _Alter_, _Displace_, _Reimburse_, _Substitute_, _Swap_, _Upgrade_, _Improve_, or _Modernize._  None of these feels nearly as good. Some feel entirely strange. 

`OverwriteUsers()` still doesn't clarify something about the implementation, though. 

When I originally looked into the method details, I noticed that users are linked to projects via `Membership` records. Memberships hold additional bits of information relevant to a user's project access--the date access was granted, their particular role on the project, and notification settings tied to that membership. 

Knowing this, the name `OverwriteMemberships()` is even more accurate.

But, what does `OverwriteMemberships()` do for users who both have a membership currently and are in the passed-in list? As it stands, it sounds like their membership will be removed and re-created, thereby erasing any current information tied to their membership. 

Another peek inside the method reveals that--in fact--the code is delicate with existing memberships. All existing memberships who will continue to have access will stay intact rather than be removed and recreated. With that knowledge, there might be a better name to express this additional nuance, like `UpsertMemberships()` or `SyncMemberships()`. 

I don't have the perfect answer here, but this exercise tells us a couple of things.

First, it's hard to pack every subtlety of a method's implementation into its name. Instead, I do my best to get the most important aspects of the implementation into the name. A comment helps with context around any extra nuance.

```C#
// Will keep current memberships in users list intact
public void OverwriteMemberships(List<Users> users) { … }
```

Second, if I can't find a method name that covers enough of the essential parts of what it does, this could be a sign that the method is doing too much. Do I need to break up the method into smaller ones so that each one's intent can be more clearly wrapped in a name?

In this case, I'd prefer to leave the method intact. It's not worth trading away the succinctness of a single function that handles updating a project's memberships for a series of smaller, separated functions just because they might be more perfectly named. I'll live with the comment and move on to more important work.

This example _seems_ like a whole lot of thinking for something inconsequential. But, once you get accustomed to critiquing names this way, the exercise becomes a normal, fluid, and somewhat addictive part of the code writing process. You get faster and better at it. A codebase that's full of expressive and thoughtful names will make your development more enjoyable. 

**Precision**

While removing extraneous words is important, writing better names isn't always about concision. We want each part of a name to clarify what something does. Sometimes, this requires more precise words. The usual standbys of programmatic verbs like `Get` or `Create` often leave something on the table. Here are a few examples.

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
 
------

Here's a final example. I have a couple of overloaded methods in a `PeopleService` class named `GetPeople()`. Both return a list of `Person` objects based on different inputs.
 ```C#
public List<Person> GetPeople(int[] ids) { … }
```
```C#
public List<Person> GetPeople(string first_name_like, string last_name_like) { … }
```
It's nice to have these methods named identically. It reduces the overall number of method names in a class which, in turn, tightens the shape of the class. 

The second implementation of `GetPeople()` does a match against first and last names. We're also not guaranteed any result back. Something like `SearchPeople()` conveys the meaning more appropriately.

In addition, there likely won't be any use cases for a method like this _other_ than as part of a search feature. By using the word _Search_ here (and assuming we've named other related components along the stack similarly), the workflow from the interface to the backend will read more cohesively. That's well worth adding an additional method name to the service.

A> `/app/search` &rarr; `PeopleController.Search()` &rarr; `PeopleService.GetPeople()`
A> `/app/search` &rarr; `PeopleController.Search()` &rarr; `PeopleService.SearchPeople()`
 
### Some sort of conclusion here


**The Appropriate Level of Detail**

In each of the examples in the previous chapter, my revised names attempt to describe a method's implementation more clearly. Some would argue that I'm committing a cardinal programming sin -- one of the fruits of encapsulating logic in methods is to _hide_ the details of the implementation. How dare I try to expose them more!

But, one of the tricks of good method naming is to expose _as much_ detail as is useful. The more knowledge we ascertain from the name of a method, the faster we get at choosing the right methods to use or deciding to add additional ones to fulfill the task-at-hand.

For instance, in the API key generation example above, I felt that `GenerateAPIKey()` was a better choice than `CreateAPIKey()` because it describes _how_ an API key is created with a little more detail that might be useful to the implementor.  

I certainly could've carried the name even further. However, would a name like `GenerateAPIKeyUsingRandomizedGuid()` be better? If there were other API generation mechanisms at play that required alternative methods, and the mechanism mattered outside of the method itself, then absolutely. In my case, there wasn't.

A name like `GenerateAPIKeyUsingRandomizedGuid()` also presents a couple more issues:

1. It's long, and long names can reak havoc on the overall shape of code. By the way, we'll talk about long names and [code shape](#ch_shape) later -- including one case where I believe they actually would be beneficial.

2. It's fragile. If, at any point, I decide to change _how_ the API key is generated, I'd have to remember to change the name as well to avoid misleading the implementor.

In the end, exposing the appropriate level of detail is more art than science. Undoubtedly, as a codebase evolves, what's "appropriate" will also change. Naming--like coding itself--requires constant upkeep. We're never done with naming so long as the product is still evolving.

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
|Get    |Map             |i.e. mapping types from one app to another (Classic to DD2)
|Update |Overwrite       | 										    |									   |
|Update |Replace 		 |								            |									   |
|Update |Merge	 		 |								            |									   |
|Update |Sync	 		 |(With 3rd party)						    |									   |
|Delete |Remove	 		 |										    |									   |
|Delete |Incinerate	 	 |For hard or permanent deletes.	    	|project.Incinerate()				   |
 
ALSO: POP (e.g. get and delete), PUSH...
