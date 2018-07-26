# Verbs

I'm staring at a method name inside a `Project` class called `UpdateUsers()`.

```C#
public void UpdateUsers(List<Users> users) { … }
```

The name seems straightforward enough. It should take a list of users and grant them access to a given project. But, what does it do to the users who aren't in the list that currently have access? Will it remove them, leaving access to only the ones I've passed in? Or, will it keep them around? A method named this way doesn't tell me enough. I have to look inside and see what's going on.

Once I've wrapped my head around the story (it turns out that existing users not in the list *are* removed), I start with a short comment on the method so that I don't have to investigate it again, a month from now, when I'll likely have the same question.

```C#
// Overwrites this project's assigned users with the passed-in users list
public void UpdateUsers(List<Users> users) { … }
```

But, I don't get a huge bang for the buck on the comment. I've spent a lot of words just to say something that I can impart directly into the name. So, I replace the comment with a better name.

```C#
public void OverwriteUsers(List<Users> users) { … }
```

Better. There's far less ambiguity with what's going to happen to this project's users after the method runs. **Overwrite** clearly means the old users aren't sticking around. 

Here's another way to test how the name feels. If I were to pass in an empty list or null into `UpdateUsers()`, what would happen? 

```C#
myProject.UpdateUsers(null);
```

Again, I'd likely expect all user access removed. But, I wouldn't be entirely shocked if the method left the project's users intact. If I were to pass the same argument to...

```C#
myProject.OverwriteUsers(null);
```

...I'd expect it to do some serious damage. That's just the feeling I want to convey.

I also use `Overwrite` instead of, say, `Override` because I like to avoid names that have other reserved meanings. In this case, `override` is a modifier used in many languages to extend or modify the implementation of an inherited method. It's best to stay away from using names that have a prescribed meaning in the language already.

`ReplaceUsers()` could work just as well. It's not a bad choice and there are days I might favor it, but it feels a touch subtler to me than `Overwrite`. Given the choice, I lean toward stronger words initially for clarity, then scale back later on the harshness over time if it doesn't feel right.

When I can't find that perfect verb, I'll look up synonyms. A quick Google search for alternatives to Replace and Overwrite gives me options like Change, Alter, Displace, Reimburse, Substitute, Swap, Upgrade, Improve, or Modernize.  None of these feels nearly as good and some feel entirely strange. 

The reverse is true too -- when I feel like a name is right but I'm not completely sure, searching for synonyms helps me confirm the one I've picked is, at the very least, pretty good for now.

OverwriteUsers() still doesn't clarify something though. Under the hood, I know that users are linked to projects via Membership records. Memberships hold additional bits of information relevant to a user's project access -- perhaps the date access was granted, their particular role on the project, or some notification settings tied to that membership. Knowing this, OverwriteMemberships() becomes an even more expressive name. 

But, what does OverwriteMemberships() do for users who both have membership currently and are in the passed-in list? As it stands, it sounds like their Membership will be removed and re-created, thereby erasing the additional bits of information tied to it.

Another peek inside the method reveals that, in fact, the code is being delicate with existing memberships. All existing memberships who will continue to have access will stay intact rather than be removed and re-created. There might be a better name to express this additional nuance, like UpsertMemberships() or SyncMemberships(). I don't have the perfect answer here, but this exercise tells us a couple of things.

First, it's hard to pack every subtlety of a method's implementation into its name. Instead, I do my best to get the most important aspects of the implementation into the name. A comment helps with context around any extra nuance.

// Will keep current memberships in users list intact
public void OverwriteMemberships(List<Users> users) { … }

Second, if I can't find a method name that covers enough of the essential parts of what it does, this could be a sign that it's doing too much. Perhaps I need to break up the method into smaller ones so that each one's intent can be more clearly wrapped in a name.

In this case, however, I'd prefer to leave the method intact. It's not worth trading the succinctness of a single function that handles updating a project's memberships as a unit of work for a series of smaller, separated functions just because they might be more perfectly named. I'll live with the comment and move on to more important work.

