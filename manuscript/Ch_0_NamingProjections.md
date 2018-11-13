## Naming Projections

[Quick intro on what a projection is. How its easy to be lazy with ORMs and other frameworks by pulling out 'everything' -- and particularly as you get into front-end and you just pull the entire mess onto the js layer when you only need specific things. Part of the pain is what to name these things because you end up with lots of projections of a central business object.]

A set of words I often see used in domain object names (and, I admit, I often use in moments of weakness) are Data and Info.  AccountData. ProjectData. CustomerInfo. MessageInfo. Extra filler words that emphasize that the objects of these types will, indisputably, include some sort of data or info you will have an interest in!

I resort to these words on those occasions for a couple reasons. 

First, when I don’t want to suggest that the object is, wholly, what it says it is. An AccountData class holds pieces of information that all relate to an account, but to boldly suggest that it is the Account class is misleading. 

Second, I may already have a main class (like Account) but I’ve introduced a second projection that contains only aspects of the main class. Let’s analyze a simple example.

An Account class that lives in my presentation domain layer might look like this:

```C#
class Account 
{
  int ID;
  string Name;
  List<Person> Memberships;
  DateTime CreatedOn;
  PlanType Plan;
  // And so forth...
}
```

However, in a few cases throughout my app, I only need the Account’s ID, Name and CreatedOn date. In other cases, I just need the ID and Plan.

I could pass around Account instances to these various cases. But, if I only need a few pieces of information from the account, it’s wasteful to hydrate entire Account object and carry it around. The assembly of an Account instance might require a few hefty database and external API calls that render information I simply don’t need in these cases.

I could implement additional construction methods that only hydrate a portion of the Account class. But, now I have to know which way an Account was created to guarantee the right information will exist to avoid unintended null or empty case scenarios.

I’d rather create separate objects -- projections of the Account class -- tailored to these use cases. They hold all, and only, the information needed in my use cases. For now, I resort to my naming crutch. I call these AccountInfo and AccountInfo2.

```C#
class AccountInfo
{
  readonly int ID;
  readonly String Name;
  readonly DateTime CreatedOn;

  public AccountInfo(int id, int name, DateTime created_on) 
  { 
  	// Hydrate...
  }
}
```

```C#
class AccountInfo2
{
  readonly int ID;
  readonly string Name;
 
  public AccountInfo2(int id, int name)
  {
  	// Hydrate...
  }	
}
```

It’s challenging to find good names for projections of a main class. What do you call “a portion” of anything, especially when that portion is seemingly an arbitrary selection of elements? Terms like Info and Info2 (and Data and Data2...) don’t suggest something misleading about the objects. So, I resort to them at first pass so I can get on with the code.

However, from the reader’s point of view, the indistinctiveness of the term is its crutch. It tells the reader nothing of value --- it’s only real use is to distinguish the class names so the app can compile. If your projection classes start growing in number, the litter of DataX and InfoX classes only add technical debt.

I usually can update these generic filler words with a healthier replacement by examining how each projection is used. Usually, there is some primary use case for each projection we can describe in a word or two. That turns into what I add to the projection’s name.

For instance, in this example, I use AccountInfo (ID, Name, CreatedOn) in a list view on the application’s global administration section. I use AccountInfo2 (ID, Name) across a few places in the app, but they all appear within some sort of selection -- a dropdown picker in the global admin, an account switcher UI for a user account, and so forth.

Using their end use cases as inspiration, I rename AccountInfo as AccountForListing and AccountInfo2 as AccountForSelection. I prefer keeping Account as a common prefix so that’s projections are grouped together visually when accessed through code hints or viewed in an object explorer.

These names are specific enough to intuit when to employ them, but still generic enough where it makes sense to use them in multiple locations. An AccountForListing instance fits in any view model displaying a list of accounts - not just a results page but, say, a public API method to get a collection of accounts. An AccountForSelection instance belongs to any view model that requires a collection of account data a user might choose from -- a dropdown, checkbox list, or navigation menu.

I continue naming projections for a particular object with this approach until the returns aren’t as bountiful. 

One of the common traps of this kind of domain model naming is falling into a naming pattern. Every kind of Account projection becomes an AccountForSomething, every kind of User projection becomes a UserForSomething and so forth. Usually, we can get around this hypnotic pattern in a few ways.

If we have a lot of projections off the same domain model, it’s very likely there are common bits that can be pushed into a base object and then inherited by other projections. This way, we can better deduce which projections have identical or nearly identical properties and consolidate. This might require some name changing as well. PersonForCompaniesListing, PersonForAdminListing, and PersonForInvitation objects might really contain the same base Person information with some additional contact info (like email and phone). 

We could reduce the three projections into, say, a PersonWithEmailAndPhone object. Even if some of the projections don’t require all of the properties in PersonWithEmailAndPhone, the benefits of consolidation might be worth the trade-off.  In this case, I choose a name that directly describes the object’s contents because it’s being used in several different contexts (on a company listing page, admin page, and for invitations).
