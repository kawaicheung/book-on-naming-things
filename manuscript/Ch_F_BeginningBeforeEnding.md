# Writing the beginning before the ending

There's this common paradigm in programming that I've always found incredibly unnatural. For years, I've pondered my legitimacy as a _real_ programmer because of my deep ineptitude at writing code in this way.

The paradigm I'm talking about is this strange idea of writing the ending _before_ the beginning. It has most notably surfaced in a couple of ways.

* First, in test-driven design (TDD), which tells us to write tests _first_, then write the actual code to make the tests pass.
* Second, in general API design, which often tells us to write the interface before the implementation.

In both cases, we're told to write what the code should do before we write what the code does.

The reasoning behind the paradigm is justifiable. Writing the ending focuses our attention on the end user first. That's who we should be focusing on. But, I find it works better in theory than in practice.

When I write code, I know generally what it's supposed to do. But, I almost never know exactly what it will do until I've written it. That makes it really difficult for me to write "the ending" before I write at least some of the actual beginning.

The clear error in my ways is that I _should_ know exactly what I'm building before I start building it. This is why organizations spend all their time and energy on design definition and specification. 

But, in all my years of programming, I've never been handed a specification that didn't undergo significant changes during the development process. That's because every time I began writing code, new and unanticipated obstacles (or pleasant surprises) would appear which would drive my code writing in directions I couldn't possibly have anticipated in the beginning. By the end of the project, we landed some place different; some place better.

I don't think this is a particularly unique phenomenon in programming. I think this is a very natural phenomenon of creating _anything_ - a story, a recipe, a song, a new invention, or an app.

---

When I code, I like to write freely. Big, long methods. I don't worry about DRYness initially. If I just so happen to know I'm doing something that I've done before - then yes -- I'll stop and look for where I had done this before and reuse the method. Otherwise, I continue to write freely.

When I get the particular feature to work...




