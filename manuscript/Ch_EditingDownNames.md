## Being Terse

In school, writing assignments always had some sort of minimum length requirement. Five paragraphs. 2000 words. Ten pages. I understood why there was a requirement -- it gave us all a general guideline for how much we should write. But, that requirement became the thing that students gravitated toward. “Can I get to ten pages if I increase the font size by one pixel?” was more important than “Does that additional paragraph really strengthen my argument?”.

I wonder what would happen if teachers gave limits in reverse -- no more than this many paragraphs, words, or pages. Maybe placing a maximum constraint would’ve produced better results all the while mitigating unnecessarily long sentences, double spacing, playing with font sizes, and sparing the world a few more trees.

Editing, in school, usually meant fattening up paragraphs -- adding four examples instead of three, finding a longer word to replace a shorter one. It wasn’t until I left school and started writing on my own blog posts, books, and -- for better or worse -- tweets that I discovered the art of writing less words. 

When I edit my writing now, I almost always end up with something _shorter_ than what I started with. I aim for clarity, not 2000 words. I find editing down words the most enjoyable part of writing.

This has helped me become a better code editor too. Luckily, we don’t stigmatize concise code. As far as I’m aware, code brevity is and has always been seen as a good thing in our industry. No sane programmer would tinker with a small bit of logic just to get it to be more wordy. Code should be treated like currency -- use each word wisely.

If you can, spend an hour just editing down names. Nothing more. Examine every method, class, variable, and property you maintain and figure out where you can take away words.

A property like `InitialCreationDate` probably doesn’t say anything more useful than `CreationDate`. It’s more verbose and, if anything, makes me question how there would be another date that something could’ve been created after the first time. (OK, if we’re going there, then we ought to add a new property like an `InitialResurrectionDate`).

A method in a `Customer` class like `GetAllSubscriptions()` is appropriately named if there are other related methods like `GetArchivedSubscriptions()` or `GetExpiredSubscriptions()`.  Otherwise, `GetSubscriptions()` is sufficiently clear. It says all it needs to say without falsely hinting that there are other related methods to consider.

When we conjure up names, adjectives like _all_ or _initial_ seem helpful at first. But, unless they provide additional meaning that the other words don’t, remove them. Each word should be there for a reason. The less words we need to describe something, the more value those descriptors will add.

That said, edit down names with extreme care. Here’s an example.

I’m analyzing method names in a `ProjectsService` class and spot this method below. I have a hunch I could come up with a more concise name.

```C#
GetAvailableProjectsForAccountMembership(int accountMembershipID) {…}
```

My first instinct is to remove the word `Available`. I doubt the class exposes a method that grabs projects unavailable to the user, but for sanity’s sake, I check the rest of the class. While I’m right about that, I do see another variant:

```C#
GetAdminableProjectsForAccountMembership(int accountMembershipID) {…}
```

`Adminable` is a necessary adjective -- it lets me know a user has extra privileges with this collection of projects. This makes me reconsider removing `Available`.

No one would confuse `GetProjectsForAccountMembership` as a method that returns projects a user can’t access. `Available` adds nothing to our understanding of the name. In addition, `Available` and `Adminable` look nearly identical at a glance:

```C#
GetAvailableProjectsForAccountMembership(int accountMembershipID) {…}
GetAdminableProjectsForAccountMembership(int accountMembershipID) {…}
```

We shouldn’t take this detail lightly either. When reading code, scannability is just as important as readability -- we spend as much time dancing our eyes around a class scanning for patterns as we do reading the actually words. The more familiar we become with the codebase, the more we scan. Naturally, it would be an easy trap to misuse these methods down the road, relying solely on tests to save our skin.

After considering the tradeoffs, I remove `Available`. I look at the two methods next to each other again. It’s an improvement.

```C#
GetProjectsForAccountMembership(int accountMembershipID) {…}
GetAdminableProjectsForAccountMembership(int accountMembershipID) {…}
```

Next, I question whether `...ForAccountMembership` is a worthwhile suffix on both methods. Since the only parameter passed into these methods is labeled `accountMembershipID`, it seems superfluous to explain this in the method name. We could we get by without it:

```C#
GetProjects(int accountMembershipID) {…}
GetAdminableProjects(int accountMembershipID) {…}
```

This is certainly even tighter. There’s a thrill in reducing a name more than half its original size and still leaving the meaning intact. But, let’s discuss what’s lost by removing the suffix.

First, I spot a handful of other methods with a similar "for"-something pattern, like `GetProjectsForAccount()` and `GetAdminableProjectsForOverview()`.  It seems arbitrary that I don’t have the same suffix pattern for the methods I’ve just edited.

I also consider how it feels to pull up a list of these methods in my IDE while I’m coding. Is it more readable to see a list of methods like this...

```C#
GetAdminableProjects(int accountMembershipID)
GetAdminableProjectsForOverview(int accountMembershipID)
GetArchivedProjectsForAccount(int accountID)
GetProjects(int accountMembershipID)
```

...or like this?

```C#
GetAdminableProjectsForAccountMembership(int accountMembershipID)
GetAdminableProjectsForOverview(int accountMembershipID)
GetArchivedProjectsForAccount(int accountID)
GetProjectsForAccountMembership(int accountMembershipID)
```

I vote for the latter. While it's more wordy, the words help me pinpoint the reason for each method more quickly.  In fact, most IDEs I’ve worked with don’t show the parameter list, making it harder to understand the use of `GetAdminableProjects()` or `GetProjects()` at a glance.

All told, I’ve deliberated intensely but make just one naming edit. `GetAvailableProjectsForAccountMembership()` is now `GetProjectsForAccountMembership()`. I stop short of editing down more because doing more would add unwanted debt. Treating words like currency doesn’t mean to be as stingy as possible, it also means making the right investments. 
