### Problem Statement
Given a number ‘n’, implement a method to count how many possible ways there are to express ‘n’ as the sum of 1, 3, or 4.

### Example:
n : 4
Number of ways = 4
Explanation: Following are the four ways we can exoress 'n' : {1,1,1,1}, {1,3}, {3,1}, {4} 

***Let’s first start with a recursive brute-force solution.***

### Basic Solution
For every number ‘i’, we have three option: subtract either 1, 3, or 4 from ‘i’ and recursively process the 
remaining number. So our algorithm will look like:

```
class ExpressNumber {

  public int CountWays(int n) {
    if( n == 0)
      return 1; // base case, we don't need to subtract any thing, so there is only one way

    if(n == 1)
      return 1; // we can subtract 1 to be left with zero, and that is the only way

    if(n == 2)
      return 1; // we can subtract 1 twice to get zero and that is the only way

    if(n == 3)
      return 2; // '3' can be expressed as {1,1,1}, {3}
      
    // if we subtract 1, we are left with 'n-1'
    int subtract1 = CountWays(n-1);
    // if we subtract 3, we are left with 'n-3'
    int subtract3 = CountWays(n-3);
    // if we subtract 4, we are left with 'n-4'
    int subtract4 = CountWays(n-4);

    return subtract1 + subtract3 + subtract4;
  }

  public static void main(String[] args) {
    ExpressNumber en = new ExpressNumber();
    System.out.println(en.CountWays(4));
    System.out.println(en.CountWays(5));
    System.out.println(en.CountWays(6));
  }
}
```

### Top-down Dynamic Programming with Memoization

```
class ExpressNumber {

  public int CountWays(int n) {
    int dp[] = new int[n+1];
    return CountWaysRecursive(dp, n);
  }

  public int CountWaysRecursive(int[] dp, int n) {
    if( n == 0)
      return 1; // base case, we don't need to subtract any thing, so there is only one way

    if(n == 1)
      return 1; // we can take subtract 1 to be left with zero, and that is the only way

    if(n == 2)
      return 1; // we can subtract 1 twice to get zero and that is the only way

    if(n == 3)
      return 2; // '3' can be expressed as {1,1,1}, {3}
      
    if(dp[n] == 0) {
      // if we subtract 1, we are left with 'n-1'
      int subtract1 = CountWays(n-1);
      // if we subtract 3, we are left with 'n-3'
      int subtract3 = CountWays(n-3);
      // if we subtract 4, we are left with 'n-4'
      int subtract4 = CountWays(n-4);
      dp[n] = subtract1 + subtract3 + subtract4;
    }

    return dp[n];
  }

  public static void main(String[] args) {
    ExpressNumber en = new ExpressNumber();
    System.out.println(en.CountWays(4));
    System.out.println(en.CountWays(5));
    System.out.println(en.CountWays(6));
  }
}
```

### Bottom-up Dynamic Programming

```
class ExpressNumber {

  public int CountWays(int n) {
    int dp[] = new int[n+1];
    dp[0] = 1;
    dp[1] = 1;
    dp[2] = 1;
    dp[3] = 2;

    for(int i=4; i<=n; i++)
      dp[i] = dp[i-1] + dp[i-3] + dp[i-4];

    return dp[n];
  }

  public static void main(String[] args) {
    ExpressNumber en = new ExpressNumber();
    System.out.println(en.CountWays(4));
    System.out.println(en.CountWays(5));
    System.out.println(en.CountWays(6));
  }
}
```

### Fibonacci number pattern
We can clearly see that this problem follows the Fibonacci number pattern. However, every number in a Fibonacci 
series is the sum of the previous two numbers, whereas in this problem every count is a sum of previous three
numbers: previous-1, previous-3, and previous-4. Here is the recursive formula for this problem:


    CountWays(n) = CountWays(n-1) + CountWays(n-3) + CountWays(n-4), for n >= 4
    
    
    

