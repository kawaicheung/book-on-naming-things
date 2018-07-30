## Methods as Verbs

In code, the words we use to describe methods usually boil down to a few commonsuspects. We `Create`, `Get`, `Update`, or `Delete` things. But, we can often substitute these verbs with choices that tell us more about the actions without having to dig into their implementations. Let's take a look at a few examples.

### Update

I'm staring at a method name inside a `Project` class called `UpdateUsers()`.

```C#
public void UpdateUsers(List<Users> users) { … }
```

The name seems straightforward enough. The method should take a list of users and grant them access to the given project. But, let's scrutinize it a bit more. What does the method do to the users that currently have access who aren't in the passed-in list? Will it remove them, leaving access to only the ones I've passed in? Or, will it keep them around, adding additional users from the list? A method named this way doesn't tell me for sure. I have to look inside and see what's going on. 

Once I've wrapped my head around the story (it turns out that existing users not in the list *are* removed), I start with a comment on the method so that I don't have to investigate it again, a month from now, when I'll likely have the same question.

```C#
// Overwrites project's assigned users with the passed-in users list
public void UpdateUsers(List<Users> users) { … }
```

That's better. But, comments tend to easily outdate themselves. If there's a way I can impart that same meaning into the name, I should. The word _overwrite_ is the key to the comment. The method isn't just updating users, it's potentially removing users. _Overwrite_ describes that idea more clearly. Let's see how it looks.

```C#
public void OverwriteUsers(List<Users> users) { … }
```

Another step better. This feels less ambiguous. The method name has a bit more weight to it. _Overwrite_ clearly means the old users aren't sticking around.

Here's another way to judge how the name feels. If I were to pass in an empty list into `UpdateUsers()`, what would it feel like? 

```C#
myProject.UpdateUsers(new List<Users>());
```

Again, I'd likely expect all user access removed. But, I wouldn't be entirely shocked if the method left the project's users intact. If I were to pass the same argument to...

```C#
myProject.OverwriteUsers(new List<Users>());
```

...I'd expect it to do some serious damage. That's just the feeling I want to convey.

I use `Overwrite` instead of, say, `Override` because I like to avoid names that have other reserved meanings. In this case, `override` is a modifier used in many languages to extend or modify the implementation of an inherited method. It's best to stay away from using names that have a prescribed meaning in the language already.

`ReplaceUsers()` could work just as well here. There are days I might favor it, but it feels a touch subtler to me than `Overwrite`. Given the choice, I lean toward stronger words initially for clarity, then scale back later on the harshness over time if it doesn't feel right.

When I can't find that perfect word right away, I'll look up synonyms to the words that feel close. A quick Google search for alternatives to _Replace_ and _Overwrite_ gives me options like _Change_, _Alter_, _Displace_, _Reimburse_, _Substitute_, _Swap_, _Upgrade_, _Improve_, or _Modernize._  None of these feels nearly as good. Some feel entirely strange. 

The reverse is true too --- when I feel like a name is right but I'm not completely sure, searching for synonyms helps me confirm the one I've picked is, at the very least, pretty good for now.

`OverwriteUsers()` still doesn't clarify something about the implementation, though. 

Under the hood, I know that users are linked to projects via `Membership` records. Memberships hold additional bits of information relevant to a user's project access--perhaps the date access was granted, their particular role on the project, or some notification settings tied to that membership. Knowing this, `OverwriteMemberships()` becomes an even more accurate name. 

But, what does `OverwriteMemberships()` do for users who both have a membership currently and are in the passed-in list? As it stands, it sounds like their membership will be removed and re-created, thereby erasing any current information tied to their membership. 

Another peek inside the method reveals that--in fact--the code is delicate with existing memberships. All existing memberships who will continue to have access will stay intact rather than be removed and recreated. There might be a better name to express this additional nuance, like `UpsertMemberships()` or `SyncMemberships()`. 

I don't have the perfect answer here, but this exercise tells us a couple of things.

First, it's hard to pack every subtlety of a method's implementation into its name. Instead, I do my best to get the most important aspects of the implementation into the name. A comment helps with context around any extra nuance.

```C#
// Will keep current memberships in users list intact
public void OverwriteMemberships(List<Users> users) { … }
```

Second, if I can't find a method name that covers enough of the essential parts of what it does, this could be a sign that the method is doing too much. Do I need to break up the method into smaller ones so that each one's intent can be more clearly wrapped in a name?

In this case, I'd prefer to leave the method intact. It's not worth trading away the succinctness of a single function that handles updating a project's memberships as a unit of work for a series of smaller, separated functions just because they might be more perfectly named. I'll live with the comment and move on to more important work.

A> This example _seems_ like a whole lot of thinking for something inconsequential. But, once you get accustomed to critiquing names this way, the exercise becomes a normal, fluid, and instinctual part of the code writing process. You get faster at it. While the name of any one method won't determine whether a suite of automated tests passes or fails, a codebase that's full of expressive and thoughtful names will make your development more enjoyable. 

### Other common verb-traps

Let's talk about the other common programming verbs: `Get`, `Create` and `Delete`. At first glance, it seems like there would be less uncertainty with these words. If we're getting something, we're retrieving it from somewhere without manipulating it. If we create something, we're building something new. If we're deleting something, we're surely getting rid of it in some permanent fashion. But, even these words can be improved.

### Get

