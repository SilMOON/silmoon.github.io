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

2. If a subclass reference is assigned to a superclass variable, you are promising less, and the compiler will simply let you do it, for example:
```java
Employee e;
e = new Manager(. . .); 
```
If a superclass is assigned to a subclass variable, you are promising more:
```java
Manager e;
e = new Employee(. . .);  //this doesn't work!
```
In this case, you must use a cast so that your promise can be checked at runtime.
Now consider the following situation:
```java
Manager boss = new Manager(...);
boss.setbonus(5000);

var staff = new Employee[3];
staff[0] = boss; //this works
staff[1] = new Employee(...);
staff[2] = new Employee(...);
```
However, staff[0] will be an `Employee` instance whose actual type (`Manager` in this case) has been temporarily forgotten and we cannot do `staff[0].setbonus(xxx)` (although we can still do `boss.setbonus(xxx)`). To convert it into `Manager` again, we can cast it like this: `Manager xx = (Manager) staff[0]`. And this should be the only case we want to make a cast for an object: to use an object in its full capacity after its actual type has been temporarily forgotten.
