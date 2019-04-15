# Traps, Corners, and Confusion

[Mesh these two chapters into one + fluent interfaces discussion here?]

**Opposites don't always attract**

Much of programming is about tracking states. A user might be active or inactive. A purchase order is either pending or processed. 

Code tends to be more readable if concepts are stated positively. Anytime I see a variable with a not operator (`!`), I see if there's another variable I can create that expresses the same concept without the `!`. For instance, a conditional statement like `if (!won)` can be improved by replacing it with `if (lost)`.

Usually, the English language has enough breadth in vocabulary that most states have a meaningful opposite. For a piece of functionality that allows us to move a file from the trash, we don't have to say _undelete_. _Restore_ makes perfect sense. It's clear that a file that can be restored is already in the state of being deleted.

Unfortunately, this isn't always the case. That makes naming a state's opposite succinctly a bit of a challenge. Take this example.

There's a concept in DoneDone called _Workflows_. A workflow defines a series of statuses that an item can be in. A typical issue tracking workflow might have states like "Open", "In Progress", and "Closed". We let users create workflows to tailor them to their own business processes. When a user is ready to use the workflow, it has to be _published_.

A user can also--for lack of a better verb--_unpublish_ a workflow. The requirements for a workflow to be "unpublishable" are more than just that it's already been published; There are a few other caveats as well. So, I decide to wrap this logic inside of a convenient little method. My first attempt at a method name is:

```
public bool IsWorkflowUnpublishable();
```

However, there's something unsavory about this name. We could easily mistake this as a check that a workflow *cannot be published* rather than a check that a workflow *can be unpublished* -- two very different things.

The problem is that there just isn't a good adjective that means the opposite of "published". There aren't even a good few words for it. "In draft mode" might be the best way to describe the opposite of published, but a method name like `IsWorkflowAbleToBeInDraftMode()` or `IsWorkflowDraftModable()` feels like we're headed in the wrong direction fast.

In cases like these, it helps to look for a completely different angle to the name. The obstacle with this method name is with the initial word "is". It corners us into having to come up with an adjective to describe the state of the workflow we want to achieve -- and there aren't any good ones.

Instead, we can get the same clarity across by changing the angle of the method name. Instead of starting the name with the adjective-requiring "is", I start with the verb-requiring "can."

```
public bool CanUnpublishWorkflow();
```

Now we might be onto something! Notice I still use the word "unpublish", but as a verb it's no longer ambiguous. It's clear we are checking whether this _workflow can be unpublished_ as opposed to checking whether the _workflow can't be published_.

If the opposite version of a name makes your method ambiguous, instead of pushing harder on finding a different name, see if you can reword the method altogether. You might already have all the pieces you need without knowing it.

**The tautologous name trap**

It's good habit to look for business logic that we can extract into a method or variable. Consistently doing this infuses better meaning into our code. It makes it easier to understand our intentions later on. But, naming these extractions appropriately can be more elusive than it initially appears.

I'm working on a piece of code that processes incoming emails for DoneDone. One of its responsibilities is to send an auto-response email back to the original sender if certain conditions are met.

In the first iteration of this feature, we decide to initiate the auto-response only if the original email was received outside of a company's office hours. 

I've written a method responsible for this work, querying the company's work hours and comparing these date ranges to the current `DateTime` to answer the question. 

```C#
bool isCurrentlyOutsideOfficeHours();
```

In my incoming email handler, I call this method to determine if an auto-response is necessary.

```
if (isCurrentlyOutsideOfficeHours()) 
  sendAutoResponse();
```
The method name `isCurrentlyOutsideOfficeHours()` makes the conditional statement read coherently. Even a non-programmer can read the statement above and deduce it's saying the following:

> If we are currently outside the office hours, then send an auto-response.

A few weeks later, we receive some requests from our customers to allow them to configure company holidays as well as office hours. This way, the auto-response will always be triggered during a company holiday regardless of the time of day.

Back under the hood, we add the new functionality. I then update the auto-response logic to check whether today is a holiday, and adjust my auto-response logic accordingly. This requires another method that handles the dirty work. I call it `isCompanyHoliday()`.

