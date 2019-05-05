### Introduction
Given an infinite supply of ‘n’ coin denominations and a total money amount, we are asked to find the total number of 
distinct ways to make up that amount.

### Problem Statement
Given a number array to represent different coin denominations and a total amount ‘T’, we need to find all the different 
ways to make a change for ‘T’ with the given coin denominations. We can assume an infinite supply of coins, therefore, each
coin can be chosen multiple times.

This problem follows the Unbounded Knapsack pattern.

### Basic Solution
A basic brute-force solution could be to try all combinations of the given coins to select the ones that give a total
sum of ‘T’. 

```
for each coin 'c' 
  create a new set which includes one quantity of coin 'c' if it does not exceed 'T', and 
     recursively call to process all coins 
  create a new set without coin 'c', and recursively call to process the remaining coins 
return the count of sets who have a sum equal to 'T'
```

***This problem is quite similar to Count of Subset Sum. The only difference here is that after including the item (i.e. coin),
we recursively call to process all the items (including the current coin). In ‘Count of Subset Sum’, however, we were 
recursively calling to process only the remaining items.***

code for the brute-force solution:
```
class CoinChange {

  public int countChange(int[] denominations, int total) {
    return this.countChangeRecursive(denominations, total, 0);
  }

  private int countChangeRecursive(int[] denominations, int total, int currentIndex) {
    // basic checks
    if (total == 0)
      return 1;

    if(denominations.length == 0 || currentIndex >= denominations.length)
      return 0;

    // recursive call after selecting the coin at the currentIndex
    // if the coin at currentIndex exceeds the total, we shouldn't process this
    int sum1 = 0;
    if( denominations[currentIndex] <= total )
      sum1 = countChangeRecursive(denominations, total - denominations[currentIndex], currentIndex);

    // recursive call after excluding the coin at the currentIndex
    int sum2 = countChangeRecursive(denominations, total, currentIndex + 1);

    return sum1 + sum2;
  }

  public static void main(String[] args) {
    CoinChange cc = new CoinChange();
    int[] denominations = {1, 2, 3};
    System.out.println(cc.countChange(denominations, 5));
  }
}
```

The time complexity of the above algorithm is exponential O(2^{C+T}), where ‘C’ represents total coin denominations 
and ‘T’ is the total amount that we want to make change. The space complexity will be O(C+T).

### Top-down Dynamic Programming with Memoization

```
class CoinChange {

  public int countChange(int[] denominations, int total)
  {
    Integer[][] dp = new Integer[denominations.length][total + 1];
    return this.countChangeRecursive(dp, denominations, total, 0);
  }

  private int countChangeRecursive(Integer[][] dp, int[] denominations, int total, int currentIndex)
  {
    // base checks
    if (total == 0)
      return 1;

    if(denominations.length == 0 || currentIndex >= denominations.length)
      return 0;

    // if we have already processed a similar sub-problem, return the result from memory
    if(dp[currentIndex][total] != null)
      return dp[currentIndex][total];

    // recursive call after selecting the coin at the currentIndex
    // if the number at currentIndex exceeds the total, we shouldn't process this
    int sum1 = 0;
    if( denominations[currentIndex] <= total )
      sum1 = countChangeRecursive(dp, denominations, total - denominations[currentIndex], currentIndex);

    // recursive call after excluding the number at the currentIndex
    int sum2 = countChangeRecursive(dp, denominations, total, currentIndex + 1);

    dp[currentIndex][total] = sum1 + sum2;
    return dp[currentIndex][total];
  }

  public static void main(String[] args) {
    CoinChange cc = new CoinChange();
    int[] denominations = {1, 2, 3};
    System.out.println(cc.countChange(denominations, 5));
  }
}
```
### Bottom-up Dynamic Programming

```
We will try to find if we can make all possible sums, with every combination of coins, to populate the array 
dp[TotalDenominations][Total+1].

So for every possible total ‘t’ (0<= t <= Total) and for every possible coin index (0 <= index < denominations.length),
we have two options:

1. Exclude the coin. Count all the coin combinations without the given coin up to the total ‘t’ => dp[index-1][t]

2. Include the coin if its value is not more than ‘t’. In this case, we will count all the coin combinations to get
the remaining total: dp[index][t-denominations[index]]

Finally, to find the total combinations, we will add both the above two values:


    dp[index][t] = dp[index-1][t] + dp[index][t-denominations[index]
```    
    
    

```
class CoinChange {

  public int countChange(int[] denominations, int total) {
    int n = denominations.length;
    int[][] dp = new int[n][total + 1];

    // populate the total=0 columns, as we will always have an empty set for zero toal
    for(int i=0; i < n; i++)
      dp[i][0] = 1;
    
    // process all sub-arrays for all capacities
    for(int i=0; i < n; i++) {
      for(int t=1; t <= total; t++) {
        if(i > 0)
          dp[i][t] = dp[i-1][t];
        if(t >= denominations[i])
          dp[i][t] += dp[i][t-denominations[i]];
      }
    }

    // total combinations will be at the bottom-right corner.
    return dp[n-1][total];
  }

  public static void main(String[] args) {
    CoinChange cc = new CoinChange();
    int[] denominations = {1, 2, 3};
    System.out.println(cc.countChange(denominations, 5));
  }
}
```

