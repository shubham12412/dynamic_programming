### Problem Statement
Given a string, find the length of its Longest Palindromic Substring (LPS). In a palindromic string, elements read 
the same backward and forward.

### Examaple:
```
Input: "abdbca"
Output: 3
Explanation: LPS is "bdb".
```

### Basic Solution
This problem follows the Longest Palindromic Subsequence pattern. The only difference is that in a palindromic 
subsequence characters can be non-adjacent, whereas in a substring all characters should form a palindrome. 
We will follow a similar approach though.

```
The brute-force solution will be to try all the substrings of the given string. We can start processing from 
the beginning and the end of the string. 
So at any step, we will do two things:

1. If the element at the beginning and the end are the same, we make a recursive call to check if the 
remaining substring is a palindrome too. If so, the substring is a palindrome from beginning to the end.

2. We will skip either the element from the beginning or the end to make two recursive calls for the 
remaining substring.

The length of LPS would be the maximum of the three recursive calls from the above two steps.
```

```
class LPS {

  public int findLPSLength(String st) {
    return findLPSLengthRecursive(st, 0, st.length()-1);
  }

  private int findLPSLengthRecursive(String st, int startIndex, int endIndex) {
    if(startIndex > endIndex)
      return 0;

    // every string with one character is a palindrome
    if(startIndex == endIndex)
      return 1;

    int c1=0;
    // case 1: elements at the beginning and the end are the same
    if(st.charAt(startIndex) == st.charAt(endIndex)) {
      int remainingLength = endIndex-startIndex-1;
      if(remainingLength == findLPSLengthRecursive(st, startIndex+1, endIndex-1))
        c1 = remainingLength + 2;
    }

    // case 2: skip one character either from the beginning or the end
    int c2 =  findLPSLengthRecursive(st, startIndex+1, endIndex);
    int c3 =  findLPSLengthRecursive(st, startIndex, endIndex-1);
    return Math.max(c1, Math.max(c2, c3));
  }

  public static void main(String[] args) {
    LPS lps = new LPS();
    System.out.println(lps.findLPSLength("abdbca"));
    System.out.println(lps.findLPSLength("cddpd"));
    System.out.println(lps.findLPSLength("pqr"));
  }
}
```

Due to the three recursive calls, the time complexity of the above algorithm is exponential O(3^n), where ‘n’ is 
the length of the input string. The space complexity is O(n) which is used to store the recursion stack.

### Top-down Dynamic Programming with Memoization

The two changing values to our recursive function are the two indexes, startIndex and endIndex. Therefore, we can
store the results of all the subproblems in a two-dimensional array. (Another alternative could be to use a hash-table
whose key would be a string (startIndex + “|” + endIndex))

```
class LPS {

  public int findLPSLength(String st) {
    Integer[][] dp = new Integer[st.length()][st.length()];
    return findLPSLengthRecursive(dp, st, 0, st.length()-1);
  }

  private int findLPSLengthRecursive(Integer[][] dp, String st, int startIndex, int endIndex) {
    if(startIndex > endIndex)
      return 0;

    // every string with one character is a palindrome
    if(startIndex == endIndex)
      return 1;

    if(dp[startIndex][endIndex] == null) {
      int c1=0;
      // case 1: elements at the beginning and the end are the same
      if(st.charAt(startIndex) == st.charAt(endIndex)) {
        int remainingLength = endIndex-startIndex-1;
        // if the remaining string is a palindrome too
        if(remainingLength == findLPSLengthRecursive(dp, st, startIndex+1, endIndex-1))
          c1 = remainingLength + 2;
      }

      // case 2: skip one character either from the beginning or the end
      int c2 =  findLPSLengthRecursive(dp, st, startIndex+1, endIndex);
      int c3 =  findLPSLengthRecursive(dp, st, startIndex, endIndex-1);
      dp[startIndex][endIndex] = Math.max(c1, Math.max(c2, c3));
    }

    return dp[startIndex][endIndex];
  }

  public static void main(String[] args) {
    LPS lps = new LPS();
    System.out.println(lps.findLPSLength("abdbca"));
    System.out.println(lps.findLPSLength("cddpd"));
    System.out.println(lps.findLPSLength("pqr"));
  }
}
```

### Bottom-up Dynamic Programming
Since we want to try all the substrings of the given string, we can use a two-dimensional array to store the
subproblems’ results. So dp[i][j] will be ‘true’ if the substring from index ‘i’ to index ‘j’ is a palindrome.

```
We can start from the beginning of the string and keep adding one element at a time. At every step, we will 
try all of its substrings. 

So for every endIndex and startIndex in the given string, we need to check the following thing:

1. If the element at the startIndex matches the element at the endIndex, we will further check if
the remaining substring (from startIndex+1 to endIndex-1) is a substring too.

So our recursive formula will look like:

if st[startIndex] == st[endIndex], and 
        if the remaing string is of zero length or dp[startIndex+1][endIndex-1] is a palindrome then
   dp[startIndex][endIndex] = true
   
```   

```
class LPS {

  public int findLPSLength(String st) {
    // dp[i][j] will be 'true' if the string from index 'i' to index 'j' is a
    // palindrome
    boolean[][] dp = new boolean[st.length()][st.length()];

    // every string with one character is a palindrome
    for (int i = 0; i < st.length(); i++)
      dp[i][i] = true;

    int maxLength = 1;
    for (int startIndex = st.length() - 1; startIndex >= 0; startIndex--) {
      for (int endIndex = startIndex + 1; endIndex < st.length(); endIndex++) {
        if (st.charAt(startIndex) == st.charAt(endIndex)) {
          // if it's a two character string or if the remaining string is a palindrome too
          if (endIndex - startIndex == 1 || dp[startIndex + 1][endIndex - 1]) {
            dp[startIndex][endIndex] = true;
            maxLength = Math.max(maxLength, endIndex - startIndex + 1);
          }
        }
      }
    }

    return maxLength;
  }

  public static void main(String[] args) {
    LPS lps = new LPS();
    System.out.println(lps.findLPSLength("abdbca"));
    System.out.println(lps.findLPSLength("cdpdd"));
    System.out.println(lps.findLPSLength("pqr"));
  }
}
```

### Manacher’s Algorithm
The best known algorithm to find LPS which runs in linear time O(n) is Manacher’s Algorithm. However, it 
is a non-trivial algorithm that doesn’t use DP. Please take a look to familiarize yourself with this algorithm, 
however, no one expects you to come up with such an algorithm in a 45 minutes coding interview.

https://en.wikipedia.org/wiki/Longest_palindromic_substring
