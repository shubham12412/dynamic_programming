### Problem Statement
Given a stair with ‘n’ steps, implement a method to count how many possible ways are there to reach the top of the 
staircase, given that, at every step you can either take 1 step, 2 steps, or 3 steps.

Let’s first start with a recursive brute-force solution.

### Basic Solution
At every step, we have three options: either jump 1 step, 2 steps, or 3 steps. So our algorithm will look like this:

```
class Staircase {

  public int CountWays(int n) {
    if( n == 0)
      return 1; // base case, we don't need to take any step, so there is only one way
      
    if(n == 1)
      return 1; // we can take one step to reach the end, and that is the only way

    if(n == 2)
      return 2; // we can take one step twice or jump two steps to reach at the top

    // if we take 1 step, we are left with 'n-1' steps;
    int take1Step = CountWays(n-1); 
    // similarly, if we took 2 steps, we are left with 'n-2' steps;
    int take2Step = CountWays(n-2); 
    // if we took 3 steps, we are left with 'n-3' steps;
    int take3Step = CountWays(n-3); 

    return take1Step + take2Step + take3Step;
  }

  public static void main(String[] args) {
    Staircase sc = new Staircase();
    System.out.println(sc.CountWays(3));
    System.out.println(sc.CountWays(4));
    System.out.println(sc.CountWays(5));
  }
}
```

The time complexity of the above algorithm is exponential O(3^n) as we are making three recursive call in the same 
function. The space complexity is O(n) which is used to store the recursion stack.

### Top-down Dynamic Programming with Memoization

```
class Staircase {

  public int CountWays(int n) {
    int dp[] = new int[n+1];
    return CountWaysRecursive(dp, n);
  }
  
  public int CountWaysRecursive(int[] dp, int n) {
    if( n == 0)
      return 1; // base case, we don't need to take any step, so there is only one way

    if(n == 1)
      return 1; // we can take one step to reach the end, and that is the only way

    if(n == 2)
      return 2; // we can take one step twice or jump two steps to reach at the top

    if(dp[n] == 0) {
      // if we take 1 step, we are left with 'n-1' steps;
      int take1Step = CountWaysRecursive(dp, n-1);
      // similarly, if we took 2 steps, we are left with 'n-2' steps;
      int take2Step = CountWaysRecursive(dp, n-2);
      // if we took 3 steps, we are left with 'n-3' steps;
      int take3Step = CountWaysRecursive(dp, n-3);
      dp[n] = take1Step + take2Step + take3Step;
    }
    
    return dp[n];
  }

  public static void main(String[] args) {
    Staircase sc = new Staircase();
    System.out.println(sc.CountWays(3));
    System.out.println(sc.CountWays(4));
    System.out.println(sc.CountWays(5));
  }
}
```

### Bottom-up Dynamic Programming
```
class Staircase {

  public int CountWays(int n) {
    int dp[] = new int[n+1];
    dp[0] = 1;
    dp[1] = 1;
    dp[2] = 2;

    for(int i=3; i<=n; i++)
      dp[i] = dp[i-1] + dp[i-2] + dp[i-3];

    return dp[n];
  }

  public static void main(String[] args) {
    Staircase sc = new Staircase();
    System.out.println(sc.CountWays(3));
    System.out.println(sc.CountWays(4));
    System.out.println(sc.CountWays(5));
  }
}
```

### Memory optimization
We can optimize the space used in our previous solution. We don’t need to store all the counts up to ‘n’, as
we only need three previous numbers to calculate the next count. We can use this fact to further improve our solution:


```
class Staircase {

  public int CountWays(int n) {
    if (n < 2) return 1;
    if (n == 2) return 2;
    int n1=1, n2=1, n3=2, temp;
    for(int i=3; i<=n; i++) {
      temp = n1 + n2 + n3;
      n1 = n2;
      n2 = n3;
      n3 = temp;
    }
    return n3;
  }

  public static void main(String[] args) {
    Staircase sc = new Staircase();
    System.out.println(sc.CountWays(3));
    System.out.println(sc.CountWays(4));
    System.out.println(sc.CountWays(5));
  }
}
```

### Fibonacci number pattern
We can clearly see that this problem follows the Fibonacci number pattern. The only difference is that in Fibonacci 
numbers every number is a sum of the two preceding numbers, whereas in this problem every count is a sum of three 
preceding counts. Here is the recursive formula for this problem:

    CountWays(n) = CountWays(n-1) + CountWays(n-2) + CountWays(n-3), for n >=3

This problem can be extended further. Instead of taking 1, 2, or 3 steps at any time, what if we can take up to ‘k’ 
steps at any time? 



