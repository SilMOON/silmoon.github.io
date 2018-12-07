---
layout: post
title: Add two numbers in two reversed singly linked list
categories: [Algorithm,Java]
tags: [Algorithm,Java]
fullview: true
---

Given two non-empty linked lists representing two non-negative integers in reverse order. Also, each node contains a single digit. The question is to add the two numbers and return the result in reverse order in another linked list. For instance, as the example given by LeetCode, here's the input: ``(2 -> 4 -> 3) + (5 -> 6 -> 4)`` and the output should be ``7 -> 0 -> 8`` because 342 + 465 = 807.

Here's the definition for singly-linked list:
```java
public class ListNode {
  int val;
  ListNode next;
  ListNode(int x) { val = x; }
 }
```

My very beginning thought when I saw the question was that we can read the two linked lists and reversely store them in two String variables, then read those strings as integers and add them. After that, we can store the result in a new linked list. However, this approach will cause high time complexity and is not easy to implement. So we can simply simulate how numbers add. And due to the reverse order of digits, it's easier to add numbers from the last digits of intended numbers which are safe for carrying. The code below is my solution:
```java
class Solution {
    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(0);            // this line is to simplify the head
        ListNode ans = head;                        // this line is creating pointer instead of normal assignment
        int tempVal = l1.val + l2.val;
        boolean isGTTen = false;
        if (tempVal >= 10) {
            isGTTen = true;
            tempVal -= 10;
        }
        ans.next = new ListNode(tempVal);
        ans = ans.next;
        while (l1.next != null || l2.next != null) {
            l1 = (l1.next != null) ? l1.next : new ListNode(0);
            l2 = (l2.next != null) ? l2.next : new ListNode(0);
            if (isGTTen) {
                l1.val += 1;
                isGTTen = false;
                if (l1.val ==10) {
                    l1.val = 0;
                    isGTTen = true;
                }
            }
            tempVal = l1.val + l2.val;
            if (tempVal >= 10) {
                isGTTen = true;
                tempVal -= 10;
            }
            ans.next = new ListNode(tempVal);
            ans = ans.next;
        }
        if (isGTTen) {
            ans.next = new ListNode(1);
        }
        return head.next;
    }
}
```

It's a really interesting question and definitely help me to understand more about pointer in Java.
