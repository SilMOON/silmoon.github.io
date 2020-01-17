---
layout: post
title: Some Java notes
categories: [Java]
tags: [Java, Notes]
fullview: true
---

1. The `this` keyword has two meanings: to refer a field of the implicit parameter and to call another defined constructor in the same class.
The `super` keyword also has two meanings: to call a superclass constructor and call a superclass method. Like using `this` keyword to call a constructor, the constructor call must be the first statement in the new constructor. In the following example, `Manager` is the subclass of `Employee`, the `Employee` class which is hidden for simplicity does not have the field `bonus`:
```java
public Manager(String name, double salary, int year, int month, int day)
{
super(name, salary, year, month, day); //is the same as Employee(name, salary, year, month, day)
bonus = 0;
}
```
Likewise, the `this` keyword can be used in a similar way.