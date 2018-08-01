## When is a boolean name not bad?

When is it ok to have a negative boolean?

not_billing_page in base controller...

Make names based on readability today, rather than futureproofing guesses... 


won, !won = lost. (Is there a "Not" version of that word)

IsAdmin, IsNormalUser

1. Does it read clearly?
2. Remove the most amount of 'flipping' (!won -> not_won -> lost), (!isAdmin = isNormalUser)
3. Perimssions.UserCanAccessProject -> Use case is always !, so how about Permissions.UsercannotAccessProject? But that takes a little mental tweaking. How about Permissions.UserDeniedAccess?
3. Whereas if (billing_page) { // do nothing } else { ... } vs. if (!billing_page) {  }...feels weak now.


Gold / Silver / Bronze / Paper

if (Paper)

maybe...

if (no_user)
else (no_avatar)
else (no_initials)
else // user & avatar & initials

vs

if (user && avatar & initials)
else if (initials & !avatar & !user)
else
