

There's a common thought amongst many programmers that classes ought to be of a certain size and length. No more than X number of methods per class. No more than Y number of lines in a method. Make classes or methods too large and you've sinned.

Much like the 1000-word minimum writing assignment, I hate the idea of putting arbitrary numbers around these well-intentioned rules. Yes, it's likely that a class littered with hundreds of methods is a sign that it can be broken apart into more logical pieces. But, there's a chance that doing so only appeases the arbitrary rule maker and doesn't make the code any easier to understand.

At the beginning of any new method, I tend to write verbosely. I don't see the natural extractions right away. For instance, (user/account membership, login etc.)

My first approach is regions.
















Regions of a file

DD -> SignupController organziation - 

	create account / begin / complete 
	register user

vs. a traditional get/update/read/create region structure.

both worthwhile...depends on what you wanna emphasize. whats the 'story' of that controller? of that class?

doesn't have to all be the same....not everything is as basic as CRUD.

an Application controller that houses various small bits vs. having a 
api / rate limit / service uptime controller etc. Theres a cost to breaking things into so many little pieces (even tho common thought is that a class shouldnt have more than X number of methods etc. i disagree. it all depends on the story, on groking where things might be.)
