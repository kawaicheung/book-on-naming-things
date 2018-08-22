# What happened vs. what it means?

One of the trickiest parts of naming a method is deciding which is more relevant to describe: What it _does_ or what it _means_. The difference can feel subtle at first but it has a big impact in how the implemented method reads. What makes it more difficult is that there isn't always a right answer. And that answer might change as the application evolves.

I recently did a major update on the billing functionality of DoneDone. While the original version billed accounts monthly based on a few pricing tiers which allowed a certain cap on users, in the latest version, accounts are billed monthlhy on a _per_ user basis.

Billing per user means that I have to know when any updates are made that would affect an account's overall membership count. When such an update is made in the app, it sends the membership count change to our third-party billing system so that the appropriate adjustments are made on the next invoice.

Thankfully, all the logic surrounding proration credits or debits are the responsibility of the billing system. All I need to do is ship the account's updated user count to the billing system each time it changes.

When working with a third-party system in a scenario like this, I usually offload the work to a worker process that runs separately from the user application. I do this for a couple of reasons.

First, there might be noticeable lag in connecting and making updates to a third-party system's API. I'd rather not pass that delay onto the user. Second, the user count update doesn't have to be instantaneous. If the billing system's data lags behind a few seconds (or even minutes), it's not a huge deal.

In order to offload the billing system updates, I need to flag an account in our local database when a user update happens. Then, a separate worker process can run on a periodic basis to look up these updated accounts, grab the new user count, and pass that amount to the billing system. The pattern is  straightforward -- it's one I've implemented a handful of times. 

I use a service/repository architecture so there are three service methods of I need to make an update to: when a new user is created, when an existing user is updated, or when an existing user is deleted. Inside those methods, I make an additional call to an `AccountsRepository` that flags the account.

What's an appropriate name for this method? My first stab is `FlagAccountAsUserCountUpdated()`.  This describes, rather directly, what the method does.

[Add BACK the idea that 'update user' is required because they may change roles and we charge on different roles. ]

[SyncWithBilling is more broad...more flexible in a way. but is that even important? it makes the readability of the sweeper side more tautologous. Is that better?  But, it also helps describe the reasoning for the flagging better on the 3 methods. Otherwise, the 'UpdateUserCount' approach makes the 3 methods have a tautologus thing... "yes it's being updated" And, my vote is that I'm going to be in those 3 methods more than the sweeper, so impart more MEANING on that side vs. on the side ill be looking at/updating less. ]


Do I want to connote wh

In each of these methods, I add in 

But, naming the pieces that support the pattern requires some deliberation.




MarkAccountAsRequiringSyncWithBilling

vs

MarkAccountAsUserUpdated

often get into this pickle of naming what happened vs the result of what ahppened.


lead into shouldSendEmail?


Take, for instance, this simple bit of code:

```C#

bool shouldSendEmail = user.IsHRManager;

...

if (shouldSendEmail)
{
	
}

```

SendAutoResponse” vs “IsOutsideOfficeHours”

What ends up happening is a tautalogous statement -- If i should send the auto response, then send the auto-response. At this point of the code, the intent's been obscured. Now, at times this is a good trade. Particularly, if the logic required to derive whether an auto response is sent is complex. We're packing all of that detail away from the point at which we care about it.  But, otherwise, we should keep the details of "what" in place. "IsOutsideOfficeHours" tells us what. If we're outside office hours, then send the auto-response is a more meaningful line.

But, as the code morphs and the logic potentially gets more complex, that method might now be too complex to formulate the name as a 'what'. IfOutsideOfficeHoursAndNotAHolidayAndProjectHasAutoResponsesSetToOn is a bit much. That's when the result makes more sense: ShouldSendAutoResponse.