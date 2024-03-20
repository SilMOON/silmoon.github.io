---
layout: post
title: Some C# notes
categories: [CSharp]
tags: [CSharp, Notes]
fullview: false
---

1. Use escape sequences for special signs such as `\n`. Or use a verbatim statement
```c#
// Escape sequences example
Console.WriteLine("I need to use escape sequences for special signs such as \\ here.");
// Verbatim statement example
Console.WriteLine(
  @"
  My name is ""SilMOON"" and this is a verbatim statement.
  I can use special signs such as \ here.
  "
  );
```
Noted that C#11 introduced raw string literals which is more convenient. I'm actually suprised that they didn't add this feature earlier. :)

2. Interpolated strings examples:
```c#
Console.WriteLine($"Interpolated strings example: {testValue}");
// Use colon and a format string to format
string s = $"255 in hex is {byte.MaxValue:X2}"; // X2 = 2-digit hexadecimal
// When using  a colon for another purpose, wrap the entire expression in parentheses
Console.WriteLine($"This is a ternary conditional operator in interpolated strings example: {(testObj.Method() ? "result1" : "result2")}");
// 
// Interpolated strings must complete on a single line, unless you also specify the verbatim string operator:
Console.WriteLine(
  $@"
  This is verbatim statement with interpolation statement.
  For example, {lucy.name}'s name can be invoked here.
  "
  );
```

3. Indices can be used to get elements from the end of an array, with the `^` operator. `^1` refers to the last element, `^0` equals the length of the array so will exceed the max index and cause an error:
```c#
char[] alphabet = new char[] {'a','b','c','d','e'};
char firstElement = alphabet[0];
char lastElement = alphabet[^1]; // 'e'
// Or declare a Index variable
Index secondToLastIndex = ^2;
char secondToLast = alphabet[secondToLastIndex]; // 'd'
```

4. Ranges can be used as `Range`, the second number in the range is exclusive:
```c#
char[] firstTwo = alphabet [..2]; // 'a', 'b'
// Declare a Range variable can do the same
Range firstTwoRange = 0..2;
char[] firstTwoVersionTwo = vowels [firstTwoRange]; // 'a', 'b'
// 
char[] lastThree = vowels [2..]; // 'c', 'd', 'e'
// Indices can be used in range as well
char[] lastThreeVersionTwo = vowels [^3..]; // 'c', 'd', 'e'
```

