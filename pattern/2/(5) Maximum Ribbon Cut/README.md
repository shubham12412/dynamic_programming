### Introduction
We are given a ribbon of length ‘n’ and a set of possible ribbon lengths. Now we need to cut the ribbon into the maximum 
number of pieces that comply with the above-mentioned possible lengths. Write a method that will return the count of pieces.

### Problem Statement
Given a number array to represent possible ribbon lengths and a total ribbon length ‘n’, we need to find the maximum number
of pieces that the ribbon can be cut into.


***This problem follows the Unbounded Knapsack pattern and is quite similar to Minimum Coin Change (MCC). The only difference 
is that in MCC we were asked to find the minimum number of coin changes, whereas in this problem we need to find the
maximum number of pieces.***

### Basic Solution
A basic brute-force solution could be to try all combinations of the given lengths to select the maximum one 
that gives the total length of ‘n’.

```
for each length 'l' 
  create a new set which includes one quantity of length 'l' if it does not exceed 'n', and 
     recursively call to process all lengths 
  create a new set without length 'l', and recursively call to process the remaining lengths
return the number of pieces from the above two sets with a higher number of pieces
```

```
class CutRibbon {

  public int countRibbonPieces(int[] ribbonLengths, int total) {
    int maxPieces = this.countRibbonPiecesRecursive(ribbonLengths, total, 0);
    return maxPieces == Integer.MIN_VALUE ? -1 : maxPieces;
  }

  private int countRibbonPiecesRecursive(int[] ribbonLengths, int total, int currentIndex) {
    // base check
    if (total == 0)
      return 0;

    if(ribbonLengths.length == 0 || currentIndex >= ribbonLengths.length)
      return Integer.MIN_VALUE;

    // recursive call after selecting the ribbon length at the currentIndex
    // if the ribbon length at the currentIndex exceeds the total, we shouldn't process this
    int c1 = Integer.MIN_VALUE;
    if( ribbonLengths[currentIndex] <= total ) {
      int result = countRibbonPiecesRecursive(
                    ribbonLengths, total - ribbonLengths[currentIndex], currentIndex);
      if(result != Integer.MIN_VALUE){
        c1 = result + 1;
      }
    }

    // recursive call after excluding the ribbon length at the currentIndex
    int c2 = countRibbonPiecesRecursive(ribbonLengths, total, currentIndex + 1);
    return Math.max(c1, c2);
  }

  public static void main(String[] args) {
    CutRibbon cr = new CutRibbon();
    int[] ribbonLengths = {2,3,5};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 5));
    ribbonLengths = new int[]{2,3};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 7));
    ribbonLengths = new int[]{3,5,7};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 13));
    ribbonLengths = new int[]{3,5};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 7));
  }
}
```

Since this problem is quite similar to Minimum Coin Change, let’s jump on to the bottom-up dynamic programming solution.

### Bottom-up Dynamic Programming

Let’s try to populate our array dp[ribbonLength][total+1] for every possible ribbon length with a maximum number of pieces.

```
So for every possible length ‘len’ (0<= len <= total) and for every possible ribbon length
index (0 <= index < ribbonLengths.length), we have two options:

1. Exclude the ribbon length: In this case, we will take the maximum pieces count from the previous
set => dp[index-1][len]

2. Include the ribbon length if its value is not more than ‘len’: In this case, we will take the maximum 
pieces needed to get the remaining total, plus include ‘1’ for the current ribbon 
length => 1 + dp[index][len-ribbonLengths[index]]

Finally, we will take the maximum of the above two values for our solution:


    dp[index][t] = max(dp[index-1][len], 1 + dp[index][len-denominations[index])

```

```
import java.util.Arrays;

class CutRibbon {

  public int countRibbonPieces(int[] ribbonLengths, int total)
  {
    int n = ribbonLengths.length;
    int[][] dp = new int[n][total + 1];

    for(int i=0; i < n; i++)
      for(int j=0; j <= total; j++)
        dp[i][j] = Integer.MIN_VALUE;

    // populate the total=0 columns, as we don't need any ribbon to make zero total
    for(int i=0; i < n; i++)
      dp[i][0] = 0;

    for(int i=0; i < n; i++) {
      for(int t=1; t <= total; t++) {
        if(i > 0) //exclude the ribbon
          dp[i][t] = dp[i-1][t];
        // include the ribbon and check if the remaining length can be cut into available lenghts
        if(t >= ribbonLengths[i] && dp[i][t-ribbonLengths[i]] != Integer.MIN_VALUE)
          dp[i][t] = Math.max(dp[i][t], dp[i][t-ribbonLengths[i]]+1);
      }
    }

    // total combinations will be at the bottom-right corner, return '-1' if cutting is not possible
    return (dp[n-1][total] == Integer.MIN_VALUE ? -1 : dp[n-1][total]);
  }

  public static void main(String[] args) {
    CutRibbon cr = new CutRibbon();
    int[] ribbonLengths = {2,3,5};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 5));
    ribbonLengths = new int[]{2,3};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 7));
    ribbonLengths = new int[]{5,3,7};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 13));
    ribbonLengths = new int[]{3,5};
    System.out.println(cr.countRibbonPieces(ribbonLengths, 7));
  }
}
```
