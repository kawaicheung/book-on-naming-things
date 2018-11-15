## Sprinkling Humor

As programmers, we champion convention. Conventions provide the guardrails for our code to stay on a consistent path. 
We apply this same general practice to naming. In this way, when we're writing code, we can limit our options with names -- otherwise we might belabor over them for far too long as the deadline looms.

But, at the right times, break the rigidity of those naming conventions. You can make a variable more memorable. You can make the entire function more memorable as well.

I have some code that controls who should receive an email notification when an issue is updated in an issue tracking system. The code collects user IDs and stores them into separated arrays named by the reason they should receive an email.

The code first finds all the people working on the issue and stores them in an array called `ids_working_on_issue`.
Next, it finds all the people who requested to always be notified on the issue and stores them in an array called `ids_requesting_emails_on_issue`.

Finally, it finds all the people that requested to be notified on everything across the entire project.

For this final list, I could choose something in-step with the rest of the names, like `user_ids_requesting_notification_on_all_issues`.

The name works. After all, it follows a nice naming convention with its fellow arrays. But, imagine someone stepping into this part of the code one day and seeing these various arrays sprinkled about a method:

```
int[] ids_working_on_issue...
int[] ids_requesting_emails_on_issue...
int[] ids_requesting_emails_on_all_issues...
```

What if I could find a name equally as informative, but a bit more memorable?

I end up going with `email_lover_ids`. It’s a name you can’t forget. First, there’s a tinge of sarcasm -- who loves email anyways? (Apparently, these folks do). 

Second, it’s much shorter. It conveys the meaning I want with much fewer letters. What else can an array of email_lover_ids mean other than a collection of user identifiers who clearly want to be emailed all the time, on everything?

I also relish that the name actually diverges from the pattern of the other array names. 

```
int[] ids_working_on_issue...
int[] ids_requesting_notification_on_issue...
int[] email_lover_ids...
```

Sometimes, I actually do want this irregularity -- it helps break up the monotony of a group of similarly named things all working together for the same cause. In that way, it helps me remember the overall logic that much better (“It cycles through all those other arrays, and then finally ends on those email lovers…”).
