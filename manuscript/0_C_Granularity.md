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