```C#
bool isCurrentlyOutsideOfficeHours();
bool isCompanyHoliday();

...

if (isCurrentlyOutsideOfficeHours() || isCompanyHoliday()) 
{ 
  sendAutoResponse(); 
}
```
Now, I've added more complexity to the auto-response logic -- enough to make me consider consolidating the conditional expression into its own method, then calling the new method in place of the expression. At first, a name like `shouldSendAutoResponse()` sounds sensible. Here's what that would look like:

```C#
bool shouldSendAutoResponse()
{
  return isOutsideOfficeHours() || isCompanyHoliday();
}

...

if (shouldSendAutoResponse()) 
{ 
  sendAutoResponse(); 
}

```
With the new method extracted, the conditional, once again, feels tidy. But, now we have a new problem. The conditional statement is tautologous: 

> If we should send an auto-response, then we should send an auto-response. 

That's not a particularly helpful statement for the code reader. 

When we read the conditional in isolation, we don't know _why_ we're sending the auto-response. We need to trickle into the `shouldSendAutoResponse()` method to find out. The extraction doesn't give us better comprehension; It merely tucks away some logic that we likely will have to drill into later anyways.

I find this happens a lot with these quick extraction exercises. Our immediate inclination is to name the newly extracted method (or variable) after the outcome of the conditions being met rather than the meaning of the conditions. We name the method after the _effect_ rather than the _cause_.

Not only does this create tautalogous statements, but it's less likely we'll reuse the new construct somewhere else. At a glance, we wouldn't think to employ `shouldSendAutoResponse()` anywhere else besides the place in code where we want to send auto-responses. If we named the method after what's _causing_ the sending of the auto-response, we better our chances of reuse later.

So, why are we sending the auto-response? In this case, the cause of sending an auto-response message is because, quite simply, the office is closed. Let's try that.

```C#
bool isOfficeClosed()
{
  return isOutsideOfficeHours() || isCompanyHoliday();
}

...

if (isOfficeClosed()) 
{ 
  sendAutoResponse(); 
}
```
The conditional now reads more meaningfully. In addition, `isOfficeClosed()` is a method that has far more obvious applications than `shouldSendAutoReponse()` does. For instance, I could call it to determine if a chat application should be disabled on the application's help site. This has nothing to do with the auto-response feature.

Tautologous conditionals aren't necessarily bad, though. There are times where describing the _effect_ is the cleanest option. We'll continue with this example. 

Suppose that we introduce a few more conditions to determine whether to send an auto-response. Namely, we let a user toggle auto-responses altogether and we also want to exclude auto-responses to emails that are flagged as spam. My first step is to augment the conditional one more time.

```C#

if (isOfficeClosed() && autoResponseEnabled && !_email.IsSpam) 
{ 
  sendAutoResponse(); 
}

```

The conditional expression bloats up again and it seems ripe for packaging things up. But, I have trouble finding an elegant solution. Is there a meaningful name that could consolidate all or some of the expression? `isOfficeClosed()`, `autoResponseEnabled`, and `!_email.IsSpam` don't appear to have any relationship to one another other than determining whether we should send an auto-response.

In this case, I can decide to either leave things as is, or name the entire expression for what it affects. I choose the latter.

```C#

bool isOfficeClosed()
{
  return isOutsideOfficeHours() || isCompanyHoliday();
}

bool shouldSendAutoResponse()
{
  return isOfficeClosed() && autoResponseEnabled && !_email.IsSpam;
}

...

if (shouldSendAutoResponse()) 
{ 
  sendAutoResponse(); 
}
```
In this case, there's far less chance there would be any another meaning given to `isOfficeClosed() && autoResponseEnabled && !_email.IsSpam` being true. We live with the tautologous argument to keep the statement succinct, and we're also unlikely to miss an opportunity to reuse this method elsewhere.

This brings up an important general idea we see throughout all programming concepts, particularly with naming. We make tradeoffs depending on how a section of code evolves. When I can find an apt name for the _cause_, I choose it. When I can't, I can convince myself that naming an extraction for its _effect_ is still better than not extracting it at all. In the end, the goal is to continually rename things to produce the most coherency given each situation.

**Abstracting too soon**

As I write code, I usually start with names that describe their constructs concretely before gradually iterating toward something more abstract. Proper abstractions need to be made at the right pace. Abstract too slowly and it will be the feature creep that steers the ship. Abstract too soon and the boat will soon be sinking under its own weight.

