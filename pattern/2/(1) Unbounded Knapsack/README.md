### Introduction
Given the weights and profits of ‘N’ items, we are asked to put these items in a knapsack which has a capacity ‘C’. 
The goal is to get the maximum profit from the items in the knapsack. The only difference between the 0/1 Knapsack problem 
and this problem is that we are allowed to use an unlimited quantity of an item.

### Problem Statement
Given two integer arrays to represent weights and profits of ‘N’ items, we need to find a subset of these items which will 
give us maximum profit such that their cumulative weight is not more than a given number ‘C’. We can assume an infinite 
supply of item quantities; therefore, each item can be selected multiple times.

### Basic Solution
```
for each item 'i' 
  create a new set which includes one quantity of item 'i' if it does not exceed the capacity, and 
     recursively call to process all items 
  create a new set without item 'i', and recursively process the remaining items 
return the set from the above two sets with higher profit 
```

***The only difference between the 0/1 Knapsack problem and this one is that, after including the item, we recursively call 
to process all the items (including the current item). In 0/1 Knapsack, however, we recursively call to process the remaining 
items.***

```
class Knapsack {

  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    return this.knapsackRecursive(profits, weights, capacity, 0);
  }

  private int knapsackRecursive(int[] profits, int[] weights, int capacity, int currentIndex) {
    // base checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length || 
        currentIndex >= profits.length)
      return 0;

    // recursive call after choosing the items at the currentIndex, note that we recursive call on all
    // items as we did not increment currentIndex
    int profit1 = 0;
    if (weights[currentIndex] <= capacity)
      profit1 = profits[currentIndex]
          + knapsackRecursive(profits, weights, capacity - weights[currentIndex], currentIndex);

    // recursive call after excluding the element at the currentIndex
    int profit2 = knapsackRecursive(profits, weights, capacity, currentIndex + 1);

    return Math.max(profit1, profit2);
  }

  public static void main(String[] args) {
    Knapsack ks = new Knapsack();
    int[] profits = { 15, 50, 60, 90 };
    int[] weights = { 1, 3, 4, 5 };
    int maxProfit = ks.solveKnapsack(profits, weights, 8);
    System.out.println(maxProfit);
  }
}
```

The time complexity of the above algorithm is exponential O(2^{N+C}), where ‘N’ represents the total number of items. 
The space complexity will be O(N+C) to store the recursion stack.

### Top-down Dynamic Programming with Memoization

We will be using a two-dimensional array to store the results of solved sub-problems. As mentioned above, we need to 
store results for every sub-array and for every possible capacity. 

```
class Knapsack {

  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    Integer[][] dp = new Integer[profits.length][capacity + 1];
    return this.knapsackRecursive(dp, profits, weights, capacity, 0);
  }

  private int knapsackRecursive(Integer[][] dp, int[] profits, int[] weights, int capacity,
      int currentIndex) {

    // base checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length ||
        currentIndex >= profits.length)
      return 0;

    // check if we have not already processed a similar sub-problem
    if(dp[currentIndex][capacity] == null) {
      // recursive call after choosing the items at the currentIndex, note that we recursive call on all
      // items as we did not increment currentIndex
      int profit1 = 0;
      if( weights[currentIndex] <= capacity )
          profit1 = profits[currentIndex] + knapsackRecursive(dp, profits, weights,
                  capacity - weights[currentIndex], currentIndex);

      // recursive call after excluding the element at the currentIndex
      int profit2 = knapsackRecursive(dp, profits, weights, capacity, currentIndex + 1);

      dp[currentIndex][capacity] = Math.max(profit1, profit2);
    }

    return dp[currentIndex][capacity];
  }

  public static void main(String[] args) {
    Knapsack ks = new Knapsack();
    int[] profits = {15, 50, 60, 90};
    int[] weights = {1, 3, 4, 5};
    System.out.println(ks.solveKnapsack(profits, weights, 8));
    System.out.println(ks.solveKnapsack(profits, weights, 6));
  }
}
```

### Bottom-up Dynamic Programming

```
“Find the maximum profit for every sub-array and for every possible capacity”.

So for every possible capacity ‘c’ (0 <= c <= capacity), we have two options:

1. Exclude the item. In this case, we will take whatever profit we get from the sub-array excluding 
this item: dp[index-1][c]

2. Include the item if its weight is not more than the ‘c’. In this case, we include its profit plus 
whatever profit we get from the remaining capacity: profit[index] + dp[index][c-weight[index]]

Finally, we have to take the maximum of the above two values:

    dp[index][c] = max (dp[index-1][c], profit[index] + dp[index][c-weight[index]])

```

```
class Knapsack {

  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    // base checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length)
      return 0;

    int n = profits.length;
    int[][] dp = new int[n][capacity + 1];

    // populate the capacity=0 columns
    for(int i=0; i < n; i++)
      dp[i][0] = 0;

    // process all sub-arrays for all capacities
    for(int i=0; i < n; i++) {
      for(int c=1; c <= capacity; c++) {
        int profit1=0, profit2=0;
        if(weights[i] <= c)
          profit1 = profits[i] + dp[i][c-weights[i]];
        if( i > 0 )
          profit2 = dp[i-1][c];
        dp[i][c] = profit1 > profit2 ? profit1 : profit2;
      }
    }

    // maximum profit will be in the bottom-right corner.
    return dp[n-1][capacity];
  }

  public static void main(String[] args) {
    Knapsack ks = new Knapsack();
    int[] profits = {15, 50, 60, 90};
    int[] weights = {1, 3, 4, 5};
    System.out.println(ks.solveKnapsack(profits, weights, 8));
    System.out.println(ks.solveKnapsack(profits, weights, 6));
  }
}
```

### Find the selected items
As we know, the final profit is at the right-bottom corner; hence we will start from there to find the items 
that will be going to the knapsack.

As you remember, at every step we had two options: include an item or skip it. If we skip an item, then we 
take the profit from the cell right above it; if we include the item, then we jump to the remaining profit to 
find more items.





