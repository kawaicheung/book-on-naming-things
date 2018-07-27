## Flow

When we name a variable, we need to look at how that variable is actually being used. Sometimes, a variable name in isolation feels right, but when it's used, a key piece of information is missing. Here's a simple example.

I have some code inside a larger method that dictates whether someone should receive an email notification.

```C#

if (user.IsOwner ...)
{
	sendNotification(user);
}

```