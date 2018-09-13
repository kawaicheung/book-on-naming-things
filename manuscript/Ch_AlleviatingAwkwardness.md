# Alleviating awkwardness

Sometimes, the name of a parameter feels awkward not because of the name itself, but how it dictates we use the method. Here's an example where I start with a seemingly sensible parameter addition but work my way to a more readable solution.

I'm working on a feature addition to canceling accounts in DoneDone. Up until now, if you wanted to cancel the account, it's immediate and irreversible. That works for the vast majority of customers.

However, some customers want to cancel their account at the end of their term, which could be several months out. Rather than require them to remember to cancel the account in a few months, they'd like the account to _automatically_ cancel on the last day of the term. I start implementing the solution.

There are now two cancellation types. We can cancel immediately or cancel at the end of a period. To begin the implementation, I start by tacking on a parameter to a `CancelAccount()` repository method. Here's what it looks like currently:

```C#
public void CancelAccount(int account_id);
```

I need to pass this extra important bit of information in. Since there are now two cancellation options, I choose the simplest parameter type that fills the need -- a boolean. Now, I can pass in `true` to handle this new special case.

```C#
public void CancelAccount(int account_id, bool cancel_at_period_end);
```

After I've implemented the new code to handle the update, I now update all existing references to this method that handle immediate cancellations. 

```C#
_billing_repository.CancelAccount(account_id, false);
```

However, something feels awkward to me about this line of code.

At a glance, it's hard to tell what the passed-in `false` parameter means. Having just written the updates, it makes sense to me now, but it won't to someone else. They'll have to look at the method signature and perhaps even drill into the method to be sure. 

I feel compelled to add the comment above each call to `CancelAccount()` for clarity.  It also helps differentiate between the new code I'll be adding later to handle the additional option of canceling at the end of the period.

```C#
// Cancel the account immediately...
_billing_repository.CancelAccount(account_id, false);
```

Better. But the method call still feels strange. The standard cancellation case (canceling immediately) accepts the `false` parameter. Passing in `false` as the default just feels odd -- it's as if I have to suppress something to perform the default action.

I can get around this pretty quickly though. Since I'm working with a boolean parameter, I can simply flip the name of the option around so that the standard case passes in `true` and update my code accordingly. I swap the `cancel_at_period_end` parameter, with `cancel_now`. And voilà!

```C#
public void CancelAccount(int account_id, bool cancel_now);
```

```C#
// Cancel the account immediately...
_billing_repository.CancelAccount(account_id, true);
```

Now, the default case passes in `true`. But, I've introduced a more onerous problem. By simply reading the `CancelAccount()` method signature, I can't quite tell what passing in `false` would do. What would it mean for `cancel_now` to be `false`? Would it cancel in a day? In a month? At the end of the period? 

At this point, I've exhausted my options with the boolean parameter. While it allows for both options, the options aren't clear from the method signature.

I often find this is the case with booleans when the concept it describes is binary but the options aren't strict _opposites_. That's the case here -- the natural opposite of `cancel_now` isn't `cancel_at_period_end` in the way the natural opposite of `open` is `closed`.

To improve, I could try an `enum` instead.

```C#
enum CancelationType 
{
  NOW,
  AT_PERIOD_END
}

...

public void CancelAccount(int account_id, CancelationType cancel_type);

...

_billing_repository.CancelAccount(account_id, CancelationType.NOW);

```

This is an improvement. I've gotten rid of the ambiguity issues we had with the booolean parameter. It also leaves us better positioned to introduce additional cancelation types in the future.

But, having worked with DoneDone since its beginning, I'd bet there won't be any new cancelation types added soon. It just doesn't feel like one of those features we'd continually augment in the near future. 

Weighing this factor in, an even more readable approach is to directly convey the type of cancelation in the method name. I ultimately decide to create two distinct cancelation methods.

```C#
public void CancelAccountNow(int account_id);
public void CancelAccountAtPeriodEnd(int account_id);
```

With this update, now both the standard and unique cancelation implementations read as informatively:

```C#
_billing_repository.CancelAccountNow(account_id);
_billing_repository.CancelAccountAtPeriodEnd(account_id);
```

I also get some additional benefits from this approach. Breaking the method out into two methods allows me to separate the implementations of each. In the original approach, I'd have to do something like this:

```C#
void CancelAccount(int account_id, bool cancel_at_period_end)
{
  if (cancel_at_period_end)
  {
    // Implementation for canceling at period end
  }
  else
  {
    // Implementation for canceling immediately
  }
}
```

The method body would not only be much longer, but it would have more than one responsibility. Breaking the methods out should make finding and updating their implementations easier down the road.

A> Notes: This is really a form of the Single-Responsibility Principle at the method level, or a simplified strategy pattern at the method level -- scales much better over time if we really do have new "Cancel" implementations.
