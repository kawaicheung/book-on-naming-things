## There's a Word for That

Sometimes, the hardest part about writing prose is finding that perfect word. The one that entirely alters the feeling of a sentence. The one that describes exactly what we're trying to say. 

We should scrutinize the words we use to name methods the same way. The difference between two word choices won't have an effect on the compiler's ability to understand our code. A poor word choice won't fail an automated test. But these are all the more reasons to pay close attention to the nouns and verbs we use to describe methods.

Here's an example.

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

------

Here's another example. I have a couple of overloaded methods in a `PeopleService` class named `GetPeople()`. Both return a list of `Person` objects based on different inputs.
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

------

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
