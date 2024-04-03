# C# Orientation

## Start Small

Remember when we first brought our JavaScript onto the server? We used Node as a new runtime and ran some vanilla JS. We proved we could do variables, conditionals, loops, and some basic data structures in Node - just like we did in the browser.

Today is very similar.

**The goal for this first day of C# is to write and run code in a single file.**

Nobody writes full C# apps in a single file but for today, this is where we'll start.

## Things are Different Now

C# will feel somewhat similar to JS syntactically. There will be LOTS of curly braces, semi-colons are no longer optional, and it has familiar structures for conditionals and loops.

But it is very different. It cares a TON about data types. Every variable needs to be a specific data type (like Number, Boolean, etc). And once it's set, it can't change! So you'll see stuff like:

```csharp
int number = 33;
string name = "Ziggy";
char oneLetter = 'a';
bool yesOrNo = false;
```

This is okay and you'll get used to it. But C# Cares!

## Guardrails

We want to focus on the language of C Sharp (C#). In order to keep us focused here, we're going to stick to the following guidelines:

- Use the `dotnet` CLI tool to generate "console" apps
- Write code top to bottom, running and testing it as you go
- We're providing some starting tutorials for you to follow and a checklist of skills to develop (like variables, conditionals, etc.)


## Gotchas

If you run into tutorials that have code like this, just don't. We'll dive into the `class` kind of stuff later today. Don't use it for now until you get to the Stretch stuff - then go ahead and explore it. But doing so will make it hard to run things in a single file --- which is Today's Goal!

This will just trip you up. Comment it out if your console app generates it for you.

```csharp
class Program
{
    static void Main(string[] args)
    {
        System.Console.WriteLine("Hello World!");
    }
}
```