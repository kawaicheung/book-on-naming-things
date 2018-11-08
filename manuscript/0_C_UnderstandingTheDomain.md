## Writing to the Domain

The image of the recluse toiling away at code, completely isolated from the rest of the business is -- fortunately -- an antiquated one. People that write code today are often the same ones making business decisions, collaborating directly  with clients. This makes it even more critical that programmers understand the business reasons behind their work.

The best way we can encourage this is to infuse the language of the business directly into our code.

If I were building internal software to track the organizational aspects of a law firm, I'd want the names of the constructs in my software system to be as closely aligned to the words law professionals use to describe their own concepts. `PracticeAreas` instead of `Topics`. `PracticeGroups` instead of `Teams`. `Attorneys` and `Paralegals` instead of `Employees`. I don't have to make the mental mapping between what the client calls Concept X with my Concept Y.

Business concepts can also dictate the kind of structures introduced in a codebase. If I were building a patient management system for a medical clinic, I might define a doctor's patient list with a structure like `List<Patient>`. 

But, doctors don't call these things patient lists, they call them _panels_. Doctors talk about how full their panels are or when new openings in their panel will open up. Knowing this, I'd opt to create a `Panel` class instead, which houses the `List<Patient>` collection. 

Because medical professionals have defined a specific concept called panels, it's almost certain that there are specific attributes tied to a panel. It's not just a convenient alias to a collection of patients. As it turns out, panels aren't just assigned to doctors, but to medical assistants and health educators. We can also apply demographic data to a panel. 

To add a patient to a panel, my first attempt at a method signature might be something like `Panel.Add(Patient p);`. Certainly, `Add` is clear, but, doctors usually talk about patients _subscribing_ to a panel. So, `Panel.Subscribe(Patient p)` is a far more fluent approach.

By naming concepts in code the way in which real software users would talk about them, we remove a layer of interpretation. Our code becomes less foreign to people that aren't directly working with it. Software writers can have more productive discussions with other teams within the organization. And, perhaps most importantly, we have a better grasps of the business concepts we have to maintain.

