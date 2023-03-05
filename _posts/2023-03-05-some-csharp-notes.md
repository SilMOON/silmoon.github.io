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

2. Interpolated strings examples:
```C#
Console.WriteLine($"Interpolated strings example: {testValue}");
// Use colon and a format string to format
string s = $"255 in hex is {byte.MaxValue:X2}"; // X2 = 2-digit hexadecimal
// When using  a colon for another purpose, wrap the entire expression in parentheses
Console.WriteLine($"This is a ternary conditional operator in interpolated strings example: {(testObj.Method() ? "result1" : "result2")}");
  
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

char[] lastThree = vowels [2..]; // 'c', 'd', 'e'
// Indices can be used in range as well
char[] lastThreeVersionTwo = vowels [^3..]; // 'c', 'd', 'e'
```