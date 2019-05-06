### Problem Statement
Given strings s1 and s2, we need to transform s1 into s2 by deleting and inserting characters. Write a function 
to calculate the count of the minimum number of deletion and insertion operations.

### Example:
```
Input: s1 = "abdca"
       s2 = "cbda"
Output: 2 deletions and 1 insertion.
Explanation: We need to delete {'a', 'c'} and insert {'c'} to s1 to transform it into s2.
```

### Solution
This problem can easily be converted to the Longest Common Subsequence (LCS). 

If we can find the LCS of the two input strings, we can easily find how many characters we need to 
insert and delete from s1. Here is how we can do this:

```
1. Let’s assume len1 is the length of s1 and len2 is the length of s2.

2. Now let’s assume c1 is the length of LCS of the two strings s1 and s2.

3. To transform s1 into s2, we need to delete everything from s1 which is not part of LCS, 
so minimum deletions we need to perform from s1 => len1 - c1

4. Similarly, we will be inserting everything in s1 which is present in s2 but not part of LCS, 
so minimum insertions we need to perform in s1 => len2 - c1
```

Let’s jump directly to the bottom-up dynamic programming solution:

### Bottom-up Dynamic Programming


```
class MDI {

  public void findMDI(String s1, String s2) {
    int c1 = findLCSLength(s1, s2);
    System.out.println("Minimum deletions needed: " + (s1.length() - c1));
    System.out.println("Minimum insertions needed: " + (s2.length() - c1));
  }
  
  private int findLCSLength(String s1, String s2) {
    int[][] dp = new int[s1.length()+1][s2.length()+1];
    int maxLength = 0;
    for(int i=1; i <= s1.length(); i++) {
      for(int j=1; j <= s2.length(); j++) {
        if(s1.charAt(i-1) == s2.charAt(j-1))
          dp[i][j] = 1 + dp[i-1][j-1];
        else
          dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
        
        maxLength = Math.max(maxLength, dp[i][j]);
      }
    }
    return maxLength;
  }

  public static void main(String[] args) {
    MDI mdi = new MDI();
    mdi.findMDI("abc", "fbc");
    mdi.findMDI("abdca", "cbda");
    mdi.findMDI("passport", "ppsspt");
  }
}
```


