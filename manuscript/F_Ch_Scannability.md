
# Scannability

**Words are currency**

[Use this bit to segue into scannability -- that scanning is aided by making each word also count, so you aren't adding anything extra that is unnecessary. It means keywords stand out mre -- "GetAll..." cognitively means something. Or, if "scan" isn't perfect, maybe leave as is and change title to scannability & digestibility or scannability & readability?]

In school, writing assignments always had some sort of minimum length requirement. Five paragraphs. 2000 words. Ten pages. 

I understood why there was a requirement -- it gave us a general guideline for how much we should write. But, that requirement became the thing all of us gravitated toward. “Can I get to ten pages if I increase the font size by one pixel?” was more important than “Does that additional paragraph really strengthen my argument?”.

I wonder what would happen if teachers gave these limits in reverse -- no more than five paragraphs, 2000 words, or ten pages. Placing a maximum constraint would focus the writer on what they're writing about, all the while mitigating unnecessarily long sentences and double spacing.

Editing, in school, usually meant bloating your words. It wasn’t until I left school that I discovered that the true value of editing is reducing words. 

When I edit, I almost always end up with something _shorter_ than what I started with. I don't aim for a word count; I aim to make each word count. Prune what's not essential.

Luckily, we don’t stigmatize concise code. Code brevity has always been seen as a good thing in our industry. No sane programmer would tinker with a small bit of logic just to get it to be more wordy.

If you can, spend an hour just editing down names. Nothing more. Examine every method, class, variable, and property you maintain and figure out where you can take away words without losing meaning.

A property like `InitialCreationDate` doesn’t say anything more useful than `CreationDate`. It’s more verbose and, if anything, makes me question how there would be another date that something could’ve been created after the first time. (OK, if we’re going there, then we ought to add a new property like an `InitialResurrectionDate`).

A method in a `Customer` class like `GetAllSubscriptions()` is appropriately named if there are other related methods like `GetArchivedSubscriptions()` or `GetExpiredSubscriptions()`.  If not, `GetSubscriptions()` is sufficient. It says all it needs to say without falsely hinting that there are other related methods to consider.

When we conjure up names, adjectives like _all_ or _initial_ seem helpful at first. But, unless they provide additional meaning that the other words don’t, remove them. Each word should be there for a reason. The less words we need to describe something, the more value those descriptors will add. 

Treat words like currency.

---
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

With methods, we'll always face these types of scannability problems because all programming methods perform the same handful of actions -- the simplest ones get, create, update, delete. The more complex ones perform some combination of these actions. There are certainly more detailed ways to label a method's actions (I get into this in a later chapter). But, we'll always have to weigh the trade-offs that the symmetry of these ubiquitous verbs provides.

With general domain language naming, on the other hand, we have a whole lot more freedom with our naming choices. 

In the original version of DoneDone, we had a single business concept called a _project_. A _project_ held a set of _issues_. In the second version, we have two concepts. A _project_ now holds _tasks_. This is essentially a carryover from the original version. The second concept is a _mailbox_. A mailbox specifically serves as a container for incoming emails. After much deliberation, we agreed that we'd call each individual email a _conversation_.

Naturally, I took to these names when writing the code to support these business concepts. But, as I began enveloping these names into my code, I noticed the name `conversation` feeling cumbersome. It's somewhat lengthy, especially when compared to the corresponding concept in a project, the delightfully short _task_. And, given how integral the name was to the app, I knew it would infest itself all over the codebase. I'd be typing and reading this word for days.

`Conversation`. `ConversationController`. `GetConversationDetail()`. `UpdateConversation()`. `ConversationTypes`. `/Views/Conversations/Detail`.

I went hunting for a different word. I didn't want to change the word entirely (i.e. calling it a _message_ or _chat_), as we had already settled and liked how the word _conversation_ sounded in, conversation. It was the right word to use to talk about the concept, and coming up with something entirely different in code would've placed the constant burden of translating a business concept into a coding concept.

Instead, I simply shortened the word in code to `Convo`. At first, it felt too informal ("I'm just gonna build a little convo controller here") until I realized that there is no rule, nor should there be, for formalities in programming names. It might not be how I would talk about the concept to the British parliament, but until they have a hand in the codebase, it also doesn't matter.

A convo is no more ambiguous a word than a conversation. The word convo is delightfully short and has a similar directness with its counterpart, task. It also helps keep every mention of the idea shorter in code, meaning our eyes scan less from left-to-right.

```C#
public Conversation GetConversationForDetail(int conversation_id);
...
public Convo GetConvoForDetail(int convo_id);
```

I also appreciate the unique aesthetics of the name. There are few, if any words, that end with _onvo_. There are a multitude that end with _ation_ or _tion_. In fact, a quick lookup in my current DoneDone codebase reveals the following matches:

_Cre**ation**_, _Integr**ation**_, _Registr**ation**_, _Applic**ation**_, _Notific**ation**_, _Authentic**ation**_, _Ac**tion**_, _Excep**tion**_, _Projec**tion**_

When scoping out names to describe your domain language, it's important to see how those names manifest in the actual code. For longer names, or names that _look_ a lot like other names, is there a different way to shape that name, without losing its meaning? If so, it might be worth the swap.


