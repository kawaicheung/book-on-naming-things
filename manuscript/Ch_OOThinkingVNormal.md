I wanted to add a check if the model person has an avatar on the view. instead of thinking 

string.IsNullOrEmpty(Model.Person.AvatarURL) on the view, my mind has to change to think "Who should know about this info? Let me empower that object to figure that out."
