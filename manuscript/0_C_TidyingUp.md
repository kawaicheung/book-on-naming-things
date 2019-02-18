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
At some point later, we also decided that an account could have multiple owners. We decided it was best to manage owners in an entirely separate part of the application. Part of this update naturally required removing the `account_owner_id` from this method. 

```
public void UpdateAccountInfo(string account_name);
```


* Often see code left in this state and wonder why...not realizing the history behind it.

```
public void UpdateAccountName(string name);
```

* Re-tidy up so it's clear...opportunity to be more concise with the params as well.


