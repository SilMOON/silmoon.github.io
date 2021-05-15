---
layout: post
title: Some Kotlin notes
categories: [Kotlin]
tags: [Kotlin, Notes]
fullview: true
---

1. Use `vararg` for variable arguments:
```kotlin
fun printLetters(vararg letters: String, count: Int): Unit {
    print("$count letters are ")
    for (letter in letters) print(letter)
    println()
}
```
Then we can use it by calling:
```kotlin
printLetters("a", "c", "q", count = 3)
```
Or use `*` to pass an array as parameters:
```kotlin
val argArray = arrayOf("q","e","w")
printLetters(*argArray, count = 3)
```

2. An example of `fold` function:
```kotlin
fun cal(list: List<Int>): Int {
    return list.fold(0){ res, el -> res * el + el }
}
```
If we call:
```kotlin
println( cal( listOf(1, 2, 3) ) )
```
It will print `15` as the process below:
0*1+1 = 1
1*2+2 = 4
4*3+3=15

3. infix function `to` returns paired data structure which is usually combined with `map`:
```kotlin
mapOf(
    1 to "one",
    2 to "two",
    3 to "three"
)
```

4. A simple infix function example:
```kotlin
class Person {
    infix fun called(name: String) {
        println("My name is $name.")
    }
}
```
To call it:
```kotlin
val p = Person()
//infix style
p called "Shaw"
//or use it as a regular function
p.called("Shaw")
```

5. Kotlin can declare abstract properties in `interface`. However, it cannot be assigned a value onto them:
```kotlin
interface Flyer {
    //WRONG
    val height: Int = 1000

    //CORRECT
    val height: Int
        get() = 1000
}
```

6. Usage of `coerceIn`:
```kotlin
println(0.coerceIn(1, 100)) // 1
println(500.coerceIn(1, 100)) // 100
```

7. Use `require` to add rules for parameters of a class in `init` block:
```kotlin
class Robot (
    val code: String,
    val battery: String? = null,
    val weight: Int? = null
) {
    init {
        require (weight == null || battery != null) {
            "Weight cannot be set with no battery assigned."
        }
    }
}
```
In this case, if we call `val robot = Robot(code="001", weight=100)`, an `IllegalArgumentException` will be thrown because the rule we set cannot be satisfied.
