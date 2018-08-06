# On naming booleans

A programming language _reads_ most like traditional language when we're writing at the most fundamental level -- with booleans and logic operators. A statement like `if (published && editable)` can be read pretty much as its written -- "if published and editable".  Most people who've never touched a programming language would be able to comprehend code at this level of detail.

Yet, we can easily convolute these statements into readability nightmares when the booleans involved are poorly named or logical operators aren't edited down into the simpest forms.

The simple statement above has the same meaning as `if (!(unpublished || !editable))`. But, the mess of nested scopes, misdirections, and negations make it impossible to deduce this right away. It's the kind of statement I often see in code that still works and passes tests. It's just developed the cruft common to code that's been handled hastily a few times without someone taking the time to unravel it.

You can tell a boolean's name is hurting by reading how it's used in context. "If it's not either unpublished or not editable" reads more like a crafty lawyer's statement than a straightforward piece of writing. 


How do we prevent this?

(1) Removing ! where possible

(2) Negative booleans are usually bad...but that's not necessarily true.

(3) Overall, read thru how the booleans are used in statements. See if the statement itself can be contorted.

(4) Ternary form...overly complex if more than a small bit.


When is it ok to have a negative boolean?

not_billing_page in base controller...

Make names based on readability today, rather than futureproofing guesses... 


Booleans are most used in programming with "logic sentences" vs. say an object where we might just be working on it -- a constrcutor... so booleans need to "read well" in the context of the logic statements -- most like sentence writing.

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
