
Regions of a file

DD -> SignupController organziation - 

	create account / begin / complete 
	register user

vs. a traditional get/update/read/create region structure.

both worthwhile...depends on what you wanna emphasize. whats the 'story' of that controller? of that class?

doesn't have to all be the same....not everything is as basic as CRUD.

an Application controller that houses various small bits vs. having a 
api / rate limit / service uptime controller etc. Theres a cost to breaking things into so many little pieces (even tho common thought is that a class shouldnt have more than X number of methods etc. i disagree. it all depends on the story, on groking where things might be.)
