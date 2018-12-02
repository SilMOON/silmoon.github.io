---
layout: post
title: Maximum sum of non-consecutive subsequence
categories: [Algorithm,Java]
tags: [Algorithm,Java]
fullview: true
---

Today I saw a question that to calculate the maximum sum of the non-consecutive subsequence of a list. An important restriction is that the subsequence must be non-consecutive in the original list. For example, here's a list [3,5,6,10], all non-consecutive subsequences in this case are [3,6], [5,10] and [3,10]. Therefore the maximum sum of non-consecutive subsequence in this case is 5 + 10 = 15.<br><br>
In order to solve this problem, we can use brutal method to calculate sums of all non-consecutive subsequences which is neither easy to implement nor efficient. An interesting fact of this problem is that if we go through the original list from the first element, every position has a maximum sum from the first element to this position. And let's say the position is i which is greater than or equal to 2, so that the maximum sum here is either ``max(maxSum(list[i-1]), list[i])`` or ``maxSum(list[i-2]) + list[i]``. As we can see here, it's quite easy to solve using a dynamic programming method.<br><br>
Here's the code for this question:

```java
class Solution {
  public static int maxNonConsecSum(int[] array) {
      int[] sumList = array.clone();
      sumList[1] = Math.max(sumList[0], sumList[1]); //base case
      for (int i = 2; i < array.length; i++){
          sumList[i] = Math.max(Math.max(sumList[i], sumList[i-1]), sumList[i] + sumList[i - 2]);
      }
      return sumList[sumList.length-1];
  }
}
```