5. Passing a reference-type argument by value copies the reference but not the object. (If it's just a value-type argument, it will just copy the value.)
```c#
StringBuilder test = new StringBuilder();
Foo(test);
Console.WriteLine(test.ToString()); // print: abcd
// test and fooTest are both just reference point to the same object
static void Foo(StringBuilder fooTest)
{
  fooTest.Append("abcd");
  // In this case, fooTest as a reference become null but doesn't affect the stringBuilder object
  fooTest = null;
}
```
6. Different from `5`, `ref` can be used to pass by reference.
```c#
int x = 8;
Foo(ref x);
Console.WriteLine(x);
// x is now 9
static void Foo(ref int p)
{
  p = p + 1;
  Console.WriteLine(p);
}
```
7. Modifier `out` and `in` can be useful in some situations.
8. Modifier `params` allows the method to accept any number of arguments of a particular type.
```c#
int total = Sum(1, 2, 3, 4); // 10
// equivalent to:
int total2 = Sum(new int[] { 1, 2, 3, 4 });
// The parameter type must be declared as array
int Sum(params int[] intList)
{
  int sum = 0;
  foreach (var item in intList)
  {
    sum += item;
  }
  return sum;
}
```
9. Methods / constructors etc. can declare optional parameters. A parameter is optional if it specifies a default value.
```c#
Foo(); // 6
void Foo(int x = 6) { Console.WriteLine(x); }
```
Optional parameters cannot be marked with `ref` or `out`.
Mandatory parameters must occur before optional parameters in both the method declaration and the method call (the exception is with params arguments, which still always come last).
10. `ref locals` and `ref returns` can be used in micro-optimization scenarios.
11. C# will perform a `defensive copy` if we use struct under a readonly context but doesn't mark the struct (or members involve) as readonly as well.
12. Target-typed `new` expressions example:
```c#
System.Text.StringBuilder sb = new ("Test");
// equivalent to
System.Text.StringBuilder sb = new System.Text.StringBuilder ("Test");
```
13. The null-coalescing operator example:
```c#
// Null-Coalescing operator
string s1 = null;
string s2 = s1 ?? "nothing"; // "nothing"
// Null-Coalescing assignment operator
int? test(int? x)
{
  x ??= 3; // x will become 3 if it is null
  return x;
}
```
14. By using Null-Conditional Operator (“Elvis” operator), if the operand on the left is null, the expression evaluates to null instead of throwing a NullReferenceException:
```c#
System.Text.StringBuilder sb = null;
string s = sb?.ToString(); // No error; s instead evaluates to null
// equivalent to
string s = (sb == null ? null : sb.ToString());
```
15. `switch` statement can switch on not only constants but also types:
```c#
// switch on constants
void constantSwitchTest(object constantExample)
{
  switch (constantExample)
  {
    case 1:
      Console.WriteLine("One or Negative");
      break;
    case 2:
      Console.WriteLine("Two");
      break;
    case 0:
    case -1:
      goto case 1;
    default:
      Console.WriteLine(constantExample);
      break;
  }
}
// switch on types
void typeSwitchTest(object typeExample)
{
  switch (typeExample)
  {
    case string s:
      Console.WriteLine($"String: {s}.");
      break;
    case float f when f > 1000:
    case double d when d > 1000:
    case decimal m when m > 1000:
      Console.WriteLine($"We can refer to {typeExample} here but not f or d or m");
      break;
    // can use `_` if don't care the value of the variable
    case int _:
      Console.WriteLine("It's an int.");
      break;
    // predicate a case with the `when` keyword
    // Fires only when b is true
    case bool b when b == true:
      Console.WriteLine("True!");
      break;
    case bool b:
      Console.WriteLine("False!");
      break;
  }
}
```
16. Switch expressions:
```c#
// The case clauses here are expressions (terminated by commas) rather than statements
string cardName = cardNumber switch
{
  13 => "King",
  12 => "Queen",
  11 => "Jack",
  _ => "Pip card" // equivalent to 'default'
};
// Can switch on multiple values (the tuple pattern)
string result = (numberAsInt, numberAsString) switch
{
  (1, "one") => "test1",
  (2, "two") => "test2",
  ...
};
```
17. Namespace examples:
```c#
namespace Outer.Middle.Inner
{
  class Class1 { }
  class Class2 { }
}
// equivalent to
namespace Outer
{
  namespace Middle
  {
    namespace Inner
    {
      class Class1 { }
      class Class2 { }
    }
  }
}
```
18. If you want all the types in a file to be defined in one namespace, a `file-scoped namespace` is simpler:
```c#
namespace MyNamespace; // Applies to everything that follows in the file
class Class1 {} // inside MyNamespace
class Class2 {} // inside MyNamespace
```
19. A class or struct may overload constructors. To avoid code duplication, one constructor can call another, using the this keyword:
```c#
public class Wine
{
  public decimal Price;
  public int Year;
  // Constructor
  public Wine(decimal price)
  {
    Price = price;
  }
  // Calls the previous constructor to avoid code duplication
  public Wine(decimal price, int year) : this(price)
  {
    Year = year;
  }
  // You can also pass an expression into another constructor
  public Wine (decimal price, DateTime year) : this (price, year.Year) {}
}
```
20. Constructors do not need to be public:
```c#
public class Class1
{
  // Private constructor
  Class1() {}
  // Create instance using static method call instead
  public static Class1 Create (...)
  {
    // Perform custom logic here to return an instance of Class1
  }
}
```
21. As an approximate opposite to a constructor, a deconstructor typically assigns fields back to a set of variables:
```c#
class Rectangle
{
  public readonly float Width, Height;
  // Constructor
  public Rectangle(float width, float height)
  {
    Width = width;
    Height = height;
  }
  // The constructor above is equivalent to
  public Rectangle (float width, float height) => (Width, Height) = (width, height);
  // Deconstruct must be called Deconstruct and have one or more out parameters
  public void Deconstruct(out float width, out float height)
  {
    width = Width;
    height = Height;
  }
}
```
22. To call the deconstructor, we can use the following syntax:
```c#
var rect = new Rectangle (3, 4);
// Deconstruction
(float width, float height) = rect;
// 3 4
Console.WriteLine (width + " " + height);
// The deconstruction part above is equivalent to
float width, height;
rect.Deconstruct(out width, out height);
// equivalent to
float width, height;
(width, height) = rect;
// equivalent to
rect.Deconstruct (out var width, out var height);
// equivalent to
(var width, var height) = rect;
// equivalent to
var (width, height) = rect;
// use `_` if don't care one or more variables
var (_, height) = rect;
```
23. Any accessible fields or properties of an object can be set via an `object initializer` directly after construction, no need to set each fields separately:
```c#
public class Bunny
{
  public string Name;
  public bool LikesCarrots;
  public bool LikesHumans;
  public Bunny () {}
  public Bunny (string n) { Name = n; }
}
// Note parameterless constructors can omit empty parentheses
Bunny b1 = new Bunny { Name="Bo", LikesCarrots=true, LikesHumans=false };
Bunny b2 = new Bunny ("Bo") { LikesCarrots=true, LikesHumans=false };
```
24. Comparing with `field`, a `property` gives more control on how to get/set:
```c#
public class Stock
{
  // Private "backing" field
  decimal currentPrice;
  // The public property
  public decimal CurrentPrice
  {
    get => currentPrice;
    // Can put some validation and throw a custom exception if needed
    set { currentPrice = value; }
  }
}
```
25. `Property initializer` can be added to automatic properties:
```c#
public class Stock{
  public decimal CurrentPrice { get; set; } = 123;
  public int Maximum { get; } = 999;
  // ...
}
```
26. The get and set accessors can have different access levels:
```c#
var a = new Foo();
a.Test(16.121m); // a.X will be 16.12
public class Foo
{
  private decimal x;
  // X should be public here because one of the getter/setter is public
  public decimal X
  {
    // public getter
    get { return x; }
    // private setter
    private set { x = Math.Round(value, 2); }
  }
  //Use the private set
  public void Test(decimal input) => X = input;
}
```
27. Init-only setters:
```c#
// Can be set once via an object initializer
var note = new Note { Pitch = 50 };
public class Note
{
  // “Init-only” property
  public int Pitch { get; init; } = 20;
  // “Init-only” property
  public int Duration { get; init; } = 100;
}
```
28. Adding an `optional parameter` to the constructor can basically achieve the same effect comparing with `object initializer`. However, `optional constructors` can break binary compatibility in some cases (especially for public libraries).
29. To write an `indexer`, define a property called this, specifying the arguments in square brackets
```c#
Sentence s = new Sentence();
// fox
Console.WriteLine (s[3]);
s[3] = "kangaroo";
// kangaroo
Console.WriteLine (s[3]);
class Sentence
{
  string[] words = "The quick brown fox".Split();
  public string this [int wordNum]
  {
    get { return words [wordNum]; }
    set { words [wordNum] = value; }
  }
}
```
30. A type can declare multiple indexers, each with parameters of different types. An indexer can also take more than one parameter:
```c#
public string this [int arg1, string arg2]
{
  get { ... } set { ... }
}
```
31. Read-only indexer can be simplified as expression-bodied syntax, range can be supported by defining a range to `indexer`:
```c#
Sentence s = new Sentence();
// fox
Console.WriteLine (s [^1]);
// (The, quick)
string[] firstTwoWords = s [..2];
class Sentence
{
  string[] words = "The quick brown fox".Split();
  public string this [int wordNum] => words [wordNum];
  // Define range
  public string[] this [Range range] => words [range];
}
```
32. A static constructor executes once per type rather than once per instance. A type can define only one static constructor, and it must be parameterless and have the same name as the type:
```c#
class Test
{
  static Test() { Console.WriteLine ("Type Initialized"); }
}
```
33. `Module initializers` which execute once per assembly (when the assembly is first loaded) was introduced from C# 9. To define a module initializer, write a static void method and then apply the `[ModuleInitializer]` attribute to that method:
```c#
[System.Runtime.CompilerServices.ModuleInitializer]
internal static void InitAssembly()
{
  // ...
}
```
34. `Finalizers` are class-only methods that execute before the garbage collector reclaims the memory for an unreferenced object:
```c#
class Class1
{
  // Name of the class prefixed with the ~ symbol
  ~Class1()
  {
    //...
  }
}
```
35. `Partial` classes and methods: https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods 
36. Be careful about type when using patterns with numeric constants
```c#
// Decimal
object obj = 2m;
// True
Console.WriteLine(obj is < 3m);
// False, because type doesn't match (integer)
Console.WriteLine(obj is < 3);
```
