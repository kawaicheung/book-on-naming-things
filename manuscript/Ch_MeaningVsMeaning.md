# Iterating meaning

We're told in code to avoid premature abstractions. Wait until as late as you can before you replace a concrete concept with something abstract. I find the same truth holds with naming things.

I'm working on a piece of code that processes incoming emails. One of the its responsibilities is to send a generic auto-response email back to the original sender if certain conditions are met.

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

With a new method extracted, the conditional, once again, feels tidy. But, now we have a new problem. The conditional is also tautologous. Of course we'd send an auto-response if we should...send an auto-response! When we read the conditional in isolation, we  don't know why we're sending the auto-response. We need to trickle into the `shouldSendAutoResponse()` method to find out. In essence, we haven't gained much of anything in comprehension. We've tucked away some logic that we likely have to open up anyways.

I find this happens a lot with these quick little extraction exercises. The natural inclination is to name the newly extracted construct (beit a method or variable) after the direct _effect_ rather than the _cause_. The _effect_ of the extracted piece of logic is that an auto-response should be sent. But, why are we sending it?

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

--> Now more meaningful.

--> Add auto-response ON +  additional stuff...at a certain point, the full meaning becomes 'shouldSendAutoResponse'...it's also so nuanced and particular, that is the BEST meaning. (whereas in the beginning, we can reuse isOfficeClosed() perhaps for other checks somewhere else...its very unlikely for us to reuse the new `shouldSendAutoResponse` for something else.







