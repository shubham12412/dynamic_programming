### Problem Statement
Given a string, find the minimum number of characters that we can remove to make it a palindrome.

### Example
```
Input: "abdbca"
Output: 1
Explanation: By removing "c", we get a palindrome "abdba".
```

### Solution
This problem can be easily converted to the Longest Palindromic Subsequence (LPS) problem. We can use 
the fact that LPS is the best subsequence we can have, so any character that is not part of LPS must be removed. 
Please note that it is ‘Longest Palindromic SubSequence’ and not ‘Longest Palindrome Substring’.

So, our solution for a given string ‘st’ will be:

Minimum_deletions_to_make_palindrome = Length(st) - LPS(st)

### bottom-up dynamic programming:

```
class MDSP {

  public int findMinimumDeletions(String st) {
    // subtracting the length of Longest Palindromic Subsequence from the length of
    // the input string to get minimum number of deletions
    return st.length() - findLPSLength(st);
  }

  public int findLPSLength(String st) {
    // dp[i][j] stores the length of LPS from index 'i' to index 'j'
    int[][] dp = new int[st.length()][st.length()];

    // every sequence with one element is a palindrome of length 1
    for (int i = 0; i < st.length(); i++)
      dp[i][i] = 1;

    for (int startIndex = st.length() - 1; startIndex >= 0; startIndex--) {
      for (int endIndex = startIndex + 1; endIndex < st.length(); endIndex++) {
        // case 1: elements at the beginning and the end are the same
        if (st.charAt(startIndex) == st.charAt(endIndex)) {
          dp[startIndex][endIndex] = 2 + dp[startIndex + 1][endIndex - 1];
        } else { // case 2: skip one element either from the beginning or the end
          dp[startIndex][endIndex] = Math.max(dp[startIndex + 1][endIndex], dp[startIndex][endIndex - 1]);
        }
      }
    }
    return dp[0][st.length() - 1];
  }

  public static void main(String[] args) {
    MDSP mdsp = new MDSP();
    System.out.println(mdsp.findMinimumDeletions("abdbca"));
    System.out.println(mdsp.findMinimumDeletions("cddpd"));
    System.out.println(mdsp.findMinimumDeletions("pqr"));
  }
}
```


## Similar problems
Here are a couple of similar problems:


***1. Minimum insertions in a string to make it a palindrome***

Will the above approach work if we make insertions instead of deletions?

Yes, the length of the Longest Palindromic Subsequence is the best palindromic subsequence we can have. 

***2. Find if a string is K-Palindromic***

Any string will be called K-palindromic if it can be transformed into a palindrome by removing at most 
‘K’ characters from it.

This problem can easily be converted to our base problem of finding the minimum deletions in a string to make 
it a palindrome. If the “minimum deletion count” is not more than ‘K’, the string will be K-Palindromic.





   
   
