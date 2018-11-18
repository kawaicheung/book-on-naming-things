## Maintaining Code Shape

_Good design_ in software usually means something that isn't immediately visible. You have to spend awhile working with the code to appreciate its design. But, there's a more immediate kind of code design.

Good code design has a certain _shape_ to it. It's spaced well. It's broken out nicely. It's easy to scan.

Variable names can play a large role in determining this shape. Consider this simple `for` loop.

```C#
for (int i=0; i < tokens.length; i++)
{
  if (tokens[i].Activated)
  {
    usedTokens.Add(tokens);
  }
}
```

To me, this code has good shape. The indents and spacing are consistent--but, look further. The importance of the variables are commensurate with their size. When I read this code, it doesn't take me long to figure out that tokens are added to a `usedTokens` list if they are activated. That's because the surrounding code that helps me understand this isn't getting in the way.

Here's that same code block again with one name change. I substitute the index variable `i` with a new, more precise, name.

```C#
for (int curIndexOfTokenArray=0; 
	curIndexOfTokenArray < token.length; 
	curIndexOfTokenArray++)
{
   if (tokens[curIndexOfTokenArray].Activated)
   {
     usedTokens.Add(token);
   }
}
```

Up until now, I've championed the idea of name clarity. Following this line of thought, `curIndexOfTokenArray` is certainly be a better name than `i`. The name is far more clear. 

But, now we face a more onerous problem. The whole thing just _looks_ substantially more complex. We need more time to digest what it's saying. The code has lost its shape.

While the current index is a critical anchor of a `for` loop, representing it in a verbally meaningful way isn’t. For one thing, an index is common to _every_ kind of `for` loop. In addition, it's scope is small -- it only exists for the duration of a few lines inside the loop. If someone were really confused about the variable name, they only need to look around a small visual radius to get refamiliarized.

The stars of the show here are the `tokens` and `usedTokens` arrays. Adding more description to `i` brings a peripheral stage crew member into the spotlight. Look at the line `if (tokens[curIndexOfTokenArray].Activated)`. I have to read this line a couple of times to parse through it.

The original version values the shape of the entire statement over describing a variable which doesn't need that type of attention:

```C#
for (int i=0; i < tokens.length; i++)
{
   if (tokens[i].Activated)
   {
     usedTokens.Add(tokens);
   }
}
```

Here’s another example. In this method, `systemTimeZones` represents a collection of objects each describing a time zone. The method loops through this collection, then extracts time zone information to build a list of `DropDownComponents` while marking the passed-in time zone as `Selected`:

```C#
public List<DropDownComponent> BuildTimeZonesDropDownList(string selectedTimeZone)
{
  var result  = new List<DropDownComponent>();
  var systemTimeZones = TimeZoneInfo.GetSystemTimeZones();

  foreach (var systemTimeZone in systemTimeZones)
  {
    var comp = new DropDownComponent(systemTimeZone.Id, systemTimeZone.DisplayName);

    if (systemTimeZone.Id == selectedTimeZone)
    {
      comp.Selected = true;
    }

    result.Add(comp);
  }

  return result;
}
```

The issue with the shape of this code is how repetitive some of the variable names look. There are several names in this method that all look similar at a glance:

* The `systemTimeZones` collection
* The `systemTimeZone` object scoped in the `foreach` loop
* The `selectedTimeZone` parameter
* The `GetSystemTimeZones()` method call. 

Scenarios like these are tricky because each name, in isolation, is appropriately descriptive. No single name is egregiously lengthy, misleading, or overly detailed. 

The problem, however, is when we step back and read the method as a whole. It reminds me a lot of the lines of a Dr. Suess story. One of my two-year-old son’s favorites is _Hop On Pop_, which begins:

> UP PUP - Pup is up. 
> CUP PUP - Pup in cup. 
> PUP CUP - Cup on pup.

The lines of Dr. Seuss, of course, are intentionally dizzying. My son loves to get lost in the pattern of the words and giggles at the absurdity of its rhythm. But, reading dizzying code at 5pm isn't that fun. For this, we need to make some name improvements.

The most immediate, impactful name change I can make is with the name of the `systemTimeZone` object. It appears four times within the method. No other similarly named construct appears more than twice. Also, because its reach is small (only scoped to the `foreach` loop), we can get away with a less descriptive name without doing much harm to the reader’s understanding. 

Let’s try reducing `systemTimeZone` to something shorter, like `tz`:

```C#
public List<DropDownComponent> BuildTimeZonesDropDownList(string selectedTimeZone)
{
  var result  = new List<DropDownComponent>();
  var systemTimeZones = TimeZoneInfo.GetSystemTimeZones();

  foreach (var tz in systemTimeZones)
  {
    var component = new DropDownComponent(tz.Id, tz.DisplayName);

    if (tz.Id == selectedTimeZone)
    {
      component.Selected = true;
    }

    result.Add(component);
  }

  return result;
}
```

I like this change already. Now, the details of the `foreach` loop are much easier to scan. In addition, all of the other similarly-named constructs benefit. They're given more room so that the similarity in their names aren’t as distracting on the eyes as they were before.

Just like our prior example, the variable `tz` feels sized appropriately. Conceptually, a small name like `tz` feels like just one element in a longer-named collection like `systemTimeZones`. These are the kinds of subtle visual cues that all lend themselves to good code shape.

Next, I decide to pare down the variable `systemTimeZones` to just `timeZones`. I originally named this list `systemTimeZones` to follow how the .NET framework named the get method I'm using (`GetSystemTimeZones()`). But, the word _system_ doesn't add any helpful meaning here. This change also helps better differentiate from the `selectedTimeZone` variable used inside the `foreach` loop.

```C#
public List<DropDownComponent> BuildTimeZonesDropDownList(string selectedTimeZone)
{
  var result  = new List<DropDownComponent>();
  var timeZones = TimeZoneInfo.GetSystemTimeZones();

  foreach (var tz in timeZones)
  {
    var component = new DropDownComponent(tz.Id, tz.DisplayName);

    if (tz.Id == selectedTimeZone)
    {
      component.Selected = true;
    }

    result.Add(component);
  }

  return result;
}
```

I could go further and rename the other variables more uniquely, but I don't feel it's necessary. When I read the updated method, I don't feel that dizzying effect of all those similarly named variables that I did earlier.

When we edit names, we shouldn't look at them in isolation. That habit can drive naming decisions that don’t actually benefit the overall readability of the surrounding code. 

Instead, look at the context in which these names live. Find out what’s making a section of code difficult to read and solve the larger problem. It might be a more descriptive variable name, but it might also be a terse one. Let the full context drive those decisions. In the previous examples, I didn't value the precision of certain variable names as much as I did the shape of the lines around them, particularly because the scope of those variables was small.

When editing prose, we read whole sentences and paragraphs to get a sense of readability and style. It’s the same in code-writing.
