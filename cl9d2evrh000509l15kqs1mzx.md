# Coin change Leetcode

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

```
Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(amount ==0) return 0;
        // we will use dynamic prgramming for  solving this 
        // bottom-up approach
        int dp[][] = new int[coins.length][amount+1];
        for(int row[]: dp) Arrays.fill(row,-1);
        // we will start from last index and go to first index
        int coinsNeeded = findSmallestList(coins.length-1,coins,amount,dp);
        return coinsNeeded ==(int)1e9 ? -1 : coinsNeeded;
    }
    public int findSmallestList(int index,int[] coin,int amount,int dp[][]){
       if(index==0){
           if(amount % coin[index] ==0){
               return amount/coin[index];
           }
           return (int)1e9;
       }
      if(dp[index][amount]!=-1) return dp[index][amount];
        
        int left =(int)1e9;
        //take same index value
        if(amount>=coin[index]){
            left = 1+ findSmallestList(index,coin,amount-coin[index],dp);
        }
        //take next index 
        int right = 0+ findSmallestList(index-1,coin,amount,dp);
        //System.out.println(Integer.min(left,right));
        return dp[index][amount] =Integer.min(left,right); 
    }
}
```

**We can remove the stack space by using top-down approach i.e. Tabulation Approach of Dp**