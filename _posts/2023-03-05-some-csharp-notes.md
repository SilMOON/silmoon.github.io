---
layout: post
title: Some C# notes
categories: [CSharp]
tags: [CSharp, Notes]
fullview: true
---

1. Use escape sequences for special signs such as `\n`. Or use a verbatim statement
```C#
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
```C#
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
```C#
char[] alphabet = new char[] {'a','b','c','d','e'};
char firstElement = alphabet[0];
char lastElement = alphabet[^1]; // 'e'
// Or declare a Index variable
Index secondToLastIndex = ^2;
char secondToLast = alphabet[secondToLastIndex]; // 'd'
```

4. Ranges can be used as `Range`, the second number in the range is exclusive:
```C#
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
```C#
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
```C#
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
```C#
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
```C#
Foo(); // 6
void Foo(int x = 6) { Console.WriteLine(x); }
```
Optional parameters cannot be marked with `ref` or `out`.
Mandatory parameters must occur before optional parameters in both the method declaration and the method call (the exception is with params arguments, which still always come last).
10. `ref locals` and `ref returns` can be used in micro-optimization scenarios.
11. C# will perform a `defensive copy` if we use struct under a readonly context but doesn't mark the struct (or members involve) as readonly as well.
12. Target-typed `new` expressions example:
```C#
System.Text.StringBuilder sb = new ("Test");
// equivalent to
System.Text.StringBuilder sb = new System.Text.StringBuilder ("Test");
```
13. The null-coalescing operator example:
```C#
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
```C#
System.Text.StringBuilder sb = null;
string s = sb?.ToString(); // No error; s instead evaluates to null
// equivalent to
string s = (sb == null ? null : sb.ToString());
```
