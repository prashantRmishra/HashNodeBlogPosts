# Coin Change 2 Leetcode

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

 

**Example 1:**

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

```java
class Solution {
    public int change(int amount, int[] coins) {
        int dp[][] = new int[coins.length][amount+1];
        for(int row[] : dp) Arrays.fill(row,-1);
        return allPossibleCombinations(coins.length-1,new ArrayList<>(),coins,dp,amount);
    }
    public int allPossibleCombinations(int index,List<Integer> list,int[] coins, int[][] dp,int amount){
        if(index==0){
            if(amount % coins[index]==0) {
                return  1;
            }
            return 0;
        }
        if(dp[index][amount]!=-1) return dp[index][amount];
        int take = 0;
        if(amount>=coins[index]){
            list.add(coins[index]);
            take = allPossibleCombinations(index,list,coins,dp,amount-coins[index]);
            list.remove(list.size()-1);
        }
       
        int dontTake = allPossibleCombinations(index-1,list,coins,dp,amount);
        return dp[index][amount] = take+dontTake;
    }
}
```
**We can remove the stack space by using tabulation approach of dp**