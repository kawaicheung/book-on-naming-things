# Naming the small stuff

If you develop applications today, you probably use some flavor of a model-view-controller (MVC) framework. The gist of any deriviative of this framework is to separate data from business logic and presentation. 

I often see examples where these hard lines are blurred. Logic creeps into the presentation layer. I have the habit of doing this myself. Take this simple case. (Change this part above to be more generic -- doesn't really have to be MVC).

I have a `User` class that stores a `Person` class that holds a person's first name, last name, and their account role.

[Add: LastLoginDate]
[Add: Admin or Owner == Admin Access]
[Add: Profile address]

```C#
public class Person
{
    public readonly string FirstName;
    public readonly string LastName;
    public readonly AccountRoleType Role;

    public Person(string firstName, string lastName, AccountRoleType role)
    {
        FirstName = firstName;
        LastName = lastName;
        Role = role;
    }
}
```
Instances of this class spring up all over the codebase--appearing within other data models, as return objects from various methods, and surfacing in the application's view layer. 

For instance, on a person's profile page, I use this object to display their name and show a few links to other sections of the application they have access to.

```
<div>
  <h2>@Model.LoggedInPerson.FirstName @Model.LoggedInPerson.LastName</h2>
  @if (Model.LoggedInPerson.Role == AccountRoleType.ADMIN)
  {
      <a href="">Edit</a> | <a href="">Cancel</a>
  }
</div>
```

In another part of the application, I use the person object to prep notification messages. But, in this case, I just want to return the person's first name and last initial.

```C#
// Display from name as [first name] [last initial]
var fromName = person.FirstName + " " + person.LastName.Substring(0,1) + ".";
return fromName + " added a comment";
```

Throughout the application, whereever the `Person` object is used, all sorts of little bits of logic against its properties are sprinkled about. Because these bits of logic are so tiny, you might not even consider them to be _bits of logic_ -- certainly not enough to take another pass at where they belong. But, whenever there are bits of logic against an object's properties strewn about, it's a smell that they can be pushed back into the object--_and_ we can name them. 

In our view example, displaying a person's first and last name might not seem like _logic_, but it is--it represents a person's _full name_. You can push this bit of logic back to the `Person` class itself. This also gives you the opportunity to name it.
```C#
public string FullName
{
  get
  {
    return FirstName + " " + LastName;
  }
}  
```
We can do the same for the other _bits of logic_ we've written throughout the app. 



