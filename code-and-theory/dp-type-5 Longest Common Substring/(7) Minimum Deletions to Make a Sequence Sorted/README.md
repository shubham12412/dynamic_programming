### Problem Statement
Given a number sequence, find the minimum number of elements that should be deleted to make the remaining sequence sorted.

### Basic Solution
A basic brute-force solution could be to try deleting all combinations of elements, one by one, and 
checking if that makes the subsequence sorted.

Alternately, we can convert this problem into a Longest Increasing Subsequence (LIS) problem. As we know that 
LIS will give us the length of the longest increasing subsequence (in the sorted order!), which means that the
elements which are not part of the LIS should be removed to make the sequence sorted. This is exactly what we need.
So we’ll get our solution by subtracting the length of LIS from the length of the input array:
Length-of-input-array - LIS()


Let’s jump directly to the bottom-up dynamic programming solution.

### Bottom-up Dynamic Programming
```
class MDSS {

  public int findMinimumDeletions(int[] nums){
    // subtracting the length of LIS from the length of the input array to get minimum number of deletions
    return nums.length - findLISLength(nums);
  }

  private int findLISLength(int[] nums) {
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
    MDSS mdss = new MDSS();
    int[] nums = {4,2,3,6,10,1,12};
    System.out.println(mdss.findMinimumDeletions(nums));
    nums = new int[]{-4,10,3,7,15};
    System.out.println(mdss.findMinimumDeletions(nums));
    nums = new int[]{3,2,1,0};
    System.out.println(mdss.findMinimumDeletions(nums));
  }
}
```

