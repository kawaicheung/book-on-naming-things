## Understanding the Domain

Law, medicine, the associate trap.


Good naming also helps improve the communication of a larger team. For instance, if I were building internal software to track the organizational aspects of law firm, I'd want the names of elements of my software system to be as closely aligned to the words law professionals use in their profession. PracticeAreas instead of Topics. PracticeGroups instead of Teams. Clients instead of Customers. Attorneys and Paralegals instead of Employees.

Business concepts can also dictate the kind of structures introduced in a codebase. If I were building a patient management system for a medical clinic, I might start with a concept like List<Patient> to describe a doctor's patient list. But, I've come to find that doctors don't call these things patient lists, they call them panels. Doctors talk about how full their panels are or when new openings in their panel will open up.

Knowing this, I'd opt to create a Panel class which would house the List<Patient> collection and any other attributes specific to a doctor's panel. I might also start with a method like Panel.Add(Patient p). But, doctors usually talk about patients subscribing to a panel. So, Panel.Subscribe(Patient p) is the more fluent approach.

By naming concepts in code the way in which real software users would talk about them, we remove a layer of interpretation. Our code becomes less foreign to people that aren't directly working with it. Software writers can have more productive discussions with other teams within the organization. And, perhaps most importantly, we have a better grasps of the business concepts we have to maintain.
