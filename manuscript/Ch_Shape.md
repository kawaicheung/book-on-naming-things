## Considering Shape {#ch_shape}

When we talk about _good design_ in software, we traditionally mean something that isn't immediately visible, like good architecture. But, code is utterly visual. Good design also means code that -- for lack of a more technical term -- _looks nice_.

Good code has a certain kind of shape that you can sense. It's spaced and cut nicely. 
I've found that code that's shaped well is usually written well. Code that looks unorderly usually _is_ unorderly. Consider this simple `for` loop.

```C#
for (int i=0; i < tokens.length; i++)
{
  if (tokens[i].Activated)
  {
    usedTokens.Add(tokens);
  }
}
```

To me, this code has good shape. The indents and spacing are consistent. Even further, the  important variables are commensurate with their size. When I read this code, it doesn't take me long to figure out that tokens are added to a `usedTokens` list if they are activated. All the code that helps me understand this isn't getting in the way.

Thus far, I've championed the idea of clarity in names. If clear names are better names, than the variable `i` is a really bad one. Judging this variable solely on its descriptiveness, it seems like we could do better.

What might a more meaningful replacement for `i` be? If its job is to hold the current index of the `tokens` array, then `curIndexOfTokenArray` certainly does the trick. Let's make the replacement:

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

There's no doubt the name is more clear. But, now we face other, more onerous problems. The whole thing just _looks_ substantially more complex. We need more time to digest it. The code's lost all of its good shape.

While the current index is a critical anchor of a `for` loop, representing it in a verbally meaningful way isn’t. For one thing, an index is common to _every_ kind of `for` loop. In addition, it's scope is small -- it only exists for the duration of a few lines inside the loop. If someone were really confused about the variable name, they only need to look around a small visual radius to get familiar.

Instead, we should emphasize the variables that are the reason for this `for` loop's existence in the first place. 

The stars of the show here are really the `tokens` and `usedTokens` objects. Adding more description to `i`  brings a stage crew member unwillingly into the spotlight. That's particularly evident in the line `if (tokens[curIndexOfTokenArray].Activated)`.  I find myself _reading_ this line of code to understand it, rather than quickly _scanning_ it.

Going back to the original version is a real sight for sore eyes, isn't it?

```C#
for (int i=0; i < players.length; i++)
{
   If (players[i].Activated)
   {
     usedPlayers.Add(players);
   }
}
```

Here’s another example. In this code snippet, `systemTimeZones` represents a collection of `TimeZone` objects. We loop through the collection, extracting time zone information to build a list of `DropDownComponents`, marking the passed in time zone as selected:

```C#
public List<DropDownComponent> BuildTimeZonesDropDownList(string selectedTimeZone)
{
  var result  = new List<DropDownComponent>();
  var systemTimeZones = TimeZoneInfo.GetSystemTimeZones();

  foreach (var systemTimeZone in systemTimeZones)
  {
    var component = new DropDownComponent(systemTimeZone.Id, systemTimeZone.DisplayName);

    if (systemTimeZone.Id == selectedTimeZone)
    {
      component.Selected = true;
    }

    result.Add(component);
  }
}
```

There are several constructs in this method that have similar looking names: the `systemTimeZones` collection, the `systemTimeZone` object scoped in the `foreach` loop, the `selectedTimeZone` parameter, and the `GetSystemTimeZones()` method call. 

Scenarios like these are tricky because each name -- in isolation -- is appropriately descriptive. No single name is egregiously lengthy or over detailed. The problem, however, is when we step back and read the method as a whole.

It reminds me a lot of the lines of a Dr. Suess story -- one of my two-year-old son’s favorites is _Hop On Pop_, which begins:

> UP PUP - Pup is up. 
> CUP PUP - Pup in cup. 
> PUP CUP - Cup on pup.

The lines of Dr. Suess, of course, are intentionally dizzying -- it’s fun to get lost in the words as my son cackles at the absurdity of its rhythm. But, reading dizzying code at 5pm isn't. For this, we need to make some name improvements.

The most immediate, impactful name change we can make is with the `systemTimeZone` object. First, it appears four times in the method. No other similarly named construct appears more than twice. Also, because it’s scope is small (only to the `foreach` loop), we can get away with a less descriptive name without doing much harm to the reader’s understanding. 

Let’s try reducing `systemTimeZone` to something really barebones, like s:

```C#
public List<DropDownComponent> BuildTimeZonesDropDownList(string selectedTimeZone)
{
  var result  = new List<DropDownComponent>();
  var systemTimeZones = TimeZoneInfo.GetSystemTimeZones();

  foreach (var s in systemTimeZones)
  {
    var component = new DropDownComponent(s.Id, s.DisplayName);

    if (s.Id == selectedTimeZone)
    {
      component.Selected = true;
    }

    result.Add(component);
  }
}
```

I like this change already. Now, the details of the `foreach` loop are easier to scan. In addition, all of the other similarly-named constructs all benefit. They feel more spread apart from each other so that the similarity in their names aren’t as distracting on the eyes.

When we edit names, we shouldn't look at them in isolation. That habit can drive naming decisions that don’t actually benefit the overall readability or scannability of the surrounding code.

Instead, look at the context in which these names live. Find out what’s making a section of code difficult to read and solve the larger problem. It might be a more descriptive variable name, but it might also be a terse one. Let the full context drive those decisions.

When editing prose, we read whole sentences and paragraphs to get a sense of readability and style. It’s the same in code-writing.

