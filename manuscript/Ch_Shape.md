## Shape {#ch_shape}

Thus far, we’ve talked about what good, descriptive names look like. But, descriptive names aren’t necessarily _good_. This is especially true of variables scoped to a small amount of code. For example, consider this simple for loop:

```C#
for (int i=0; i < players.length; i++)
{
  if (players[i].Activated)
  {
    usedPlayers.Add(players);
  }
}
```

Let’s talk about the indexing variable i. Judging this variable’s clarity solely on the meaning of its name, i seems like a really poor choice.

But, since i is instantiated at the beginning of the for loop, we know that i only exists within the context of the loop -- it’s scope is small. It doesn’t leave a big footprint. In the context of the loop, it’s clear how i is used.
But, suppose we wanted the variable to express itself more clearly. What might a more meaningful replacement name be? Well, it’s job is to hold the current index of the Players list, so cur_index_of_players_list could work:

```C#
for (int cur_index_of_players_list=0; cur_index_of_players_list < players.length; cur_index_of_players_list++)
{
   If (players[cur_index_of_players_list].Activated)
   {
     usedPlayers.Add(players);
   }
}
```

Undoubtedly, the name, in and of itself, is clearer. With no surrounding code, I know what cur_index_of_players_list represents. I don’t get that same benefit from i. 

But, now we face other, more onerous, problems. The for loop now looks substantially more complex. We need to take longer to digest the code -- and most notably on the very first line.

While the current index is the critical anchor of a for loop, representing that index in a verbally meaningful way isn’t -- particularly in this case where the physical size of the for loop is small, and the reach of the variable is minimal.

What matters far more here is emphasizing the right players. The stars of the show here are both the players and usedPlayers lists. They’re the ones we’re interested in reviewing and updating. Adding more description to i now brings the side attraction to the forefront. 

Going back to the original version, the code’s physical shape feels more appropriate. It reads more smoothly. It’s easier to see everything that’s happening. If, for some reason, i isn’t clear to the reader, it’s not hard to re-examine the rest of the code it’s scoped to, to figure out its purpose.

for (int i=0; i < players.length; i++)
{
   If (players[i].Activated)
   {
     usedPlayers.Add(players);
   }
}

Here’s another example. In this code snippet, system_time_zones represents a collection of TimeZone objects. We loop through the collection, extracting time zone information to build a list of DropDownComponents, marking the passed in time zone as selected:

       public List<DropDownComponent> BuildTimeZonesDropDownList(string selected_time_zone = null)
        {
 List<DropDownComponent> result  = new List<DropDownComponent>();

            var system_time_zones = TimeZoneInfo.GetSystemTimeZones();

foreach (var system_time_zone in system_time_zones)
            {
                var component = new DropDownComponent(system_time_zone.Id, system_time_zone.DisplayName);

                if (system_time_zone.Id == selected_time_zone)
                {
                    component.Selected = true;
                }

                result.Add(component);
            }
    }

There are several constructs in this method that have similar looking names --  the system_time_zones collection, the system_time_zone object scoped in the for loop, the selected_time_zone parameter, and even the GetSystemTimeZones() method call. I’ve bolded them below:
 
public List<DropDownComponent> BuildTimeZonesDropDownList(string selected_time_zone = null)
        {
 List<DropDownComponent> result  = new List<DropDownComponent>();

            var system_time_zones = TimeZoneInfo.GetSystemTimeZones();

foreach (var system_time_zone in system_time_zones)
            {
                var component = new DropDownComponent(system_time_zone.Id, system_time_zone.DisplayName);

                if (system_time_zone.Id == selected_time_zone)
                {
                    component.Selected = true;
                }

                result.Add(component);
            }
    }

Scenarios like these are difficult because each name, in isolation, feels appropriately descriptive.  No single name is egregiously lengthy or over detailed. 

But, the method reads a bit like the lines of a Dr. Suess story -- one of my two-year-old son’s favorites is Hop On Pop, which begins:

> UP PUP - Pup is up. 
> CUP PUP - Pup in cup. 
> PUP CUP - Cup on pup.

The lines of Dr. Suess, of course, are intentionally dizzying -- it’s fun to get lost in the words as my son cackles at the absurdity of the rhythm. On the contrary, It’s not all that fun to read dizzying code. For this, we need to make some name improvements.

For me, the most impactful name change is with the system_time_zone object. First, it appears four times in the method, while no other similarly named construct appears more than twice. Second, it’s scoped only to the foreach loop itself. As with the previous example, we can get away with a less descriptive name without doing too much harm to a reader’s understanding of the logic.

Let’s try reducing system_time_zone to something really barebones, like s:

public List<DropDownComponent> BuildTimeZonesDropDownList(string selected_time_zone = null)
        {
 List<DropDownComponent> result  = new List<DropDownComponent>();

            var system_time_zones = TimeZoneInfo.GetSystemTimeZones();

foreach (var s in system_time_zones)
            {
                var component = new DropDownComponent(s.Id, s.DisplayName);

                if (s.Id == selected_time_zone)
                {
                    component.Selected = true;
                }

                result.Add(component);
            }
    }

I like this change already. Now, the guts of the foreach loop are easier to scan. In addition, the other similarly named constructs all benefit -- they feel more spread apart from each other so that the similarity in their names aren’t as distracting on the eyes.

My only small apprehension is that s emphasizes the wrong part of system_time_zone.  System isn’t really the critical part of the object - “Time Zone” is. So, an update to tz might give us a little more boost in meaning:

public List<DropDownComponent> BuildTimeZonesDropDownList(string selected_time_zone = null)
        {
 List<DropDownComponent> result  = new List<DropDownComponent>();

            var system_time_zones = TimeZoneInfo.GetSystemTimeZones();

foreach (var tz in system_time_zones)
            {
                var component = new DropDownComponent(tz.Id, tz.DisplayName);

                if (tz.Id == selected_time_zone)
                {
                    component.Selected = true;
                }

                result.Add(component);
            }
    }

I like this more. While, on it’s own, tz isn’t that much more descriptive than s, within the entire method, it’s more clear the name references an individual time zone than s does.

--- (maybe applies to past few sections -- shape, and the one before that on sentences/phrases “hr_manager” vs. “should_send_email”) --

When we edit names, it’s important to not look at names in isolation. That habit can drive naming decisions that don’t actually benefit the overall readability or scannability of the surrounding code.

Instead, look at the larger context that these names live in. Find out what’s making a section of code difficult to understand and solve that specific problem. When editing prose, we read whole sentences and paragraphs to get a sense of readability and style. It’s the same in code-writing.

