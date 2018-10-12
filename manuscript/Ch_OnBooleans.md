# On naming booleans

A programming language feels most like traditional language at the most fundamental level -- with booleans and logic operators. So it's to our benefit to make this type of code _read_ as fluidly as possible.

A statement like `if (published && editable)` can be read pretty much as it's written -- "if published and editable".  Most people who've never touched a programming language would be able to comprehend code at this level of detail.

Yet, we can easily convolute these statements when the booleans involved are poorly named or logical operators aren't edited down to their simpest forms.

The simple statement above is identical to `if (!(unpublished || !editable))`. But, the mess of nested scopes, misdirections, and negations make it nearly impossible to deduce this right away. It's the kind of statement that still works and passes tests, but has developed the cruft common to code that's been manipulated a few times without someone taking the time to prune it.

You can tell a boolean's name is hurting by reading how it's used in context. "If it's not either unpublished or not editable" reads more like a crafty lawyer's statement than a straightforward piece of writing.

Introducing the right booleans to an object can significantly improve how code reads. Take this example -- I have an `Issue` object with an optional `Assignee` that represents the `Person` assigned the issue. There are other properties of `Issue` but I'll omit those here.

```C#
class Issue
{
  ...
  Person Assignee { get; set; }
}
```

When an issue is updated, I want to notify the assignee of the update. This requires a quick check to confirm the issue has an assignee.

```C#
if (issue.Assignee != null)
{
  sendNotificationTo(issue.Assignee);
}
```

I can improve how this code reads by introducing a boolean property on the `Issue` class which checks the `Assignee` property directly. Then, I replace the nullability check with the new boolean.

```C#
class Issue
{
  ...
  Person Assignee { get; set; }

  bool IsAssigned 
  {
    get
    {
      return Assignee != null;
    }
  }
}
```
```C#
if (issue.IsAssigned)
{
  sendNotificationTo(issue.Assignee);
}
```
Now, the conditional reads more fluidly -- "If the issue is assigned" rather than "If the issue's assignee is not `null`". There are other viable names I consider with the boolean like `HasAssignee` or `AssigneeExists`. But, `IsAssigned` reads slightly more appropriately to me; It focuses the attention on the issue rather than the assignee. It also avoids some of the technical stiffness that the word `Exists` conveys. Yes, we're programmers. But, we're readers first.

A few weeks later, I have a new requirement in a different part of the codebase. If the issue is not assigned to anyone and it's deleted, I want to notify the project manager of the deletion. Here's the snippet of code that alerts the project manager/

```C#
if (!issue.IsAssigned)
{
  sendAlertTo(projectManager);
}
``` 

I can make the conditional line better if I can get rid of the leading `!`. Just like I had earlier, I tack on another boolean property to the `Issue` called `Unassigned` and then update the conditional.

```C#
class Issue
{
  ...
  Person Assignee { get; set; }

  bool IsAssigned 
  {
    get
    {
      return Assignee != null;
    }
  }

  bool Unassigned 
  {
    get
    {
      return !IsAssigned;
    }
  }
}
```
```C#
if (issue.Unassigned)
{
  sendAlertTo(projectManager);
}
``` 

Again, the conditional reads more clearly. The line `!issue.IsAssigned` requires a few semantic hops -- "if not issue is assigned" to "if issue is not assigned" to "if issue is unassigned". I avoid those hops by adding the `Unassigned` boolean to the `Issue` class itself.

There are other names that might work just as well. I could be persuaded to use `IsUnassigned` as it's more consistent with `IsAssigned`. But, taking the name in isolation, `IsUnassigned` reads a little oddly in the beginning compared to simply `Unassigned`. `NotAssigned` is also a good candidate -- but `Unassigned` says the same thing in one less word.

Now let's take a step back. While I've improved a couple of conditional statements, it comes at the expense of adding new properties to a class. Some would argue that I've bloated the `Issue` class unnecessarily. For instance, having both an `IsAssigned` and `Unassigned` property now means there's more to maintain. 

But, both properties are getters that are dependant upon the already existing `Assignee` property. These properties are all anchored off the same object -- there's nothing _more_ to maintain.

Other purists might have a philosophical problem with the idea of having both a positive and negative version of the assignee's existence. They might argue that we've repeated ourselves (a coding sin!) But, I far prefer the benefit of writing more readable code than the perceived consequence of having two properties that essentially say the same thing.

Finally, others might still argue against having a negative boolean like `Unassigned`. Well, certainly if we were to use this property as `!Unassigned`, but this is where developer discretion needs to take place. Besides, not all negative booleans are "bad". 

Would you rather say `!won` or simply `lost`? `!Active` or `Canceled`? There are words we can use that, underneath, mean the negative of the preferred outcome but aren't thought of as "not something". Ultimately, it depends on how we use them in context.




---



won, !won = lost.


How do we prevent this?

(1) Removing ! where possible

(2) Negative booleans are usually bad...but that's not necessarily true.

(3) Overall, read thru how the booleans are used in statements. See if the statement itself can be contorted.

(4) Ternary form...overly complex if more than a small bit.


When is it ok to have a negative boolean?

not_billing_page in base controller...

Make names based on readability today, rather than futureproofing guesses... 


Booleans are most used in programming with "logic sentences" vs. say an object where we might just be working on it -- a constrcutor... so booleans need to "read well" in the context of the logic statements -- most like sentence writing.

won, !won = lost. (Is there a "Not" version of that word)

IsAdmin, IsNormalUser

1. Does it read clearly?
2. Remove the most amount of 'flipping' (!won -> not_won -> lost), (!isAdmin = isNormalUser)
3. Perimssions.UserCanAccessProject -> Use case is always !, so how about Permissions.UsercannotAccessProject? But that takes a little mental tweaking. How about Permissions.UserDeniedAccess?
3. Whereas if (billing_page) { // do nothing } else { ... } vs. if (!billing_page) {  }...feels weak now.


Gold / Silver / Bronze / Paper

if (Paper)

maybe...

if (no_user)
else (no_avatar)
else (no_initials)
else // user & avatar & initials

vs

if (user && avatar & initials)
else if (initials & !avatar & !user)
else


unless vs. if not -- Not just about COMPACTNESS but about readability.
