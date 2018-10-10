# Code Shape

_Good design_ in software usually means something that isn't immediately visible. You have to spend time digesting the code to grasp how the model it describes is elegant. But, the code _itself_ has a design.

Good code design has a certain kind of _shape_ to it. It's spaced well; it's broken nicely.

Names can play a large role in determining this shape as well. Consider this simple `for` loop.

```C#
for (int i=0; i < tokens.length; i++)
{
  if (tokens[i].Activated)
  {
    usedTokens.Add(tokens);
  }
}
```

To me, this code has good shape. The indents and spacing are consistent. But, look further. The importance of the variables are commensurate with their size. When I read this code, it doesn't take me long to figure out that tokens are added to a `usedTokens` list if they are activated. That's because the surrounding code that helps me understand this isn't getting in the way.

Here's that same code block again with a minor name change. I substitute the `for` loop index with a new name.

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

Up until now, I've championed the idea of name clarity. If clear names are better names, then the name `curIndexOfTokenArray` should certainly be a better option than `i`, right?

There's no doubt the name is more clear. But, now we face a more onerous problem. The whole thing just _looks_ substantially more complex. We need more time to digest what it's saying. The code has lost its shape.

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

Here’s another example. In this method, `systemTimeZones` represents a collection of objects each describing a time zone. The method loops through this collection, extracting time zone information to build a list of `DropDownComponents` while marking the passed-in time zone as `Selected`:

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

There are several names in this method that all look similar:

* The `systemTimeZones` collection
* The `systemTimeZone` object scoped in the `foreach` loop
* The `selectedTimeZone` parameter
* The `GetSystemTimeZones()` method call. 

Scenarios like these are tricky because each name -- in isolation -- is appropriately descriptive. No single name is egregiously lengthy or over detailed. 

The problem, however, is when we step back and read the method as a whole. It reminds me a lot of the lines of a Dr. Suess story. One of my two-year-old son’s favorites is _Hop On Pop_, which begins:

> UP PUP - Pup is up. 
> CUP PUP - Pup in cup. 
> PUP CUP - Cup on pup.

The lines of Dr. Seuss, of course, are intentionally dizzying. My son loves to get lost in the pattern of the words and giggles at the absurdity of its rhythm. But, reading dizzying code at 5pm isn't that fun. For this, we need to make some name improvements.

The most immediate, impactful name change we can make is with the name of the `systemTimeZone` object. First, it appears four times within the method. No other similarly named construct appears more than twice. Also, because its reach is small (only scoped to the `foreach` loop), we can get away with a less descriptive name without doing much harm to the reader’s understanding. 

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

I like this change already. Now, the details of the `foreach` loop are much easier to scan. In addition, all of the other similarly-named constructs benefit. They're given more room such that the similarity in their names aren’t as distracting on the eyes.

The variable `tz` is also sized appropriately. Conceptually, a small name like `tz` feels like just one element in a longer-named collection like `systemTimeZones`. These are the kinds of subtle visual cues that all lend themselves to good code shape.

When we edit names, we shouldn't look at them in isolation. That habit can drive naming decisions that don’t actually benefit the overall readability or scannability of the surrounding code.

Instead, look at the context in which these names live. Find out what’s making a section of code difficult to read and solve the larger problem. It might be a more descriptive variable name, but it might also be a terse one. Let the full context drive those decisions.

When editing prose, we read whole sentences and paragraphs to get a sense of readability and style. It’s the same in code-writing.
