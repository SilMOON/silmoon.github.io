---
layout: post
title: The majority element problem
categories: [Algorithm,Java]
tags: [Algorithm,Java]
fullview: true
---

This post is about the "Majority Element" problem and my attempts to it.<br><br>
In this problem, you are given an array of size **n** with some int numbers and you need to find the "majority element" which appears more than **n/2** times.<br><br>
My first attempt is intuitive: use two loops to traverse the whole array and an if statement and a counter variable to find the majority element. This approach did solve the problem, but wasn't fast enough to pass the speed test. Here's the code:

```java
class Solution {
    public int majorityElement(int[] nums) {
        int counter = 1;
        int answer = 0;
        if (nums.length==1){
            answer = nums[0];
        } else{
            for (int i = 0; i < nums.length; i++){
                for (int j = i+1; j < nums.length; j++){
                    if (nums[i]==nums[j]){
                        counter++;
                        if (counter > nums.length/2){
                            answer = nums[j];
                            break;
                        }
                    }
                }
                counter = 1;
            }            
        }
        
        return answer;
    }
}
```

I didn't figure out a better solution so I turned to the solution tab. And I saw some good approaches: <br><br>
**I. Sorting** <br><br>
The main idea of this approach is that if an element appears more than n/2 times, if we sort it in an monotonically increasing (or decreasing) order, the element in the middle (the index is (n/2) or (n/2 + 1), depends on the length of the array is odd or even) must be this majority element. So the solution will be very simple:

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}
``` 
<br><br>
**II. HashMap**<br><br>




<br><br>