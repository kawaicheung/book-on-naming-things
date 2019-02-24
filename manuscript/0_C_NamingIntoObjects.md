# Pushing (and naming) logic into objects

If you develop applications today, you probably use some flavor of a model-view-controller (MVC) framework. The gist of any deriviative of this framework is to separate data from business logic and presentation. 

I often see examples where these hard lines are blurred--logic creeps into the presentation layer. I have the habit of doing this myself. Take this simple case.

I have a `User` class that stores a `Person` class that holds a person's first and last name as well as their account role.

```C#
public class Person
{
    public readonly string FirstName;
    public readonly string LastName;
    public readonly AccountRole Role;

    public Person(string firstName, string lastName, AccountRole role)
    {
        FirstName = firstName;
        LastName = lastName;
        Role = role;
    }
}
```
Instances of this class spring up all over the codebase, appearing within other data models, as return objects from various methods, and surfacing in the application's view layer. For instance, on a person's profile page, I might use this object to display their full name and show a few links to other sections of the application they have access to.

```
<div>
  <h2>@Model.LoggedInPerson.FirstName @Model.LoggedInPerson.LastName</h2>
  <@
</div>
```

In anoth
