## Opposites don't always attract

Much of programming objects is about states. A user might be either active or inactive; A purchase order is either pending or confirmed and so forth.

Luckily, the English language is broad enough that most states have a meaningful opposite. For instance, if we have a piece of functionality that allows us to move a file from the trash, we don't have to say _undelete_. _Restore_ makes perfect sense. It's clear that a file that can be restored is already in the state of being deleted.

Unfortunately, we don't always have the luxury of a convenient opposite word. Take this example.

In DoneDone, we've created a new concept called _Workflows_. A workflow defines a series of statuses that an issue can be in. We let users create workflows to tailor them to their own business processes. When a user is ready to use the workflow, it has to be _published_.

A user can also -- for lack of a better verb -- _unpublish_ a workflow. The requirements for a workflow to be "unpublishable" are more than just that it's already been published; There are a few other caveats like being associated to an active project. So, I decied to wrap this logic inside of a convenient little method. My first attempt at a method name is:

```
public bool IsWorkflowUnpublishable();
```

However, there's something really unsavory about this name. I could easily mistake this as a check that a workflow can't be published rather than a check that a workflow can be unpublished -- two very different things.


