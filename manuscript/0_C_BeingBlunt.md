## Being blunt

Many of us have trouble communicating directly. We don't want to offend. We don't want to come across as having an unwavering opinion. We try to dampen our real intentions behind a fog of words. In code, this can be  dangerous.

Consider what it means when we see a method like `Delete()`. It's a loaded term. On the surface, we know it means something like "get rid of this thing and never let me see it again." And, that's generally good enough for users. 

But, programmatically, this could mean a number of things. We might be merely placing a flag on a record in a database or removing a relationship between items to prevent access. It's also possible we're wiping the record for good.

When we use the word `Delete` as part of a method name, we usually don't care about the implementation --- we only care about the meaning in the scope of the class we're working in. In a method like `Delete(int project_id)` on a `ProjectsController` class, we only care that executing this method means a user can no longer access it. Whether aspects of the project still remain intact behind-the-scenes is irrelevant.

However, it's smart to make an exception when a method does perform a true, irreversible delete on data that's not easily recreatable. Because the word `Delete` is such a ubiqutious term in programming, it might not convey that meaning appropriately.

Choosing such a word is difficult. It should have even more cautionary weight than `Delete`.

David Heinemeier Hansson (Basecamp CTO and Ruby on Rails creator) reserves the term `Incinerate` in Basecamp's codebase to distinguish the "sweaty-palms" kind of delete from the softer, "just put a flag on it" type. By doing so, it's become part of the technical lexicon for the Basecamp development team. “When we talk about incineration within the app, it means this one, specific thing.”[^dhh1] Destroy or Eradicate could also work.

Contrast this to a word like "Remove" which feels softer -- as if you can bring it back if you had to. Use "Remove" for things that could easily be brought back (or recreated from scratch if need be). I would be nervous to name a method "RemoveUser" that deletes all user data but OK with "RemoveUser" if it untied a user from an account which can easily be re-attached.

Be blunt with your words. If something permanent is going to happen in your method, make sure your name makes that obvious. Being clear about these actions adds another layer of defense beyond testing. You can squash any potential misfortunes much earlier in the QA process with strong words. 
