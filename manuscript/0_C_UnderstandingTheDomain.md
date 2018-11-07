## Understanding the Domain

(Need to clean up this chapter -- I combined the old intro on domain with 'pattern hynposis')

Good naming also helps improve the communication of a larger team. For instance, if I were building internal software to track the organizational aspects of law firm, I'd want the names of elements of my software system to be as closely aligned to the words law professionals use in their profession. PracticeAreas instead of Topics. PracticeGroups instead of Teams. Clients instead of Customers. Attorneys and Paralegals instead of Employees.

Business concepts can also dictate the kind of structures introduced in a codebase. If I were building a patient management system for a medical clinic, I might start with a concept like List<Patient> to describe a doctor's patient list. But, I've come to find that doctors don't call these things patient lists, they call them panels. Doctors talk about how full their panels are or when new openings in their panel will open up.

Knowing this, I'd opt to create a Panel class which would house the List<Patient> collection and any other attributes specific to a doctor's panel. I might also start with a method like Panel.Add(Patient p). But, doctors usually talk about patients subscribing to a panel. So, Panel.Subscribe(Patient p) is the more fluent approach.

By naming concepts in code the way in which real software users would talk about them, we remove a layer of interpretation. Our code becomes less foreign to people that aren't directly working with it. Software writers can have more productive discussions with other teams within the organization. And, perhaps most importantly, we have a better grasps of the business concepts we have to maintain.

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

