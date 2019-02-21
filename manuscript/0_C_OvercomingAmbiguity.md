## Ambiguity

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
