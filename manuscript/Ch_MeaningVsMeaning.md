# Iterating meaning

We're told in code to avoid premature abstractions. Wait until as late as you can before you replace a concrete concept with something abstract. I find the same truth holds with naming things.

I'm working on a piece of code that processes incoming emails. One of its responsibilities is to send a generic auto-response email back to the original sender if certain conditions are met.

In the first iteration of this feature, we decide to send an auto-response email only if the original email was received outside of a company's office hours. I've written another method that does this dirty work -- querying the company's work hours and comparing date ranges to the current date and time before returning a boolean to answer the question. I then call on this method to determine if an auto-response is necessary.

```C#
bool isOutsideOfficeHours();

...

if (isOutsideOfficeHours()) 
{ 
	sendAutoResponse(); 
}

```

I choose the name `isOutsideOfficeHours()` because that's precisely what the method is doing. The conditional statement also reads pretty coherently: If we're outside office hours, then send an auto response.

A little while later, we receive some complaints from our customers that some folks aren't receiving an auto-response on company holidays. This makes sense. Up until now, we haven't been tracking company holidays, so any emails sent on those days during normal office hours are still seen as being within office hours for that day.

So, we decide to allow a company to log holidays. Back under the hood, I add another method to check whether today is a holiday, and adjust my auto-response logic accordingly.

```C#
bool isOutsideOfficeHours();
bool isCompanyHoliday();

...

if (isOutsideOfficeHours() || isCompanyHoliday()) 
{ 
	sendAutoResponse(); 
}

```

I've added some complexity to the auto-response logic -- enough to make me consider consolidating the conditional expression into its own method, then calling the new method in place of the expression. At first, a name like `shouldSendAutoResponse()` sounds sensible. 

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

With a new method extracted, the conditional, once again, feels tidy. But, now we have a new problem. The conditional is tautologous: If we should send an auto-response, then...send an auto-response. When we read the conditional in isolation, we don't know why we're sending the auto-response. We need to trickle into the `shouldSendAutoResponse()` method to find out. In essence, we haven't gained much of anything in comprehension. We've just tucked away some logic that we likely will have to drill into later anyways.

I find this happens a lot with these quick little extraction exercises. Our natural inclination is to name the newly extracted construct (beit a method or variable) after its direct _effect_ (the outcome of the conditions being met) rather than the _cause_ (what the conditions actually mean). It's the simpler naming choice because we are staring at the effect in code while we're doing the extraction. In this case, the _effect_ of the extracted piece of logic is that an auto-response should be sent. But, why are we sending it?

Also, by naming the extraction this way, it's less likely we'll reuse it somewhere else. At a glance, I wouldn't think to employ `shouldSendAutoResponse()` anywhere else besides the place in code where I want to send auto-responses.

A much better name for this method would succinctly describe the cause. In this case, the cause of sending an auto-response message is because, quite simply, the office is closed.

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

The conditional now reads more meaningfully. In addition, `isOfficeClosed()` is a method I can see myself reusing far more often than `shouldSendAutoReponse()`.

Tautologous conditionals aren't necessarily bad, though. There are times where describing the _effect_ is the cleanest option. We can stick to this example. Suppose that we introduce a few more conditions to determine whether to send an auto-response. Namely, we let a user toggle auto-responses altogether and we also want to exclude auto-responses to emails that are flagged as spam. 

```C#

if (isOfficeClosed() && autoResponseEnabled && !_email.IsSpam) 
{ 
	sendAutoResponse(); 
}

```

The conditional expressions bloats up again and it seems ripe for packaging things up. But, I have trouble finding an elegant way to do so. Is there a meaningful name that could consolidate all or some of the expression? `isOfficeClosed()`, `autoResponseEnabled`, and `!_email.IsSpam` don't appear to have any relation to one another other than determining whether we should send an auto-response.

In this case, I can decide to either do nothing or name the entire expression for what it affects. I choose the latter.

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

The decision is certainly arguable as I've reintroduced the tautologous conditional. But, there's little chance there would be any another meaning given to `isOfficeClosed() && autoResponseEnabled && !_email.IsSpam` being true. We live with the tautologous argument to keep the statement succinct, but we're unlikely to miss an opportunity to reuse this method elsewhere. 

[Can we explain this last bit better? Is the point of this and the last example clear enough?]

====

ANOTHER EXAMPLE:

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








