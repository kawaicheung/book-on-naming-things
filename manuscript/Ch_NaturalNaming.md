## Alleviating awkwardness

I'm working on a feature addition to how someone can cancel their account in DoneDone. Up until now, if you wanted to cancel the account, it's immediate.

However, now we want to let someone wait until the end of their current billing period to cancel. That way, they don't have to set a reminder to cancel just before the new billing period and risk being charged again but still can use the app until then.

There are now two cancellation types. We can cancel immediately or cancel at the end of a period. I start by tacking on a parameter to a `CancelAccount()` repository method to pass this extra important bit of information:

```C#

public void CancelAccount(int account_id);

```

```C#

public void CancelAccount(int account_id, bool cancel_at_period_end);

```

Since there are only two cancellation options, I choose the simplest parameter type -- a boolean. I could decide to use something more robust -- say, an `enum` -- but I stick with a boolean for now.

After I've implemented the new code to handle the update, I now update all existing references to this method that handle immediate cancellations.

```C#

_billing_repository.CancelAccount(account_id);

```

...updates to...

```C#

// Cancel the account immediately...
_billing_repository.CancelAccount(account_id, false);

```

I feel compelled to add the comment above each call to `CancelAccount()` for clarity.  It also helps differentiate between the new code I'll be adding to handle the additional option of canceling at the end of the period.

However, the _feel_ of the existing reference updates bothers me. Canceling an account immediately feels like it should be the _default_ case rather than the outlier. Passing in `false` for the default feels odd -- it's as if I have to suppress something to perform the default action.

I can get around this pretty quickly. Since I'm working with a boolean parameter, I can simply flip the name of the option around and update my code accordingly. I swap the `cancel_at_period_end` parameter, with `cancel_now`. And Voila!

```C#

public void CancelAccount(int account_id, bool cancel_now);

```

```C#

// Cancel the account immediately...
_billing_repository.CancelAccount(account_id, true);

```

The references now feel better -- the default case passes in `true`. But, I've introduced a more onerous problem. By simply reading the `CancelAccount()` method signature, I can't quite tell what passing in `false` would do. What would it mean to "not cancel the account now"? Would it cancel in a day? In a month? At the end of the period? 


-- enum option...

-- hard naming method

-- Basically a different version of the pattern of having 'strategies' instead of a series of cases -- but this is just a boolean so it could go either way on what feels better -- in this case the odd boolean naming )