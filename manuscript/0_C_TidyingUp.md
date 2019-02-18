## Tidying Up

Keeping a codebase with well-intentioned names is often just about _remembering_ to do so. Everytime we make a change to a codebase, we have to consider names over again. 

Awhile back, I had a method that updated various pieces of account data:

```
public void UpdateAccountInfo(string account_name, int account_owner_id, byte[] logo_image);
```
As you can probably tell by the signature, this method lets you change the name of the account, the owner of the account, and the logo tied to the account.

At some point, it became advantageous to handle the uploading of the logo somewhere else. As part of this update, I pulled the `logo_image` parameter out of this method.

```
public void UpdateAccountInfo(string account_name, int account_owner_id);
```
At some point later, we also decided that an account could have multiple owners. Since the change was fairly large, we decided it was best to manage owners in an entirely separate part of the application. Part of this update naturally required removing the `account_owner_id` from this method.

```
public void UpdateAccountInfo(string account_name);
```
In the flurry of coding changes, it's easy to leave the `UpdateAccountInfo` method named as is. But, stripped of most of its original responsibilities, this method is much better named `UpdateAccountName`. That's all it's doing anymore. It also doesn't hurt to shorten the parameter `account_name` to just `name`, since it's obvious at this point what the parameter refers to.

```
public void UpdateAccountName(string name);
```
This example may seem obvious, but it's only because I've isolated the discussion of what's changed to just this lone method. When we're actually coding, this is just one of perhaps dozens of areas of code we're juggling simultaneously as we augment a feature. There won't be a failed unit test or compiler warning telling us that a method name might need adjustment.

When we miss renaming opportunities like this over and over again, our code starts to unravel. When we come back to a codebase littered with loose names, it becomes more difficult to know what things really mean. 

A developer down the road, working on an update, might not easily spot where a piece of functionality lives simply because its obfuscated behind a poor name. Codebases in this state are far more susceptible to having repeated concepts, beginning the inevitable slide toward unmaintainable code. 

So, remember to look for opportunties to tighten a construct's name everytime something changes about that construct.




