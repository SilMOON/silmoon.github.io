---
layout: post
title: Jewels and stones problem
categories: [Algorithm,Java]
tags: [Algorithm,Java]
fullview: true
---

This post is about the "jewels and stones" problem and my attempt to it.<br><br>
The purpose is to find elements of string "J" in string "S", it's like finding some type of jewels in a pile of stones.<br><br>
For example, here's the input: J="aA", S="aAAAbB", so the output should be 4 in this case.<br><br>
This is a pretty easy one, so I just put my code below. Although the speed of my program is not the fastest, I can't check the official answer because I don't have a premium subscription.

```java
class Solution {
    public int numJewelsInStones(String J, String S) {
        int num=0;
        for (int i=0, l=S.length(); i<l; i++){
            char c = S.charAt(i);
            for (int j=0, k=J.length(); j<k; j++){
                if(c==J.charAt(j)){
                    num++;
                    break;
                }
            }
        }
        return num;
    }
}
```
