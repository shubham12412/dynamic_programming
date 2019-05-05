### Problem Statement
Given a rod of length ‘n’, we are asked to cut the rod and sell the pieces in a way that will maximize the profit. 
We are also given the price of every piece of length ‘i’ where ‘1 <= i <= n’.

#### Example:
Lengths: [1, 2, 3, 4, 5]
Prices: [2, 6, 7, 10, 13]
Rod Length: 5

### Basic Solution
This problem can be mapped to the Unbounded Knapsack pattern. The Weights array of the Unbounded Knapsack problem is 
equivalent to Lengths array, and Profits is equivalent to Prices.

```
for each rod length 'i' 
  create a new set which includes one quantity of length 'i', and recursively process 
      all rod lengths for the remaining length 
  create a new set without rod length 'i', and recursively process for remaining rod lengths
return the set from the above two sets with a higher sales price 
```
Since this problem is quite similar to Unbounded Knapsack, let’s jump directly to the bottom-up dynamic solution.

### Bottom-up Dynamic Programming

```
So for every possible rod length ‘len’ (0<= len <= n), we have two options:

1. Exclude the piece. In this case, we will take whatever price we get from the rod length 
excluding this piece => dp[index-1][len]

2. Include the piece if its length is not more than ‘len’. In this case, we include its price 
plus whatever price we get from the remaining rod length => prices[index] + dp[index][len-lengths[index]]

Finally, we have to take the maximum of the above two values:


    dp[index][len] = max (dp[index-1][len], prices[index] + dp[index][len-lengths[index]])
```

##### Example
Lengths: [1, 2, 3, 4, 5]  
Prices: [2, 6, 7, 10, 13]  
Rod Length: 5


```
class RodCutting {

  public int solveRodCutting(int[] lengths, int[] prices, int n) {
    // base checks
    if (n <= 0 || prices.length == 0 || prices.length != lengths.length)
      return 0;

    int lengthCount = lengths.length;
    int[][] dp = new int[lengthCount][n + 1];

    // process all rod lengths for all prices
    for(int i=0; i < lengthCount; i++) {
      for(int len=0; len <= n; len++) {
        int p1=0, p2=0;
        if(lengths[i] <= len)
          p1 = prices[i] + dp[i][len-lengths[i]];
        if( i > 0 )
          p2 = dp[i-1][len];
        dp[i][len] = Math.max(p1, p2);
      }
    }

    // maximum price will be at the bottom-right corner.
    return dp[lengthCount-1][n];
  }

  public static void main(String[] args) {
    RodCutting rc = new RodCutting();
    int[] lengths = {1, 2, 3, 4, 5};
    int[] prices = {2, 6, 7, 10, 13};
    int maxProfit = rc.solveRodCutting(lengths, prices, 5);
    System.out.println(maxProfit);
  }
}
```

#### Find the selected items
As we know, the final price is at the right-bottom corner; hence we will start from there to find the rod lengths.

As you remember, at every step we had two options: include a rod piece or skip it. If we skip it, then we take the price 
from the cell right above it; if we include it, then we jump to the remaining length to find more pieces.

