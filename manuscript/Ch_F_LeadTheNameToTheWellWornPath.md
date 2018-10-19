# Lead the Name to the Well-Worn Path

As I've just discussed avoiding tautologous names, there are certain cases where they aren't avoidable. Rather, it's a matter of choosing which one makes more sense.

The most common pattern I can think of this occuring is, broadly speaking, the event dispatcher pattern. It's a pattern I first used in my long gone days of writing ActionScript for Flash apps. The conceptual idea is pretty simple: Some object can dispatch an event and other susbcribers to said event are notified immediately. For instance, in a Flash game, a player squashes the enemy, and an `ENEMY_SQUASHED` event is emitted. In turn, the various objects listening for the event perform their own work: updating the user's score, spawning more enemies, and playing a sound effect.

Flash was fun.

These days, I'm writing more "serious" code for data-driven web applications, but I still use the general event dispatcher conceptually every so often.  Here's an example.

I recently did a major update on the billing functionality of DoneDone. While the original version billed accounts monthly based on a few pricing tiers which allowed a certain cap on users, in the latest version, accounts are billed monthlhy on a _per_ user basis.

Billing per user means that I have to know when any updates are made that would affect an account's overall membership count. When such an update is made in the app, it sends the membership count change to our third-party billing system so that the appropriate adjustments are made on the next invoice.

Thankfully, all the logic surrounding proration credits or debits are the responsibility of the billing system. All I need to do is ship the account's updated user count to the billing system each time it changes.

When working with a third-party system in a scenario like this, I usually offload the work to a worker process that runs separately from the user application. I do this for a couple of reasons.

First, there might be noticeable lag in connecting and making updates to a third-party system's API. I'd rather not pass that delay onto the user. Second, the user count update doesn't have to be instantaneous. If the billing system's data lags behind a few seconds (or even minutes), it's not a huge deal.

In order to offload the billing system updates, I need to flag an account in our local database when a user update happens. Then, a separate worker process can run on a periodic basis to look up these updated accounts, grab the new user count, and pass that amount to the billing system. The pattern is  straightforward -- it's one I've implemented a handful of times. 

I use a service/repository architecture so there are three service methods of I need to make an update to: when a new user is created, when an existing user is updated, or when an existing user is deleted. Inside those methods, I make an additional call to an `AccountsRepository` that flags the account.

What's an appropriate name for this method? My first stab is `FlagAccountAsUserCountUpdated()`.  This describes, rather directly, what the method does.


===


There are times where you have a choice between two method names that are entirely distinct but seem equally appropriate. In those cases, I like to choose the name that represents the path I'm most likely to take when I stumble across the method. Here's what I mean.

Title idea: You have a 50/50 choice in names, one can talk about the concept one way (FlagAsUserUpdated) or the other way (FlagAsSyncWithBillingSystem).  The "tautology" kind of... (as we saw in the previous chapter) will live on one side or the other (either in all user update methods that have this added FlagAsUserUpdated() method or in the single billing system sync with the (GetALlForFlaggedAsNeedingSync). So which way?

Cater to the path you'll likely "see" it the most... (there are more cases where the user is updated than we are syncing, plus more modifications are likely made in those methods SLASH they play a role to the core of the app more than this one-off billing system sync so name it where its most obvious from THAT STANDPOINT -- e.g. FlagAsNeedingBillingSync).

===


[Add BACK the idea that 'update user' is required because they may change roles and we charge on different roles. ]

[SyncWithBilling is more broad...more flexible in a way. but is that even important? it makes the readability of the sweeper side more tautologous. Is that better?  But, it also helps describe the reasoning for the flagging better on the 3 methods. Otherwise, the 'UpdateUserCount' approach makes the 3 methods have a tautologus thing... "yes it's being updated" And, my vote is that I'm going to be in those 3 methods more than the sweeper, so impart more MEANING on that side vs. on the side ill be looking at/updating less. ]

