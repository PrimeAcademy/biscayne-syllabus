# C# Review, Q&A, and Classes Intro

The purpose of this time is to step through some of the language basics, provide comparison to JS, and answer questions on fundamentals.

Write the following code, describing as you go. We're going to build up, explore/reinforce data types, compile errors and generally working in C#.

## Note

Some tutorials they may have found relied heavily on `static` methods to work. AVOID this. Tell them not to rely on static (it means roughly "global"). This is why we tell them to wait to tackle classes until after this lecture!


## Lecture

Cover the following in a file, compiling and running.

- basic datatypes
- .NET List generic collection
- class examples

Setup:

- Create a new console app: `dotnet new console -o Test`
- Open app in VSCode, update the `Program.cs` file
- Run code with `dotnet run`


```csharp
// Program.cs

// how to console log stuff
Console.WriteLine("Hello, World!");

// primitive data types, explore a bit
int age = 44;
float price = 1.99F;
char letter = 'a';
string name = "Kris";

// built-in arrays are immutable and rather useless compared to JS
int[] numberArray = {1, 2, -4, 3};
string[] stringArray = {"Mitchison", "Ramirez"};
// can't log stuff! to .ToString() on any data type.
Console.WriteLine("here is my array" + numberArray);

// loops
// foreach built in
foreach(string cohort in stringArray) {
    Console.WriteLine(cohort);
}

// .NET gives us a List! 
// This is the JS array you expected
// Function/DataType <data type>
List<string> shoeList = new List<string>();
shoeList.Add("Nike");
shoeList.Add("Keds");
shoeList.Add("Crocs");
shoeList.Add("New Balance");
// shoeList[2];

Console.WriteLine(shoeList);
foreach(string shoe in shoeList) {
    Console.WriteLine(shoe);
    // expressions
}

// A list of constants is called an ENUM (enumerated data)
// Use enums when you have values that you know aren't going to change, like month days, days, colors, deck of cards, etc.
// Dropdown options could be considered ENUM

enum Level 
{
  Low,
  Medium,
  High
}


  Level myVar = Level.Medium;
  switch(myVar) 
  {
    case Level.Low:
      Console.WriteLine("Low heat level");
      break;
    case Level.Medium:
      Console.WriteLine("Medium heat level");
      break;
    case Level.High:
      Console.WriteLine("High heat level");
      break;
    default:
      Console.WriteLine("NO TEMP FOUND");
  }


```

## Class Syntax Intro

The goal next is to get them looking at class syntax and organization. It is NOT to delve deeply into OOP, class hierarchies, all the access levels etc. Get them seeing it and trying to understand the idea of an "object" described in code.

1. A class is a grouping of code. It serves as a template for 1 kind of object.
2. From this class we can make "instances" or actual variables in our code. Each one is a copy of that template, with its own specifics.
3. Each template can have properties (state, data) and methods (functions, things it can do)

## Real World Example

I like to use a Marker, or Food item. Make it concrete and easily relatable. Talk thru describing the physical properties.

Then layer in what it can do.

Then code it up as a

Example:

```csharp
// In Marker.cs

// classes are object templates. The define what the object is, what it can do.
public class Marker 
{
    // properties/fields
    public string material;
    private string color;
    public int inkLevel = 100;

    // methods/functions
    // allows us to change a private property
    // setter
    public void changeColor(string newColor) {
        if(newColor != null) {
            this.color = newColor;
        }
    }

    // getter
    public string getColor() {
        return this.color;
    }

} // end class
```

```csharp
// In Program.cs

// make instances of markers (variables)
// new creates an instance of a class, or an Object!
Marker redMarker = new Marker();
// redMarker.color = "Red";
redMarker.changeColor("Red"); // .color is private!
Console.WriteLine(redMarker.getColor());


```



More full example with some inheritance.

```csharp
Hotdog chicago = new Hotdog(); // create an instance of this class
Hotdog corndog = new Hotdog(); // create an instance of this class

Console.WriteLine(chicago.GetEnergy()); // 20

chicago.TakeBite();

Console.WriteLine(chicago.GetEnergy()); // 15
Console.WriteLine("our corndog energy is: " + corndog.GetEnergy()); // 20

Console.WriteLine(chicago); // 15

// classes
// public, private, internal, readonly, static, virtual, abstract
public class Hotdog
{
    // fields
    int energy; // private
    string meat = "beef";
    float temperature;
    // Color color;
    Hotdog seconds;

    // constructor
    public Hotdog() {
        Console.WriteLine("constructed a hotdog object");
        energy = 20;
    }

    // methods
    public void TakeBite() {
        energy -= 1;
    }

    // getter to access the private data
    public int GetEnergy() {
        return energy;
    }

}

public class ChicagoDog : Hotdog
{
    // inherited
    public int sportPeppers = 10;

    void TakeBite() {
        // stuff
    }
}

```