[Maybe Sandi Metz bit here about championing the idea of duplicate code better than the wrong abstraction?]

Naming should also undergo the same kind of scrutiny. The gradual reshaping of our codebase also warrants constant re-evaluation of the names we give to the constructs in our code. 

It's taxing work. And I think it leads programmers (including myself) to abstract the meaning of a name too early in the process - a desparate attempt to get a name right, once and forever. But I've never found prematurely abstracting a name helpful. In fact, it often inflicts more pain than comfort, just like premature code abstraction does.

I'm going through a refactoring in DoneDone's codebase to consolidate a few data objects that hold very similar kinds of information in slightly different ways. They all revolve around the history of actions applied to an issue.

Ultimately, the properties of these objects manifest in a few different places. On an issue's detail page, the collection of history for a single issue is written out from an `IssueDetailHistory` collection. On the activity dashboard, a list of history on all issues in a given day is extracted from a list of `ActivityHistory` items. When activity occurs on an issue, yet another "history" object is packaged up in a class with a list of emails to send to specific users. A mailing service unpacks the data and formats the right bits of information into emails.

Each of these objects look nearly identical. They all could derive from a single class, but they've all been written separately from each other over time. It's time to consolidate them.

After a fair amount of hair-pulling, I've reduced these classes into a single `IssueHistory` class that can support the needs of each of these three distinct use cases, and more going forward. This also gives me an opportunity to consolidate similar names in the old objects like `CreatedOn`, `CreatedDate`, or `CreateDate` into one consistent name (I like `CreatedOn`, for what it's worth).

In the process of the consolidation, I discover one particular string off of the old history object used by the mailing service. It's a terse description used as the beginning of the email subject, like "New fixer" or "Priority Update" or "Closed". (The rest of the subject is composed of the issue number and title). 

The dilemma I face here is what exactly to call this string. At present moment, it's _only_ used by the mailing service and not by either the detail page or activity dashboard. But, since I've done all the pruning to get toward a single object, I feel compelled to come up with a name that can satisfy all future implementers.

My natural inclination is to call this something like `AbbreviatedAction`, especially because it juxtaposes nicely with the `Action` attribute that stores a more verbose description of the action ("John Doe was assigned to fix the issue" or "The priority was updated to Critical") I go with this name.

After a few days of refactoring the rest of the code, I find myself going back to this name a couple of times not entirely remembering what it's being used for. I even decide to comment the name to remind me it's only being used by the mailing service as part of the subject of the email. 

```C#
// Note: Only being used by the mailing service right now as part of the email subject, but I'm sure it will have other uses one day.
public string AbbreviatedAction
{
  get { … }
}
```

Whenever I have to remind myself what a name means (and eventually surrender to the commenting devils) there's a good chance I've prematurely abstracted the name. 

Instead of finding a name that simultaneously fits anything but nothing in particular, it's best I reduce the scope of the name's meaning to fit how it's being used right now. A name like `EmailSubjectAction` ends up working a lot better.

```C#
public string EmailSubjectAction
{
	get { … }
}
```

Some might worry that a name that's too specific like this might hide the full capabilities of that construct. That, one day, I'll need the very thing that `EmailSubjectAction` brings to the table (a terse description of an issue action) for something entirely unrelated to email subjects and I will never realize it existed. Instead, I'll create a near-identical version of that construct and name it something else.

In my experience, I rarely find this to be the case. If I'm familiar enough with the codebase, I recall some part of it that replicates (or nearly replicates) what I'm trying to implement by thinking about the particular feature ("What I need is just like that first part of that subject line of the emails that are sent when issues are updated!") rather than the particular name of the relevant construct (`EmailSubjectAction`). The more specific name just helps me find where that construct lives, faster.

Others might argue that I really should move this particular property to its own class and inherit the `IssueHistory` class. This way, I can use this class specifically for the emailing case. But, to me, the additional overhead of managing another class just for the sanctity of keeping the name exposed only to those who need it isn't worth the tradeoff. I'm OK with exposing this property to the other use cases especially if it's the only one of its kind. If this starts becoming a theme with other properties, then I might sway the other way. But, it's too early now.

Once that construct is being used in multiple different ways, then I can justifies the broader name change. In my case, `AbbreviatedAction` may eventually be the best name one day, but only when its set of use cases warrants it.

