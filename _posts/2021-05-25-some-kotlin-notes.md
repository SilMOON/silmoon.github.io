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

8. Use `Delegates.observable()` to implement observer pattern, unlike Java's `Observable()` which only provide one `update()` method for overriding, `Delegates.observable()` provide 3 parameters (property itself for delegating, oldValue, newValue) which shows powerful ability to customize: 
```kotlin
interface PriceUpdateListener {
    fun onRise(price: Int)
    fun onFall(price: Int)
} 
class PriceDisplayer: PriceUpdateListener {
    override fun onRise(price: Int) {
        println("Price has risen to ${price}.")
    }
    override fun onFall(price: Int) {
        println("Price has fallen to ${price}.")
    }
} 
class PriceUpdater {
    var listeners = mutableSetOf<PriceUpdateListener>()
    var price: Int by Delegates.observable(0) { _, old, new ->    //0 is initial value, (_, old, new) are the 3 parameters mentioned above
        listeners.forEach {
            if (new > old) it.onRise(price) else it.onFall(price)
        }
    }
} 
fun main (args: Array<String>) {
    val priceUpdater = PriceUpdater()
    val priceDispalyer = PriceDisplayer()
    priceUpdater.listeners.add(priceDispalyer)
    priceUpdater.price = 50
    priceUpdater.price = 40
}
```

9. `Delegates.vetoable()` can be used when you want to make sure the assignment follows your rule before a new value is assigned to the target:
```kotlin
var value: Int by Delegates.vetoable(0) { prop, old, new ->
    new > 0
}
```
In this case:
```
>>> value = 1
>>> println(value)
1
```
```
>>> value = -1  //will be intercepted because -1 does not satisfy the rule (greater than 0)
>>> println(value)
1
```
