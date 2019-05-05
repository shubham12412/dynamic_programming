### Problem Statement
Write a function to calculate the nth Fibonacci number.

Mathematically we can define the Fibonacci numbers as:

    Fib(n) = Fib(n-1) + Fib(n-2), for n > 1
    Given that: Fib(0) = 0, and Fib(1) = 1
    
### Basic Solution
A basic solution could be to have a recursive implementation of the mathematical formula discussed above:

```
class Fibonacci {

  public int CalculateFibonacci(int n) {
    if(n < 2)
      return n;
    return CalculateFibonacci(n-1) + CalculateFibonacci(n-2);
  }

  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println("5th Fibonacci is ---> " + fib.CalculateFibonacci(5));
    System.out.println("6th Fibonacci is ---> " + fib.CalculateFibonacci(6));
    System.out.println("7th Fibonacci is ---> " + fib.CalculateFibonacci(7));
  }
}
```

The time complexity of the above algorithm is exponential O(2^n) as we are making two recursive calls in the same function. 
The space complexity is O(n) which is used to store the recursion stack.

### Top-down Dynamic Programming with Memoization

```
class Fibonacci {

  public int CalculateFibonacci(int n) {
    int dp[] = new int[n + 1];
    return CalculateFibonacciRecursive(dp, n);
  }

  public int CalculateFibonacciRecursive(int[] dp, int n) {
    if (n < 2)
      return n;
    if (dp[n] == 0)
      dp[n] = CalculateFibonacciRecursive(dp, n - 1) + CalculateFibonacciRecursive(dp, n - 2);
    return dp[n];
  }

  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println("5th Fibonacci is ---> " + fib.CalculateFibonacci(5));
    System.out.println("6th Fibonacci is ---> " + fib.CalculateFibonacci(6));
    System.out.println("7th Fibonacci is ---> " + fib.CalculateFibonacci(7));
  }
}
```

### Bottom-up Dynamic Programming

```
class Fibonacci {

  public int CalculateFibonacci(int n) {
    int dp[] = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    for (int i = 2; i <= n; i++)
      dp[i] = dp[i - 1] + dp[i - 2];
    return dp[n];
  }

  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println("5th Fibonacci is ---> " + fib.CalculateFibonacci(5));
    System.out.println("6th Fibonacci is ---> " + fib.CalculateFibonacci(6));
    System.out.println("7th Fibonacci is ---> " + fib.CalculateFibonacci(7));
  }
}
```

### Memory optimization
We can optimize the space used in our previous solution. We don’t need to store all the Fibonacci numbers up to ‘n’,
as we only need two previous numbers to calculate the next Fibonacci number. We can use this fact to further improve
our solution:

```
class Fibonacci {

  public int CalculateFibonacci(int n) {
    if (n < 2)
      return n;
    int n1 = 0, n2 = 1, temp;
    for (int i = 2; i <= n; i++) {
      temp = n1 + n2;
      n1 = n2;
      n2 = temp;
    }
    return n2;
  }

  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println("5th Fibonacci is ---> " + fib.CalculateFibonacci(5));
    System.out.println("6th Fibonacci is ---> " + fib.CalculateFibonacci(6));
    System.out.println("7th Fibonacci is ---> " + fib.CalculateFibonacci(7));
  }
}
```

