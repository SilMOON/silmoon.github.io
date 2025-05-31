---
layout: post
title: A simple DP example
categories: [Algorithm]
tags: [Algorithm, CSharp]
fullview: true
---

DP code sample.  
Just a quick refresh.  

```c#
class Program
{
  static void Main()
  {
    int goal = 97;
    int[] coins = {10, 25, 5, 1};

    int []table = new int[goal + 1];

    table[0] = 0;

    for (int i = 1; i <= goal; i++)
    {
      table[i] = int.MaxValue;
    }

    for (int j = 1; j <= goal; j++)
    {
      for (int k = 0; k < coins.Length; k++)
      {
        int selectedCoin = coins[k];
        if (selectedCoin <= j)
        {
          table[j] = int.Min(table[j], table[j - selectedCoin] + 1);
        }
      }
    }

    foreach (var elem in table)
    {
      Console.Write(elem);
      Console.Write(" ");
    }
    Console.WriteLine();
    Console.WriteLine(table.Last());

  }
}
```

