### Problem Statement
Given a number sequence, find the length of its Longest Increasing Subsequence (LIS). In an increasing subsequence, 
all the elements are in increasing order (from lowest to highest).

### Examaple
```
Input: {4,2,3,6,10,1,12}
Output: 5
Explanation: The LIS is {2,3,6,10,12}.
```

### Basic Solution
```
A basic brute-force solution could be to try all the subsequences of the given number sequence. 
We can process one number at a time, so we have two options at any step:

1.If the current number is greater than the previous number that we included, we can increment our count and 
make a recursive call for the remaining array.

2.We can skip the current number to make a recursive call for the remaining array.

The length of the longest increasing subsequence will be the maximum number returned by the two recurse calls 
from the above two options.
```

```
class LIS {

  public int findLISLength(int[] nums) {
      return findLISLengthRecursive(nums, 0, -1);
  }

  private int findLISLengthRecursive(int[] nums, int currentIndex, int previousIndex) {
    if(currentIndex == nums.length)
      return 0;

    // include nums[currentIndex] if it is larger than the last included number
    int c1 = 0;
    if(previousIndex == -1 || nums[currentIndex] > nums[previousIndex])
      c1 = 1 + findLISLengthRecursive(nums, currentIndex+1, currentIndex);

    // excluding the number at currentIndex
    int c2 = findLISLengthRecursive(nums, currentIndex+1, previousIndex);

    return Math.max(c1, c2);
  }

  public static void main(String[] args) {
    LIS lis = new LIS();
    int[] nums = {4,2,3,6,10,1,12};
    System.out.println(lis.findLISLength(nums));
    nums = new int[]{-4,10,3,7,15};
    System.out.println(lis.findLISLength(nums));
  }
}
```


The time complexity of the above algorithm is exponential O(2^n), where ‘n’ is the lengths of the input array. 
The space complexity is O(n) which is used to store the recursion stack.

### Top-down Dynamic Programming with Memoization

The two changing values for our recursive function are the current and the previous index. Therefore, we can 
store the results of all subproblems in a two-dimensional array. (Another alternative could be to use a hash-table
whose key would be a string (currentIndex + “|” + previousIndex)).


```
class LIS {

  public int findLISLength(int[] nums) {
    Integer[][] dp = new Integer[nums.length][nums.length+1];
    return findLISLengthRecursive(dp, nums, 0, -1);
  }

  private int findLISLengthRecursive(Integer[][] dp, int[] nums, int currentIndex, int previousIndex) {
    if(currentIndex == nums.length)
      return 0;

    if(dp[currentIndex][previousIndex+1] == null) {
      // include nums[currentIndex] if it is larger than the last included number
      int c1 = 0;
      if(previousIndex == -1 || nums[currentIndex] > nums[previousIndex])
        c1 = 1 + findLISLengthRecursive(dp, nums, currentIndex+1, currentIndex);

      int c2 = findLISLengthRecursive(dp, nums, currentIndex+1, previousIndex);
      dp[currentIndex][previousIndex+1] = Math.max(c1, c2);
    }

    return dp[currentIndex][previousIndex+1];
  }

  public static void main(String[] args) {
    LIS lis = new LIS();
    int[] nums = {4,2,3,6,10,1,12};
    System.out.println(lis.findLISLength(nums));
    nums = new int[]{-4,10,3,7,15};
    System.out.println(lis.findLISLength(nums));
  }
}
```

### Bottom-up Dynamic Programming

The above algorithm tells us two things:

1. If the number at the current index is bigger than the number at the previous index, we increment the count
for LIS up to the current index.

2. But if there is a bigger LIS without including the number at the current index, we take that.

So we need to find all the increasing subsequences for the number at index ‘i’, from all the previous numbers 
(i.e. number till index ‘i-1’), to eventually find the longest increasing subsequence.

If ‘i’ represents the ‘currentIndex’ and ‘j’ represents the ‘previousIndex’, our recursive formula would look like:

    if num[i] > num[j] => dp[i] = dp[j] + 1 if there is no bigger LIS for 'i'
 
 ```
 class LIS {

  public int findLISLength(int[] nums) {
    int[] dp = new int[nums.length];
    dp[0] = 1;    

    int maxLength = 1;
    for (int i=1; i<nums.length; i++) { 
      dp[i] = 1; 
      for (int j=0; j<i; j++)   
        if (nums[i] > nums[j] && dp[i] <= dp[j] ) {
          dp[i] = dp[j]+1;  
          maxLength = Math.max(maxLength, dp[i]);
        }
    } 
    return maxLength;
  }

  public static void main(String[] args) {
    LIS lis = new LIS();
    int[] nums = {4,2,3,6,10,1,12};
    System.out.println(lis.findLISLength(nums));
    nums = new int[]{-4,10,3,7,15};
    System.out.println(lis.findLISLength(nums));
  }
}
```


 
 

