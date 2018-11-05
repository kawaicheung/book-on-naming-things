# Refactoring Through Renaming

If you read books about programming, I’ll bet you have Martin Fowler’s _Refactoring_ book sitting on your bookshelf somewhere. It’s the veritable _Joy of Cooking_ of classic refactoring recipes. When you take a look at his extensive list of refactorings, many of them are - at the core - exercises in renaming. 

"Introduce Explaining Variable", "Replace Magic Number with Symbolic Constant," and "Rename Method" are some of the obvious ones, but nearly all of them rely on a well-considered name to play a key role in the success of the refactoring.

In my view, refactoring and renaming go hand-in-hand: I often complete a series of refactorings by not even knowing it - it’s through the practice of renaming things where I suddenly find myself executing one (or several) of Martin’s recipes.

Here’s a working method on a project that I revisited recently (I’ve removed some of the bits that aren’t helpful to the discussion).  We’ll dive into the details later, so for now, just take a minute to scan the method. 

```C#
public static string GetAvatarHtmlWithInitialsFallback(
	string avatar_url, 
	string first_name, 
	string last_name)
{
  StringBuilder avatar_html = new StringBuilder();

  avatar_html.Append("<div class=\"avatar\">");
   
  if (string.IsNullOrEmpty(first_name) && string.IsNullOrEmpty(last_name))
  {
    avatar_html.Append(string.Format("<span title=\"no-user\">?</span>", first_initial, last_initial));
  }
  else if (string.IsNullOrEmpty(avatar_url))
  {
    string first_initial = string.Empty;
    string last_initial = string.Empty;

    if (!string.IsNullOrEmpty(first_name) && first_name.Length > 0)
    {
      first_initial = first_name.Substring(0, 1).ToUpper();
    }

    if (!string.IsNullOrEmpty(last_name) && last_name.Length > 0)
    {
      last_initial = last_name.Substring(0, 1).ToUpper();
    }

    string hex_color = getHexColorFromInitials(first_initial, last_initial);

    avatar_html.Append(string.Format(@"<span title=\"{2} {3}\" style=\"background-color:{4}\">{0}{1}</span>", 
    first_initial, last_initial, first_name, last_name, hex_color));
  }
  else
  {
    avatar_html.Append(string.Format("<img src=\"{0}\" width=\"48\" alt=\"{1} {2}\" title=\"{1} {2}\">", avatar_url, first_name, last_name));
  }

  avatar_html.Append("</div>");

  return avatar_html.ToString();
}
```

I could tell this method needed some renaming considerations just by the look of it -- the repetitive pieces, the small bits of logic that muddy conditional checks, the general onerous shape of it all. Here’s how I went about improving it.

Now, for the quick summary. `GetAvatarHtmlWithInitialsFallback()` is a method that takes in a few details about a particular person and returns a string of HTML to display that person’s avatar.

I begin my renaming session by attacking the first eye sore that pops out. For me, it’s the ugly repetition of logic in the `first_initial` and `last_initial` assignments. 

```C#
var first_initial = string.Empty;
var last_initial = string.Empty;

if (!string.IsNullOrEmpty(first_name) && first_name.Length > 0)
{
  first_initial = first_name.Substring(0, 1).ToUpper();
}

if (!string.IsNullOrEmpty(last_name) && last_name.Length > 0)
{
  last_initial = last_name.Substring(0, 1).ToUpper();
}
```

I use Fowler’s "Extract Method" recipe to DRY this logic up into a method with a meaningful name:

```C#
private static string getInitialLetter(string name)
{
  if (!string.IsNullOrEmpty(name) && name.Length > 0)
  {
    return name.Substring(0, 1).ToUpper();
  }

  return string.Empty;
}
```

I like the name `getInitialLetter` here. I can’t think of any other meaning it would have other than "Get me the initial letter of the passed-in name string". Here’s the cleanup on the main method:

