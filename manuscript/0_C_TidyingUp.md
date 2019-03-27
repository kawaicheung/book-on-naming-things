# Remembering to tidy up

In the original version of DoneDone, all forms of text a user can input is saved and stored in Markdown format, from the application interface all the way down to the database. The controller method parameter that represented this text was named `body`. The service methods that handled the updates and inserts also named its text parameters `body`. Down into my repository layer, where the text ships off into the database, I call this text parameter `body`.

Because all text was submitted, stored, and returned as Markdown, there wasn't a need to note that anywhere in a name. This is how we started things in the next version of DoneDone (which was an iterative refactoring of the original version's codebase). So, the various references to `body` remained.

About two-thirds of the way through developing the new version, I realized that storing everything as Markdown was becoming a crutch. We had introduced a WYSIWYG editor which would replace the Markdown editor we had been using. Initially, I thought the best approach to implementing the new editor was to force the editor to convert HTML into Markdown before submitting it into the codebase. This way, I wouldn't have to make wholesale changes to existing code. But, a _reliable_ HTML-to-Markdown converter is, I'd come to find, a veritable oxymoron.

Eventually, I came to terms with the fact that the easier approach was to have my backend code accept HTML instead of Markdown. But, this would only be true for certain text -- namely the ones that originate from a WYSIWYG editor in the application. There were other bits of text (most of which is generated behind the scenes), that still were being stored and served as Markdown. Further complicatin things, there were a few places in my code where the new HTML text still had to be converted to Markdown. Specifically, to send Slack notifications, I had to convert text originating from the editor to Markdown because that's how the Slack endpoint was still expecting its text.

However, the fact that some text was stored as HTML and other text was stored as Markdown wasn't a big deal. Keeping track of whether a particular method parameter or a particular class property would be in HTML or Markdown was. This was an issue up and down all the layers of my code.

One option to resolve this would be a pretty deep cut. I could create two new classes that subclass `String`, say a `MarkdownString` and `HTMLString` class, then, manually replace the myriad of string declarations in my code that needed this further specification. This would let me lean on the compiler to catch any issues--for instance, if an `HTMLString` were passed to my Slack API wrapper class. But, this felt like a sledgehammer approach to the whole problem. 

I opted for a much lighterweight approach--simply rename the parameters and properties _themselves_. Where a `body` method parameter existed, I replaced with `html_body` or `markdown_body`. Where a `text` class property lived, I replaced with `html_text` or `markdown_text`.

The best approach I've found is to rename the top and bottommost layers first. That's because those are the places where I know, with 100% confidence, what format the text should be. 

I started with the very top--at the interface level. A form element for a WYSIWYG editor would be renamed to `html_body`. (By necessity, I'd then rename its corresponding controller method parameter `html_body` since the parameters bind together by default on a name match).

Once I had all the top-level names updated, I then moved to the very bottom of the app, the layers that integrate with our own datastore or an external API. For instance, I have an object I pass to my Slack API wrapper which holds the body text that's displayed on a Slack message. I renamed that from `body` to `markdown_body`, because I know that's how Slack expects the data.

At this point, the code would compile because I'd done nothing more than rename properties. But, the work was _far_ from done. There was all the naming to do in between.

For example, even though a database repository method's parameter was renamed....

```C#
UpdateComment(int id, string html_body)
```
...calls to this method from higher level layers weren't touched yet.

```C#
public UpdateComment(int comment_id, string body, Requester user)
{
   // Validate user has access to this comment...
   _permissions.ConfirmUserCanUpdateComment(comment_id, user);
   
   _database.UpdateComment(comment_id, body);
}
```

Updating all the [tedious trivial]




I'd trickle my way inward through the layers of abstraction. (By the way, it's at a time like this where a `Rename...` feature on your development environment is most useful. I always use the built-in feature in Visual Studio for this rather than renaming a property and all of its references manually).



At first, this felt like cheating. It felt too...easy. And, I'm accustomed to nothing in programming being as easy as things first appear.

But, the more I went down this path, the more fluid the code felt. 






Power of just renaming params without structurally changing anything.

But, we need to be vigilant that the param names are fully cleaned up everywhere.

It's certainly a kind of refactoring. 

"Each discovery should be pushed back into all previous code"

Show URL / Response / Page example.

"Learning that a name is imprecise is knowledge, and that knowledge should be available everywhere".

Markdown HTML/Body.

Three options:

1) Do nothing
2) Create new classes that use a String as a subclass.
3) Lightweight: Rename the params for clarity.

It was a great way to make sure everything connected to go up and down the chains of code to make sure various bodies were transformed correctly.



====


> _One of the biggest sins you can commit is to stop programming when it works._

Keeping a codebase with well-intentioned names is often just about _remembering_ to do so. Everytime we make a change to a codebase, we have to consider the names of things over again. It's easy to leave working code as-is, without considering the debt you've just handed over the next reader. [Change to more about Brandon's idea of pushing back the information everywhere]

Awhile back, I had a method that updated various pieces of account data:

```
public void UpdateAccountInfo(string account_name, int account_owner_id, byte[] logo_image);
```
As you can probably tell by the signature, this method lets you change the name of the account, the owner of the account, and the logo tied to the account.

At some point, it became advantageous to handle the uploading of the logo somewhere else. As part of this update, I pulled the `logo_image` parameter out of this method.

```
public void UpdateAccountInfo(string account_name, int account_owner_id);
```
Down the road, we also decided that an account could have multiple owners. Since the change was fairly large, we decided it was best to manage owners in an entirely separate part of the application. Part of this update naturally required removing the `account_owner_id` from this method.

```
public void UpdateAccountInfo(string account_name);
```
In the flurry of updating code, it's easy to leave the `UpdateAccountInfo` method named as is. But, stripped of most of its original responsibilities, this method is much better named `UpdateAccountName`. That's all it's doing anymore. It also doesn't hurt to shorten the parameter `account_name` to just `name`, since it's obvious at this point what the parameter refers to.

```
public void UpdateAccountName(string name);
```
This change sounds obvious to make, but it's only because I've isolated the discussion of what changed to just this lone method--not the other myriad of method additions, refactorings, and adjustments that come as a natural part of every kind of software change.

When you’ve moved code around your application to get the pieces fitting just right, revisit how you’ve named the methods, properties, and classes that have undergone the facelift. Do these names still make sense? Do the comments around these methods still apply?

--- Here's another example -- markdown/html body--

When you’re in the same code daily, you might not even notice that the name of a variable or method is misleading because you’re so familiar with it. But, to someone coming into the codebase fresh (or, if you happen to take a few weeks off and come back later), misleading names will be detrimental to their understanding of the system.

There won't be a failed unit test or compiler warning telling us that a construct's name is no longer relevant. That's why they're so often left unchanged. So, remember to look for opportunties to tighten a construct's name everytime something changes about that construct. Nothing else will automatically remind you to do so.
