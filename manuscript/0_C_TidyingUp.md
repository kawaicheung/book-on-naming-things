## Tidying Up

Every Friday during grade school, my teacher would set aside about fifteen minutes toward the end of the afternoon to clean the insides of our desks. I remember relishing this cleanup time -- not only because I knew it signaled the beginning of the weekend, but because I actually enjoyed re-organizing my desk.

When I finished cleaning up, it was always the same result; Notebooks and binders neatly tucked to the left, pencils, pens and other instruments organized in my school box and shoved to the right, and other miscellaney thrown in the trash. I took pride in a clean desk. I'd tell myself that next week, I'd try harder to *keep* my desk clean throughout the week.

Fast-forward to the next Friday afternoon and the same mess would require the same amount of cleanup.

It frustrated me that my desk would get messy again. I realized that as the week progressed, I'd find myself less and less vigilant with keeping things organized amidst all the other activity happening outside of my desk. Being messy, I learned, was just part of the human condition.

This is also why codebases also unravel over time. In the beginning, we pledge to be vigilant with everything in our code. Over time, we loosen our reigns as the stress to just keep the engine running weighs more heavily.

It's really important to get in the habit of tidying up often. Done as a ritual at the end of any update, it just becomes part of the flow of writing code. There are many ways of tidying up, but I want to focus (naturally) on tidying up names.

```
public void UpdateAccountInfo(string account_name, int account_owner_id, byte[] logo);
```

```
public void UpdateAccountInfo(string account_name, int account_owner_id);
```

```
public void UpdateAccountInfo(string account_name);
```
* Often see code left in this state and wonder why...not realizing the history behind it.

```
public void UpdateAccountName(string name);
```

* Re-tidy up so it's clear...opportunity to be more concise with the params as well.