For example, I have a couple of overloaded methods in a `PeopleService` class named `GetPeople()`. Both return a list of `Person` objects based on different inputs.

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

Here's another case where `Get` can be improved. I've written an extension method off of the `String` class which pulls out all email addresses for the given string with some magical regular expressions. I use this to be able to notify users if they are mentioned in a user's comment. Calling this method `GetEmails()` is confusing. It doesn't quite make sense in context:

```C#
string comment = "Hey bill@microsoft.com, can I get a raise?";
List<string> emails_in_comment = comment.GetEmails();
```

How exactly do you _get emails_ from a piece of text? Instead, the method is really _extracting_ emails. And since I can't guarantee there will be emails in the string at-hand, I can further clarify that the method will "extract any emails" rather than simply "extract emails." Let's see the result:

```C#
string comment = "Hey bill@microsoft.com, can I get a raise?";
List<string> emails_in_comment = comment.ExtractAnyEmails();
```

I think it's a considerable improvement.

### Create

Now, let's look at a creation example. I've written a method that returns a unique API key for a given user. For this method, I could come up with a name like `CreateAPIKey()`. But, the word _create_ doesn't tell me a whole lot about _how_ the key is created. Is it pulled from some list of available keys in a database? Is it randomly generated? Is it somehow derived from other data a user owns?

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

### Delete 

Now let's consider `Delete`. It's, indeed, a loaded term. On the surface, we know it means something like “get rid of this thing and never let me see it again.” And, that's generally good enough for users. But, programmatically, this could mean a number of things. I might be placing a flag on a record in a database, removing a relationship between items to prevent access, or, actually hard deleting the record for good.

When I use the word `Delete` as part of a method name, I _usually_ don't care about the implementation --- I only care about the meaning in the scope of the class I'm working in. In a method like `Delete(int project_id)` on a `ProjectsController` class, I only care that executing this method means a user can no longer access it. Whether aspects of the project still remain intact behind-the-scenes is irrelevant.

However, I think it's prudent to make an exception when a method does perform a _true, irreversible hard-delete on data that's not easily recreatable_ --- when I'm not softly removing, dropping, clearing, hiding, or deactivating, but in fact, I'm permanently destroying something, for good. [Why? We'll use it with care. Another level of defense.]

Choosing such a word is difficult. It should have even more cautionary weight than _Delete_. 

David Heinemeier Hansson (Basecamp CTO and Ruby on Rails creator) reserves the term _Incinerate_ in Basecamp's codebase to distinguish the "sweaty-palms" kind of delete from the softer, "just put a flag on it" type.  By doing so, it's become part of the technical lexicon for the Basecamp development team. “When we talk about incineration within the app, it means this one, specific thing.”[^dhh1] _Destroy_ or _Eradicate_ could also work. 

[^dhh1]: [On Writing Software Well #6: Actually deleting data, not just pretending to](https://www.youtube.com/watch?v=AoxoPfilKqE)

### Exposing the appropriate level of implementation detail

In each of the examples above, my revised names attempt to describe a method's implementation more clearly. Some would argue that I'm committing a cardinal programming sin -- one of the fruits of encapsulating logic in methods is to _hide_ the details of the implementation. How dare I try to expose them more!

But, one of the tricks of good method naming is to expose _as much_ detail as is useful. The more knowledge we ascertain from the name of a method, the faster we get at choosing the right methods to use or deciding to add additional ones to fulfill the task-at-hand.

For instance, in the API key generation example above, I felt that `GenerateAPIKey()` was a better choice than `CreateAPIKey()` because it describes _how_ an API key is created with a little more detail that might be useful to the implementor.  

I certainly could've carried the name even further. However, would a name like `GenerateAPIKeyUsingRandomizedGuid()` be better? If there were other API generation mechanisms at play that required alternative methods, and the mechanism mattered outside of the method itself, then absolutely. In my case, there wasn't.

A name like `GenerateAPIKeyUsingRandomizedGuid()` also presents a couple more issues:

1. It's long, and long names can reak havoc on the overall shape of code. By the way, we'll talk about long names and [code shape](#ch_shape) later -- including one case where I believe they actually would be beneficial.

2. It's fragile. If, at any point, I decide to change _how_ the API key is generated, I'd have to remember to change the name as well to avoid misleading the implementor.

In the end, exposing the appropriate level of detail is more art than science. Undoubtedly, as a codebase evolves, what's "appropriate" will also change. Naming--like coding itself--requires constant upkeep. We're never done with naming so long as the product is still evolving.

### Alternative verb ideas

In the introduction, I mentioned that this book is not a reference manual. But, I thought it would be useful to list a bunch of synonyms I frequently use as replacements to conventional action terms. Hopefully, this list inspires you to think of more informative method names.

|Verb   |Replacement     |Use case                                  |Example                               |
|-------|----------------|------------------------------------------|--------------------------------------|
|Create |Generate   	 |Creating something with randomness.		|GenerateAPIKey()				   	   |
|Get    |Extract, Expose |Exposing something that already exists. 	|string.ExtractAnyEmails()	   		   |
|Get    |Convert         |Getting a manipulation of an object.		|date.ConvertToShortDate()			   |
|Update |Overwrite       | 										    |									   |
|Update |Replace 		 |								            |									   |
|Update |Merge	 		 |								            |									   |
|Delete |Remove	 		 |										    |									   |
|Delete |Incinerate	 	 |For hard or permanent deletes.	    	|project.Incinerate()				   |

