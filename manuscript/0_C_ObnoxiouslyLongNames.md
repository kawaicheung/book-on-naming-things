# Obnoxiously Long Names

One generally agreed upon rule of thumb with names is to keep them short. Concise names are easier to scan. A codebase sprinkled with really long names feels unwieldy. 

But, sometimes, I think this rule is better off broken.

A common case for this? Inheriting someone else’s code. I often find the best way to unravel the underlying meaning behind unfamiliar code is to first examine (and clarify) names. It’s also my first step in any refactoring session.

The beauty of this step is it can be done safely, particularly with a type-safe, compiled language. Many of the frameworks that use these languages might already come with a set of convenient refactoring tools; And, one of the most basic of these tools is some sort of “Rename...” feature. Instead of copying and pasting, just right-click and rename something, knowing any references will adjust accordingly.

When I dive into the dark underbelly of an unfamiliar and unruly codebase, I use renaming as the first tool in its rehab. My goal isn’t to change the structure of the code in any way, but simply to disambiguate the meanings of anything within it. It helps me get to know the code I’m going to have to deal with intimately before making any behavioral change. The mere act of making names more clear gives me a sense of control. Renaming is therapeutic.

Early on, I try not to get too prescriptive with “clean” names because my goal is comprehension, not elegance. Aesthetics, uniformity, consistency, and other worthwhile goals are secondary in the beginning. Naturally, I find that the names I come up with can be unusually long. 

I’ve inherited an application with a large class that manages employee paid time-off policies for companies. Time-off policy calculations seem straightforward at first. There’s some amount of time an employee gets to take off per year, an amount they’ve taken off, and the difference is what they have left. Three variables. Simple enough!

But, like all business concepts, the devil is in the details; In this case, it’s all over, around and through them. The codebase I’m working with supported accrued time. This means that employees can earn a certain amount of PTO time as the year goes on. It supports carryover time (how much went unused from the year prior), carryover expiration (when an employee needs to use up said time in the current year), a maximum amount they could accrue, manual time adjustments, matriculating to new policies, and a host of other extraneous levers.

The original developer of this logic took a reasonable approach that looks something like this (I’ve simplified the names and approach somewhat): 

Park all the logic inside of a single class (TimeOffCalculator). 
Pass in the relevant configuration details of a particular policy (TimeOffPolicy), a particular employee (Employee) and their requested, taken, and approved time offs (TimeOffs) to the TimeOffCalculator constructor.
Implement a Calculate() method which runs a big For loop over each day an employee was a member of a particular policy, doing the requisite math along the way.

I expect this Calculate() method to return a tidy object which describes the relevant data needed regarding the current state of an employee’s policy - an object that would have the answer to questions like “What’s their current balance?” or “How much time did they carryover from the previous year?” or “How many hours did they take off in May?” or “What did their balance look like on July 12th?” or “Did they ever have a negative balance this year?”

But the Calculate() method doesn’t  return such an object. Instead, it returned a decimal. One lonely decimal. A decimal that only represents the balance of hours the employee had to-date. But, how then, could the logic answer all those important questions?

I dive deeper. I discover that those questions are being answered, but not stored in any useful return object, let alone one with meaningful names. Instead, these data points are being concatenated into a gigantic string, building upon itself with each passing day in the For loop iteration. This string is ultimately written out, wholesale, to the browser, but all the opportunity to easily interact with the individual data points in code is lost.

Up until this point, those data points are only being used by our internal admins. So, while a big messy string output isn’t ideal, it got the job done thus far. On the customer facing side, all they see is their current balance (i.e. the return decimal). 

But, the team now wants to push some of those intermediate data points to the customer. A raw string output in 12-point courier font isn’t exactly going to do the trick. I need to refactor the method monolith to return that tidy object I originally was hoping for, so we’d be able to make something much prettier out of it.

But, this end goal is at least a dozen steps removed. 

A deeper problem with this code is that the string concatenation party makes it easy for the original developer to dodge around naming things. 

[Just outputted the “available hours” at the right time to the string. Often times, I’d have to read through to get the right meaning -- but this led to those big names]

My immediate goal was to begin breaking apart this logic into meaningful pieces. 






The largest hurdle with understanding the accrual logic was that 

The class contained a large loop that kept track of this data each day from an employee’s first day to the current day. 

The details of the logic aren’t the point here. But, the names I 

[Discuss Kin accrual logic in broad strokes and need for variables like these:
maxAvailableHoursAllowedForOffsetAdjustment

offsetForCurrentPTOPeriodAccountingForMaxLimitOnAvailableHours

availableHoursThroughThePresentDayNotCountingFutureAccruedTime

availableHoursThroughThePresentDayNotCountingFutureAccruedTimeWithoutPresentPTOPeriodOffset
]

, particularly when the code isn’t well-encapsulated. 


It couldn’t be any simpler of a name.

Smell for more objects.

Ugly as a reminder.

Single responsibility...etc.




Here’s a simple example:

[Example with unraveling names]