```C#
public static string GetAvatarHtmlWithInitialsFallback(string avatar_url, string first_name, string last_name)
{
  StringBuilder avatar_html = new StringBuilder();

  avatar_html.Append("<div class=\"avatar\">");
   
  if (string.IsNullOrEmpty(first_name) && string.IsNullOrEmpty(last_name))
  {
    avatar_html.Append(string.Format("<span>?</span>", first_initial, last_initial));
  }
  else if (string.IsNullOrEmpty(avatar_url))
  {
    string first_initial = getInitialLetter(first_name);
    string last_initial = getInitialLetter(last_name);
    string hex_color = getHexColorFromInitials(first_initial, last_initial);

    avatar_html.Append(string.Format("<span title=\"{2} {3}\" style=\"background-color:{4}\">{0}{1}</span>", first_initial, last_initial, first_name, last_name, hex_color));
  }
  else
  {
    avatar_html.Append(string.Format("<img src=\"{0}\" width=\"48\" alt=\"{1} {2}\" title=\"{1} {2}\">", avatar_url, first_name, last_name));
  }

  avatar_html.Append("</div>");

  return avatar_html.ToString();
}
```

Next, I work on the bits of logic evaluated within the conditionals. They aren’t particularly nasty lines of code, but when the conditional logic has multiple branches, the _shape_ of the conditional quickly becomes dizzying. Replacing all the evaluations with a meaningful name ("Introduce Explaining Variable") makes it easier to read and helps corral the shape. So, I go to work on each one.

I create a variable called `no_name` and assign it to `string.IsNullOrEmpty(first_name)` && `string.IsNullOrEmpty(last_name)`.  I then create a variable called no_avatar and assign it to `string.IsNullOrEmpty(avatar_url)`. I move these variables up to the very top of the method. This pass leaves me with the following:

```C#
public static string GetAvatarHtmlWithInitialsFallback(string avatar_url, string first_name, string last_name)
{
  bool no_user = string.IsNullOrEmpty(first_name) && string.IsNullOrEmpty(last_name);
  bool no_avatar = string.IsNullOrEmpty(avatar_url);

  StringBuilder avatar_html = new StringBuilder();

  avatar_html.Append("<div class=\"avatar\">");
   
  if (no_user)
  {
    avatar_html.Append(string.Format("<span>?</span>", first_initial, last_initial));
  }
  else if (no_avatar)
  {
    string first_initial = getInitialLetter(first_name);
    string last_initial = getInitialLetter(last_name);
    string hex_color = getHexColorFromInitials(first_initial, last_initial);

    avatar_html.Append(string.Format("<span title=\"{2} {3}\" style=\"background-color:{4}\">{0}{1}</span>", first_initial, last_initial, first_name, last_name, hex_color));
  }
  else
  {
    avatar_html.Append(string.Format("<img src=\"{0}\" width=\"48\" alt=\"{1} {2}\" title=\"{1} {2}\">", avatar_url, first_name, last_name));
  }

  avatar_html.Append("</div>");

  return avatar_html.ToString();
}
```

You might object to the negative form of these booleans. But, I name these variables `not_user` and `not_avatar` here because I only plan on evaluating against the negative form. The line `if (no_user) { … }` much more readable than, say, `if (!has_user) { … }`.  The former reads like plain English whereas the latter requires the annoying mental switch (if not has user → if no user).

On the other hand, if there were any chance I would use the inverse of the booleans for a check (like `!not_user` or `!not_avatar`) I’d certainly opt to name them positively to avoid a double-negative. But, in the scope of this method, the situation doesn't come about.

I make a few more renaming moves. 

1. For clarity, I replace the hard-coded width of `"48"` on the avatar image with a typed integer named `img_width` and move it just underneath the booleans so it’s easier to update if I ever need to. (Fowler’s "Replace Magic Number with Symbolic Constant").

2. I revisit the already-extracted method `getHexColorFromInitials()`. With a fresh set of eyes, it reads oddly to me. I mean, how exactly do you get a hex color from two letters? I dive into the method and am reminded that it holds three two-hexadecimal-digit arrays each representing a piece of the hex color (the R,G, or B). Using the passed-in letters, it concatenates a member of each array to a single hex color. The details aren’t critical. But, we aren’t just "getting" a hex color from, say, a database fetch -- we’re really deriving it. I update the method to `deriveHexColorFromInitials()` to clarify what the details of the implementation. (Fowler’s "Rename Method")

