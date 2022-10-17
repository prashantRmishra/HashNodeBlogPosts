# Target Sum or Partition with given difference Leetcode

You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '`+`' and '`-`' before each integer in `nums` and then concatenate all the integers.

For example, if `nums = [2, 1]`, you can add a '`+`' before `2` and a '`-`' before `1` and concatenate them to build the expression "`+2-1`".
Return the number of different expressions that you can build, which evaluates to target.

 

**Example 1**:

```
Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

```
**_Simple Recursive Approach_** 
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        return find(0,0,target,nums);
    }
    public int find(int index,int sum, int target, int[] nums){
        if(index>=nums.length){
            if(sum==target) return 1;
            return 0;
        }
        //we can take + for current index element
        int takePlus = find(index+1,sum+nums[index],target,nums);
        int takeMinus = find(index+1,sum-nums[index],target,nums);
        return takePlus + takeMinus;
    }
}
```
**_Optimal Approach_** :
Time complexity `O(n*m)` as `n*m` are the no. of possible states that we will traverse or recursively call, 
Space complexity : `O(n*m) + o(n)` (n for auxiliary space )

```java
//this is similar to subset sum equals to target
//example  nums = [1,1,1,1,1], target = 3
/*
-1 + 1 + 1 + 1 + 1 = 3
i.e. (1+1+1+1)-(1) = 3 i.e subsetSum1 - subsetSum2 = target , where subsetSum1 > sumbsetSum2
so, s1-s2 = t, t is target and s1>s2
since, s1+s2 = sum , sum is sum of entire array;
hence, t+s2+s2 = sum
i.e., s2 = (sum-t)/2;
//edges cases to take care of 
sum-t %2 should be 0, because (sum-t)/2 can't be fraction else s2 can't be equal to a fraction value as the array is integer.
sum-t>=0 as  sum of all the elements can't be negative
*/
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // we have to achieve the target
        int sum = Arrays.stream(nums).reduce(0,(a,b)->a+b);
        
        if(sum-target<0 || (sum-target)%2!=0) return 0;
        //return subsetSumEqualsToTarget(nums.length-1,(sum-target)/2,nums);
        int dp[][] = new int[nums.length][(sum-target)/2 +1];
        for(int row [] : dp) Arrays.fill(row,-1);
        return findSubsetSumCountEqualsToTarget(nums,nums.length-1,(sum-target)/2,dp);
        
    }
 public static int findSubsetSumCountEqualsToTarget(int [] arr, int i, int target,int dp[][]){
       
        if(i==0){
             //since 0<=arr[i]<=1000; hence we have to check the possibility of 0 as well
            // if arr[i]==0 and target =0 then you should return 2 
            //as there are two solutions now i.e. either you will take this 0 or won't take this 0 
            //in either case of taking or not taking of 0 target will remain 0 only, as 0 won't alter target value hence there will be 2 possible solutions
            if(target ==0 && arr[i]==0) return 2; // extra line added to the usual approach because of presence of 0 in the array
            if(target==0 || arr[i]==target) return 1; // usual approach
            return 0;
        }
         if(dp[i][target]!=-1) return dp[i][target];
        int left =0;
        if(target>=arr[i]){
            left = findSubsetSumCountEqualsToTarget(arr,i-1,target-arr[i],dp);
        }
        int right = findSubsetSumCountEqualsToTarget(arr,i-1,target,dp);
        return dp[i][target] = (left+right);
    }
}
```