---

This seems like a whole lot of thinking for something seemingly inconsequential. But, once you get accustomed to critiquing names this way, the exercise becomes a normal, fluid part of the code writing process. Like any skill, at first it feels unnatural and cumbersome; Later, it just becomes a way of thinking and you get fast at it. Your code reads more expressively -- it's hardly inconsequential at all.


Aside from Update, Get, Create and Delete are unsurprisingly among the next most commonly used verbs applied to a method. At first glance, it seems like there would be less uncertainty with those words. If we're getting something, we're retrieving it from somewhere without manipulating it. If we create something, we're clearly building something new. If we're deleting something, we're surely getting rid of it in some permanent fashion. But, even these words should undergo some scrutiny.

For example, I have a couple of overloaded methods in a PeopleService class named GetPeople() that return a list of people based on differing inputs.

public List<Person> GetPeople(int[] ids) { … }
public List<Person> GetPeople(string first_name_match, string last_name_match) { … }

On the positive side, it's nice to have these methods named identically. It reduces the number of method names in the class which, in turn, tightens the shape of the class. However, the latter implementation of GetPeople() is really more of a search -- I'm not guaranteed any result back. SearchPeople() conveys the meaning more easily.

In addition, there likely won't be many use cases for a method like this other than as a search feature. By using the Search verb here, and assuming we've named other related components along the stack similarly, the workflow from the UI to backend will read more cohesively. This is well worth adding an additional method name to the service class.

[Search UI] => [POST] /app/search => [Search() controller action] => GetPeople() 
[Search UI] => [POST] /app/search => [Search() controller action] => SearchPeople()


Let's look at another example. I have a method that will return a unique API key for a given user. I could come up with something like CreateAPIKey(). But, GenerateAPIKey() conveys the idea much more clearly. Instantly, I know the key creation involves some random generation rather than some replayable set of steps. It hints at the implementation detail even if I don't need it right away.

Later on, I may want to introduce a way to revoke and issue a new key as follows:

public string RegenerateAPIKey() 
{
   RevokeAPIKey();
   return GenerateAPIKey();
}

If, instead, I had settled for the more generic Create, my penchant for consistent naming would drive me to a corresponding method like this:

public string RecreateAPIKey() 
{
   RevokeAPIKey();
   return CreateAPIKey();
}

RecreateAPIKey() makes me think, incorrectly, that an already-used API key will be reinstated. RegenerateAPIKey() makes me think, correctly, that the new API key isn't being revived from the ashes.

Delete is a loaded term. On the surface, we know it means something like “get rid of this thing and never let me see it again.” And, that's generally good enough for users. But, programmatically, this could mean a number of things. I might be placing a flag on a record in a database, removing a relationship between items to prevent access, or, actually hard deleting the record for good.

When I use the term Delete as part of a method name, I usually don't care about the implementation -- I only care about the meaning in the scope of the class I'm working in. In a method like Delete(int project_id) on a ProjectsController class, I only care that executing this  method means the project is no longer accessible by the user. Whether aspects of the project still remain behind-the-scenes is irrelevant.

I make an exception when a method does perform a true, irreversible delete on data that's not easily recreatable -- when I'm not softly removing, dropping, clearing, hiding, or deactivating, but in fact, I'm permanently destroying something, for good.

Choosing such a term is difficult. Eradicate? Destroy? Burn?

Ruby on Rails creator David Heinemeier Hansson reserves the term Incinerate in Basecamp to separate a normal, soft delete, from the sweaty-palms kind of delete.  It's become part of the technical lexicon for the development team.  “When we talk about incineration within the app, it means this one, specific thing.”

[Perhaps add a list of better verbs instead of Create, Update, Get, Delete]

Update: Assign,Add,Merge
Get: Extract (Extract email from text in DD add-item emails first line), Convert
Create: Generate...
Delete: Remove,Incinerate,Destroy
