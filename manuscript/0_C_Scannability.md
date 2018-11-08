
## Scannability

In my previous example, we focused on pruning method names down to only what's essential. While this technique _usually_ improves code clarity, we need to consider the updated method names in the context of the other methods around it. Here’s an example.

I’m analyzing method names in a `ProjectsService` class and spot this one below. I have a hunch I could come up with a more concise name.

```C#
GetAvailableProjectsForUser(int userID) {…}
```

My first instinct is to remove the word `Available`. I doubt the class exposes a method that grabs projects unavailable to the user, but for sanity’s sake, I check the rest of the class. While I’m right about that, I do see another variant:

```C#
GetAdminableProjectsForUser(int userID) {…}
```

`Adminable` is a necessary adjective -- it's a subset of all the projects a user can access. This makes me reconsider the impact of removing the word `Available` from the original method.

No one would think that `GetProjectsForUser()` returns projects a user can’t access. So, `Available` truly adds nothing to our understanding of the name. But, there's another reason I'm in favor of removing it. `Available` and `Adminable` look nearly identical at a glance:

```C#
GetAvailableProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

We shouldn’t take this detail lightly. When reading code, _scannability_ is just as critical as readability -- we spend as much time dancing our eyes around a class scanning for patterns as we do reading the actually words. The more familiar we become with the codebase, the more we scan. Naturally, it would be an easy trap to misuse these methods down the road.

After considering the tradeoffs, I remove `Available`. I look at the two methods next to each other again. It’s an improvement.

```C#
GetProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

Next, I question whether `ForUser` is a worthwhile suffix on both methods. Since the only parameter passed into these methods is labeled `userID`, it seems superfluous to add this to the method name. We could we get by without it:

```C#
GetProjects(int userID) {…}
GetAdminableProjects(int userID) {…}
```

This is even more succinct. But, let’s discuss what’s lost by removing the suffix.

First, I spot a few other methods with a similar "for-something" pattern, like `GetProjectsForAccount()` and `GetArchivedProjectsForAccount()`.  It seems arbitrary that I don’t have the same suffix pattern for the methods I’ve just edited.

I also consider how it feels to pull up a list of these methods in my IDE while I’m coding. Is it more readable to see a list of methods like this...

```C#
GetProjects(int userID)
GetAdminableProjects(int userID)
GetProjectsForAccount(int accountID)
GetArchivedProjectsForAccount(int accountID)
```

...or like this?

```C#
GetProjectsForUser(int userID)
GetAdminableProjectsForUser(int userID)
GetProjectsForAccount(int accountID)
GetArchivedProjectsForAccount(int accountID)
```

I lean toward the latter because I find the scannability of these method names more valuable than brevity. Also, while the names are more wordy, the words help me pinpoint the genesis of each method more quickly. I don't have to deduce the intent of the methods from its parameters.

All told, I’ve deliberated intensely but make just one naming edit. `GetAvailableProjectsForUser()` is now `GetProjectsForUser()`. I stop short of pruning down even further because it doesn't improve the clarity of these methods, particularly in the context of similar methods around it. 

Treating words like currency doesn’t mean to be as stingy as possible, it also means making the right investments. 
