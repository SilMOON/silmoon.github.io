---
layout: post
title: The single number problem in Leetcode
categories: [Algorithm,Java]
tags: [Algorithm,Java]
fullview: true
---

I'm doing some algorithm practices recently in Leetcode. This post is about the "Single Number" problem and my attempts to the answer.<br><br>
In this problem, you are given an array with some int numbers in pair except one single number and you need to find the single number. The tricky part I think is that you can't delete an element from an array directly in java so you need to fine another approach to make the single element more "outstanding".<br><br>
My first attempt is quite straightforward (and stupid): use a for loop to find the maximum number in the given array and plus it by 1 to generate an invalid number, then use another 2 loops to find elements in pair and turn them into this invalid number. At last, use another loop to find the element which doesn't equal to the invalid number to get the result. This approach did solve the problem, but the speed is pathetically slow and unsurprisingly, it failed the speed test.<br><br>
My second attempt got rid of the loop for generating invalid number and the loop for finding the element which is not "invalid". Instead, I use a variable named "counter", the counter will become 1 when an element has a duplicate when its "duplicate checking" loop ends, and the counter will be reset to 0 before next checking loop. If an element doesn't have a duplicate, the counter will still be 0 and the answer will be nums[i], then the program will jump out of the loop directly. Here's this version's solution:

```java
class Solution {
    public int singleNumber(int[] nums) {
        int answer = 0;
        int counter = 0;

        for (int i = 0; i < nums.length; i++){
            for (int j = 0; j < nums.length; j++){
                if (nums[i]==nums[j] && i!=j){
                    counter++;
                }
            }
            if (counter == 1){
                counter = 0;
            } else {
                answer = nums[i];
                break;
            }
        }

        return answer;
    }
}
```

<br><br>
This solution passed the time limited test in most cases but still failed sometimes. Obviously this solution is not fast enough because the two loops for duplicate checking. But I couldn't figure out a better solution. So I checked the discussion posts of this problem and find a much more better solution than mine:
```java
    int ans =0;
    
    int len = nums.length;
    for(int i=0; i<len; i++)
        ans ^= nums[i];
    
    return ans;
```
<br><br>
This solution uses only one loop and the way it check unduplicated element is using XOR. The main idea is that 0^N==N and N^N==0. But I'm still confused about the mechanism of executing order of XOR in java. I'll update when I figure it out.
<br><br>
