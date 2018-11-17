
## Scannability

When reading code, _scannability_ is just as critical as readability. Our eyes spend as much time dancing around code scanning for patterns as they do reading the actual words. The more familiar we are with a codebase, the more we scan. 

While trimming words usually improves a name, the true litmus test is if the new name helps our ability to scan the code around it.

I’m analyzing method names in a `ProjectService` class and spot these two below. I have a hunch I can improve their names.

```C#
GetAllAvailableProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

My first instinct is to remove the word `All` from the first method. Trimming the method name to `GetAvailableProjectsForUser()` doesn't alter my understanding of what the method does. So, I make the quick edit and re-evaluate.

```C#
GetAvailableProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

The method names now have a natural uniformity to them. Coupled together, they feel more _elegant_. Elegance is a term we sometimes like to throw around when we think we've done something good but can't really prove why.

Despite the elegance, here's the problem staring me right in the face: In practice, these methods are now much harder to tell apart. When I scan these methods, they look nearly identical. It would be an easy trap to misread -- or worse -- misuse these methods. I need to differentiate them further.

`Adminable` describes a subset of all the projects a user can access -- it's a necessary adjective. Take it away and the method name has lost most of its value. But, fortunately, `Available` isn't necessary. No one would think that `GetProjectsForUser()` returns projects a user couldn’t access. 

I remove `Available` from the first method and look at the two methods next to each other again. 

```C#
GetProjectsForUser(int userID) {…}
GetAdminableProjectsForUser(int userID) {…}
```

While they've lost their uniformity, they're now easier to tell apart. That's a trade-off I'll take almost every time.

Feeling good about my luck with shortenting names, I now consider whether the `ForUser` suffix is needed on both methods. Since the only parameter passed into them is labeled `userID`, it seems superfluous to add this to the method name. Without them, the method signatures still read pretty clearly, and even more succinctly:

```C#
GetProjects(int userID) {…}
GetAdminableProjects(int userID) {…}
```

But, there's another problem. The class also contains two other methods with a similar "for-entity" pattern: `GetProjectsForAccount()` and `GetArchivedProjectsForAccount()`. I think about how it feels if these methods are pulled up in my development environment as code completion hints or in a method search. 

Removing the `ForUser` suffix makes the collection of methods even harder to read together because there's no naming consistency to latch onto.

```C#
GetAdminableProjects(int userID)
GetArchivedProjectsForAccount(int accountID)
GetProjects(int userID)
GetProjectsForAccount(int accountID)
```

I could solve this by removing the `ForAccount` suffix from the other methods as well, since their use is also implied by the parameter `accountID`. But, I've suddenly regressed to the same issue I had earlier: The methods look too similar and require more than just a quick scan to deduce their meaning, especially when I look at them together. I also run into a compiler error since I now have two identical method signatures.

```C#
GetAdminableProjects(int userID)
GetArchivedProjects(int accountID)
GetProjects(int userID)
GetProjects(int accountID) // The compiler will complain!
```

However, if I leave the `ForUser` suffix intact, the methods still follow a consistent pattern, but they aren't difficult to tell apart. Also, each word adds meaning to the method. `GetProjectsForUser()` is not only specific to an account, but implies there are other entities we can get projects for (like an account).

```C#
GetAdminableProjectsForUser(int userID)
GetArchivedProjectsForAccount(int accountID)
GetProjectsForAccount(int accountID)
GetProjectsForUser(int userID)
```

Overall, the final result balances brevity with meaning and keeps each of the names distinct _enough_ from each other. The name choices make the collective group of methods easier to scan.
