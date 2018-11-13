## Overcoming Ambiguity

[Quick Intro regarding Ambiguity]
[Ambiguity can mean a major rewrite...but a more pragmatic short-term fix is to name it better. You can make a larger scale fix later but removing the ambiguity can delay it (because it's not that important and isn't hurting) or at the very least make it more identifiable later---we'll see that later with 'really long names'?

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

This example _seems_ like a whole lot of thinking for something inconsequential. But, once you get accustomed to critiquing names this way, the exercise becomes a normal, fluid, and instinctual part of the code writing process. You get faster at it. While the name of any one method won't determine whether a suite of automated tests passes or fails, a codebase that's full of expressive and thoughtful names will make your development more enjoyable. 
