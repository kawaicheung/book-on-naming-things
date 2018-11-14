
## Scannability

When reading code, _scannability_ is just as critical as readability. Our eyes spend as much time dancing around code scanning for patterns as they do reading the actual words. The more familiar we are with a codebase, the more we scan. 

I’m analyzing method names in a `ProjectService` class and spot these two below. I have a hunch I could improve their names.

```C#
GetAllAvailableProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

My first instinct is to remove the word `All` from the first method. Trimming the method name to `GetAvailableProjectsForUser()` doesn't alter my understanding of what the method does. So, I make the quick edit and re-evaluate.

```C#
GetAvailableProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

The method names now have a more natural pattern to them. The programmer in me likes this -- it feels _elegant_. But, in practice, these methods are now much harder to scan. At a quick glance, the two methods look nearly identical. It would be an easy trap to misuse these methods down the road. 

`Adminable` describes a subset of all the projects a user can access -- it's a necessary adjective. Take it away and the method name has lost most of its value. But, fortunately, `Available` isn't. No one would think that `GetProjectsForUser()` returns a project a user couldn’t access. 

I remove `Available` as well and look at the two methods next to each other again. 

```C#
GetProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

While I lose some sense of elegance, I've improved the scannability of these methods. That's a trade-off I'll take every time.

Feeling good about my luck with shortenting names, I now consider whether the `ForUser` suffix is need on both methods. Since the only parameter passed into these methods is labeled `userID`, it seems superfluous to add this to the method name. Without them, the method signatures still read pretty clearly:

```C#
GetProjects(int userID) {…}
GetAdminableProjects(int userID) {…}
```
But, let’s discuss what’s lost by removing the suffix.

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
