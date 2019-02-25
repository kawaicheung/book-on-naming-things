# Corralling bits of logic into names

Making your codebase clearer isn't just about naming things better. It's also about finding things to name where they weren't being named before. You can hunt for these nameless things anytime you see small bits of logic strewn about code. 

Suppose you have a `Person` class that houses some basic information.

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
Instances of this class spring up all over the codebase--appearing within other objects, as return objects from various methods, or surfacing in the application's view layer. 

For instance, on a person's profile page, you might use this object to display a person's name and show a few links to other sections of the application they have access to.

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

In another part of the application, you might use the person's first and last name to prep notification messages.

```C#
// Display from name as [first name] [last initial]
var fromName = person.FirstName + " " + person.LastName.Substring(0,1) + ".";
return fromName + " added a comment";
```

Somewhere else, you might check if the person hasn't logged into the application in a certain amount of time and require them to re-login.

```C#
if ((person.LastLoggedIn - DateTime.Now).TotalDays > 1)
{
    // Sign out and require this person to login again.
}
```

Throughout the application, whereever the `Person` object is used, little bits of business logic against its properties are sprinkled about. Most of these bits of logic are so minor--simple, one-line constructions and statements--you might not even consider them to be real business logic at all.

I see this a lot in code I've read, and, it's something I do quite often. When I need to perform some small manipulation on an object's properties, I do them in the place I currently need them rather than push them back into the object. But, doing so is a missed opportunity to clarify what these manipulations mean. 

For instance, in our view example, displaying a person's first and last name might not seem like _logic_, but it is--it represents a person's _full name_. You can easily push this bit of logic back to the `Person` class itself. This also gives you the opportunity to name it.

```C#
public string FullName
{
  get
  {
    return FirstName + " " + LastName;
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

We can do the same for the other _bits of logic_ in this example. Our `Person` class now evolves to something a lot more powerful:

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

But, the biggest gain comes from the improved readability of your code. Look at how much tighter and fluid the updated implementations feels now:

```
<div>
  <h2>@Model.LoggedInPerson.FullName</h2>
  @if (Model.LoggedInPerson.HasAdminAccess)
  {
      <a href="...">Edit</a> | <a href="...">Cancel</a>
  }
</div>
```

```C#
return person.AbbreviatedName + " added a comment";
```

```C#
if (person.DaysSinceLastLogin > 1)
{
    // Sign out and require this person to login again.
}
```



