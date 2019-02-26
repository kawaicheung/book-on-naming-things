# Corralling bits of logic into names

Making your codebase clearer isn't just about naming things better--it's also about finding things to name that weren't being named before. 

A common place you can find these nameless things is anywhere you see small bits of business logic using the properties of the same object in a place _other_ than within the object itself. There's usually an easy way to name that logic and push it back into the object.

Suppose you have a `Person` class that houses some basic information used throughout your codebase.

```C#
public class Person
{
    public string FirstName;
    public string LastName;
    public DateTime LastLoggedIn;
    public AccountRoleType Role;
    
    ...
}
```
Instances of this class spring up all over--appearing within other objects, as return objects from various methods, or surfacing in the application's view layer. 

For example, on a person's profile page, you might use this object to display a person's name and show a few links to other sections of the application they have access to.

```HTML
<div>
  <h2>@Model.LoggedInPerson.FirstName @Model.LoggedInPerson.LastName</h2>
  @if (Model.LoggedInPerson.Role == AccountRoleType.ADMIN || 
       Model.LoggedInPerson.Role == AccountRoleType.OWNER)
  {
      <a href="...">Edit</a> | <a href="...">Cancel</a>
  }
</div>
```

In another part of the application, you might use the person's first and last name to prep notification messages when they update their profile.

```C#
// e.g. "Mary D. updated their profile"
var subject = person.FirstName + " " + person.LastName.Substring(0,1) + "." + " updated their profile.";
```

In a security class, you might check if the person hasn't logged into the application in a certain amount of time and require them to re-login.

```C#
if ((person.LastLoggedIn - DateTime.Now).TotalDays > 1)
{
    // Sign out and require this person to login again.
}
```

Throughout the application, whereever the `Person` object is used, little bits of business logic against its properties are sprinkled about. Most of these bits of logic are so minor--simple, one-line constructions and statements--you might not even consider them to be real business logic at all.

I do this myself all the time. When I need to write code against an object's properties, I do it in the place I currently need that logic rather than push the logic back into the object. But, doing so is a missed opportunity to clarify what these manipulations mean. 

For instance, in our view example, displaying a person's first and last name might not seem like _logic_, but it is--it represents a person's _full name_. You can easily push this bit of logic back to the `Person` class itself. This also gives you the opportunity to name it something meaningful. 

```C#
public string FullName
{
  get
  {
    return FirstName + " " + LastName;
  }
}  
```

Likewise, the same can be done for the person's name in the subject line of the notification message:

```C#
public string AbbreviatedName
{
  get
  {
    return FirstName + " " + LastName.Substring(0,1) + ".";
  }
}
```

Similarly, you can push the check for whether this person is an admin or owner as a property of the object itself, and give it a meaningful name. In this case, the check is answering the question, "Does this person have administrative access?"

```C#
public bool HasAdminAccess
{
  get
  {
    return Role == AccountRoleType.ADMIN || Role == AccountRoleType.OWNER;
  }
}   
```
The logic against the `Person` object within the security class can be pushed back to the class in a couple of ways. Let's take a closer look at the conditional statement again:

```C#
if ((person.LastLoggedIn - DateTime.Now).TotalDays > 1)...
```

You could take the entire statement and turn it into a boolean property off the `Person` like so:

```C#
public bool IsLastLoginWithinOneDay
{
  get
  {
    return person.LastLoggedIn - DateTime.Now).TotalDays > 1;
  }
}  
```

Or, you could judiciously push just the calculation of the total days into the object:

```C#
public int DaysSinceLastLogin
{
  get
  {
    return person.LastLoggedIn - DateTime.Now).TotalDays;
  }
}  
```

I prefer the latter. The former seems a bit too specific to push into the `Person` class. The latter feels like the right amount of responsibility for the `Person` class to own.

With these updates, our `Person` class now evolves to something a lot more powerful:

```C#
public class Person
{
    public string FirstName;
    public string LastName;
    public DateTime LastLoggedIn;
    public AccountRoleType Role;
    
    public string FullName
    {
      get
      {
        return FirstName + " " + LastName;
      }
    }  
    
    public string AbbreviatedName
    {
      get
      {
        return FirstName + " " + LastName.Substring(0,1) + ".";
      }
    } 
    
    public bool HasAdminAccess
    {
      get
      {
        return Role == AccountRoleType.ADMIN || 
               Role == AccountRoleType.OWNER;
      }
    } 
    
    public int DaysSinceLastLogin
    {
      get
      {
        return (person.LastLoggedIn - DateTime.Now).TotalDays;
      }
    } 
    ...
}
```

By moving this logic into the `Person` class, it's now easier to DRY up your codebase. There will likely be other places that require displaying a person's full name or knowing whether they have administrative privileges. Those answers are already baked into the object itself.

And besides reuse, the biggest gain comes from the improved readability of your code. Here's how the improved implementations look like:

```
<div>
  <h2>@Model.LoggedInPerson.FullName</h2>
  @if (Model.LoggedInPerson.HasAdminAccess)
  {
      <a href="...">Edit</a> | <a href="...">Cancel</a>
  }
</div>
```

First, in our HTML markup, the business logic below competes far less with the HTML around it.

```C#
var subject = person.AbbreviatedName + " added a comment";
```

The subject of the email notification can also be interpreted with one glance. You don't spend time focusing on the details of how the person's name is being displayed anymore.

```C#
if (person.DaysSinceLastLogin > 1)
{
    // Sign out and require this person to login again.
}
```

Finally, the conditional check on the person's login date can be understood instantly, instead of having to parse (even if for a brief moment) through the date logic.

Keep hunting for places where you can corral bits of logic back into the objects they're derived from. It will do wonders to the clarity of your code.
