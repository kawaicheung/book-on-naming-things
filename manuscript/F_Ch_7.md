# Speaking the Native Tongue

**Pattern Hypnosis**

Unearthing patterns is one of the most fundamental mental exercises in coding. When we can trivialize a procedure that once required a lot of thought into something seemingly automatic, we can move onto solving other problems. 

On the other hand, automation can bias our thinking. We end up forcing a pre-conceived solution every time we encounter what feels like a similar problem without the careful examination we once gave it before we discovered a pattern. The reasoning becomes dogmatic -- it fits this pattern so this must be the way forward!

Let's call this phenomenon "pattern hypnosis." Even the task of naming can fall prey to this pattern hypnosis. I notice I fall into the trap quite frequently when naming tables in a relational database model.

If you're familiar with relational data modeling, you'll know one of the more common modeling concepts is the associative table. 

An associative table's core characteristic is that it contains two or more foreign keys to tables that have a "many-to-many" relationship, like a table that links a `tbl_Students` table to a `tbl_Classes` table. A student enrolls in many classes and a class has many students. The associative table would hold the relationship between those two tables alongside any other data relevant to that relationship (like a student's current grade in that class).

A common pattern is to name such an associative table by gluing the adjacent table names together. This approach usually creates a unique table name that's palatable. `tbl_StudentsClasses` feels like a reasonable name for this table. It reads decently.

Let's take another example. I have a table that links a `tbl_Users` table with a `tbl_Accounts` table.  A single user can belong to many accounts and an account can have many users. If I'm blindly naming an associative table that links these two tables together, I'd call it `tbl_UsersAccounts`. 

Down the road, I add other attributes to the model, like a date for when a user was added to the account. That column would fit naturally in the `tbl_UsersAccounts` table. But, saying "Nora's user account began on June 3rd, 2016" sounds obnoxiously technical. It sounds much more natural to say "Nora's membership began on June 3rd, 2016." 

So, I decide to update the table name to `tbl_Memberships`. 
 
Later on, I want to add a third association to the table -- a relationship to a set of available roles. This way, I can model a particular user with an account and a level within the account (Nora has Gold status). If I'm blindly following convention, I'd name my table to something like `tbl_UsersAccountRoles`.

Meanwhile, the name `tbl_Memberships` still holds up. "Nora has a gold-access membership to the Acme account." 

This kind of pattern hypnosis isn't relegated to just table naming. Object-relational mappers usually map directly to tables, so it's common to see the same name patterns appear in data transfer or business domain objects as well.  A `UserPublication` class might be better served as a `Subscription` class. `CustomerProduct` could be a `PurchaseOrder`. `PassengerFlight`? A `Booking`.

This isn't to say that the default pattern never works. Lots of times, it is the most sensible way to describe an association. But, in the end, to name things well, I sometimes have to fight the hypnosis of the easy route.

**Profession-speak**

People that write code today are often the same ones making business decisions or collaborating directly with clients. This makes it even more critical that programmers understand the business reasons behind their work. One of the best ways we can encourage this is to infuse the language of the business directly into our code.

If I were building internal software to track the organizational aspects of a law firm, I'd want the names of the constructs in my software system to be as closely aligned to the words law professionals use to describe their own concepts. `PracticeAreas` instead of `Topics`. `PracticeGroups` instead of `Teams`. `Attorneys` and `Paralegals` instead of `Users`. This way, I don't have to make the mental mapping between what the client calls Concept X with what I call Concept Y.

Business concepts can also dictate the kind of structures we introduce to a codebase. Consider this example.

If I were building a patient management system for a medical clinic, I might define a doctor's patient list with a structure like `List<Patient>`. It seems reasonable enough. Plus, I get all the normal functions associated with a `List` out-of-the-box, like `Add()`, `Remove()`, `Count()` and so forth.

But, doctors don't call these things patient lists, they call them _panels_. Doctors talk about how full their panels are or when new openings in their panel will open up. Knowing this, I'd opt to create a `Panel` class instead which houses the `List<Patient>` collection as its own property.

```
public class Panel
{
  private List<Patient> _patients;  
  ...
}
```

Because medical professionals have defined a specific concept called panels, it's almost certain that there are specific attributes tied to a panel -- it's not just a convenient alias to a collection of patients. 

As it turns out, panels aren't just assigned to doctors, but to medical assistants and health educators. Panels also have a certain optimal number of patients (so that all clinicians generally see the same number of patients). Panels can have openings or be closed. It turns out there's a lot of other attributes tied to the concept of a panel. 

```
public class Panel
{
  private List<Patient> _patients;  
  
  private Doctor _doctor;
  private HealthEducator _healthEducator;
  private List<MedicalAssistant> _medicalAssistants;
  
  private bool hasOpening;
  ...
}
```

To add a patient to a panel, my first attempt at a method signature might be something like:

```
public class Panel
{
  ...

  public void Add(Patient p)
  {
    patients.Add(p);
  }
}
```

Certainly, `Add` is clear, and it follows directly from the `Add()` method associated to the `List`. But, doctors usually talk about patients _subscribing_ to a panel. So, `Subscribe` is a far more fluent approach.

```
public class Panel
{
  ...

  public void Subscribe(Patient p)
  {
    patients.Add(p);
  }
}
```

These name and construct choices seem small at first. But, as the codebase grows and the functionality gets more complex, the benefits become more significant.

By naming concepts in code the way in which software users would talk about them, we remove a layer of unnecessary interpretation. Our code becomes less foreign to people that aren't directly working with it. Software writers can have more productive discussions with other teams within the organization. And, most importantly, we have a better grasp of the business concepts we have to maintain.