3. I turn my attention to the `StringBuilder` variable `avatar_html`. There’s nothing wrong with the name itself other than that it makes an appearance seven times in the method. Because it’s the returned object of the method, a name like `result` feels better. It clearly demarcates what the responsibility of the string is. It’s easier on the eyes when repeated multiple times than the longer and more oddly shaped `avatar_html` is. At a glance, I don’t have to wonder if this is some temporary variable later being used in some other fashion.

4. At this point, I’m satisfied with the progress I’ve made with the method. I take another hard look at the method name `GetAvatarHtmlWithInitialsFallback()`. The name is a little on the wordy side so I scrutinize. Does `WithInitialsFallback` really matter to someone using this method? As it turns out, there once was another method in the class named `GetAvatarHtmlWithImageFallback()`. Sometime ago, it was wiped away leaving only one method to piece together the HTML for an avatar. So, I remove it. I also don’t like the use of Get here. Again, we’re not just retrieving some HTML. Rather, we’re constructing, building, or assembling the HTML. I choose Build because it’s the simplest sounding word. `BuildAvatarHTML()` it is. (Fowler’s "Rename Method" again)

Here’s the refactored method:

```C#
public static string BuildAvatarHTML(string avatar_url, string first_name, string last_name)
{
  bool no_user = string.IsNullOrEmpty(first_name) && string.IsNullOrEmpty(last_name);
  bool no_avatar = string.IsNullOrEmpty(avatar_url);
  int img_width = 48;

  StringBuilder result = new StringBuilder();

  result.Append("<div class=\"avatar\">");
   
  if (no_user)
  {
    result.Append(string.Format("<span>?</span>", first_initial, last_initial));
  }
  else if (no_avatar)
  {
    string first_initial = getInitialLetter(first_name);
    string last_initial = getIniitalLetter(last_name);
    string hex_color = deriveHexColorFromInitials(first_initial, last_initial);

    result.Append(string.Format("<span title=\"{2} {3}\" style=\"background-color:{4}\">{0}{1}</span>", first_initial, last_initial, first_name, last_name, hex_color));
  }
  else
  {
    result.Append(string.Format("<img src=\"{0}\" width=\"{3}\" alt=\"{1} {2}\" title=\"{1} {2}\">", avatar_url, first_name, last_name, img_width));
  }

  result.Append("</div>");

  return result.ToString();
}
```

Whenever I thoroughly rename a method or class and its internal structures, I take a step back and review how all the new names read. Each individual refactoring might feel right in isolation, but the update is only worthwhile if it makes sense in its larger context.

I like to start top-to-bottom. In this case, it means I first find references to the method and see if the new name still makes sense in practice.

As it turns out, most of the callers are from other objects which have a get property that simply calls the method using its other properties. For instance, in a `PersonForEdit` domain object:

```C#
public class PersonForEdit : PersonBase
{
  ...
  
  public string AvatarHTML
  {
    get
    {
      return HTMLBuilder.BuildAvatarHTML(AvatarURL, FirstName, LastName);
    }
  }
}
```

The name feels fine. If anything, my mind drifts toward other refactorings I could apply, like moving the method out of the static class and into a base object. That’s a good sign for me -- it means the name is no longer an issue and I can move onto other changes later.

Drilling in, I like to read the method top-down, focusing on the updates I’ve just made. Compared to the initial draft, the biggest improvements I see are as follows:

* By replacing the conditional logic with variable names and moving these to the top of the method, I get a more condensed view of all the pieces at-play, before they are used. If I need to modify them, they live closer together and neatly before the assembly of the result. I’ve separated concerns more clearly within the method (Definitions first, using the definitions later).
* The meat of the method is in the conditional logic. Overall, it reads much more coherently: "If there’s no user, do this...if there’s no avatar, do this… else if the user has an avatar...do this".
* The "no avatar" branch benefits from the same improvements as the overall method. The pieces at-play are well-named and moved away from the implementation. 
