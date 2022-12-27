# Dynamic Programming

[Subset sum equals to target](https://www.codingninjas.com/codestudio/problems/1550954?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1) Note: [For detailed explanation](https://dev.to/prashantrmishra/partition-equals-subset-sum-leetcode-problem-or-subset-sum-equals-to-target-4441) or [TakeUForward YouTube Solution](https://www.youtube.com/watch?v=fWX9xDmIzRI)

`TC: O(nm) same for both memoization and tabulation Memoization has extra stack space of o(n), as we will be calling recursion method at max n times`

```java
import java.util.*;
public class Solution {
    public static boolean subsetSumToK(int n, int k, int arr[]){
        //we will use backtracking and then we will optimize it
        boolean dp[][] = new boolean[n][k+1];
        //return isPresent(n-1,k,arr,dp); for memoization approach
        for(int i =0;i<n;i++) dp[i][0] = true;
        if(arr[0]<=k)
            dp[0][arr[0]] = true;
        for(int index =1;index<n;index++)
            for(int target = 1;target<=k;target++){
    
                boolean take = false;
                if(target>=arr[index]){
                    take = dp[index-1][target-arr[index]];
                }
                //don't take
                boolean dontTake = dp[index-1][target];
                dp[index][target]  = take || dontTake;
            }
        return dp[n-1][k];
    }
    
    
    
    
    public static boolean isPresent(int index,int target, int arr[],boolean dp[][]){
        //base case
        if(target ==0) return true;
        if(index ==0) {
            if(target == arr[index]) return true;
            return false;
        }
        if(dp[index][target]!=false) return dp[index][target];
        //take 
        boolean take = false;
        if(target>=arr[index]){
            take = isPresent(index-1,target-arr[index],arr,dp);
        }
        //don't take
        boolean dontTake = isPresent(index-1,target,arr,dp);
        return dp[index][target]  = take || dontTake;
    }
}
```

[Rod cutting problem](https://www.codingninjas.com/codestudio/problems/800284?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1) `TC: O(nm)`, SC: O(nm) + O(n), extra o(n) for recursive call stack

```java
import java.util.*;
public class Solution {
	public static int cutRod(int price[], int n) {
		// Write your code here.
        //this is similar to unbounded 0/1 knapsack matrix
        //we have cost of different pieces, we have to take N pieces that will 
        //give  maximum profit.
        //consider the n as the knapsack capacity
        int dp[][] = new int[n][n+1];
        for(int row[] : dp) Arrays.fill(row,-1);
        return maxProfit(n-1,price,n,dp);
	}
    public static int maxProfit(int index,int[] price,int W,int dp[][]){
        //base case
        if(index==0){
            return (W /(index+1))*price[index];
        }
        if(dp[index][W]!=-1) return dp[index][W];
        //take the same pieces
        int take  = 0;
        //since weight is related to sizeof the pieces and it should not exceed
        //n which is the max no. of pieces that can be taken
        if(W>=(index+1)){
            take  = price[index] + maxProfit(index,price,W-(index+1),dp);
        }
        //take different pieces
        int dontTake  = 0 + maxProfit(index-1,price,W,dp);
        return dp[index][W] =  Integer.max(take , dontTake);
        
    }
}
```

[LCS](https://www.codingninjas.com/codestudio/problems/624879?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)

`TC:O(nm), SC:O(max(n,m))`

```java
import java.util.*;
public class Solution {

	public static int lcs(String s, String t) {
        int dp[][] = new int[s.length()][t.length()];
        for(int row[] : dp) Arrays.fill(row,-1);
		return lcsUtil(s.length()-1,t.length()-1,s,t,dp);
    }
    public static int lcsUtil(int i,int j, String a, String b,int[][] dp){
        //base case
        if(i<0 || j<0) return 0;
        if(dp[i][j]!=-1) return dp[i][j];
        
        if(a.charAt(i) == b.charAt(j)){
            return dp[i][j] =  1 + lcsUtil(i-1,j-1,a,b,dp);
        }
        return dp[i][j] =  0 + Integer.max(lcsUtil(i-1,j,a,b,dp),lcsUtil(i,j-1,a,b,dp)); 
    }

}
```

[Longest increasing subsequence](https://www.codingninjas.com/codestudio/problems/630459?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)

`TC: O(nm) , sc: O(nm) + O(n), extra O(n) for stack space`

```java
 import java.util.*;
public class Solution {

    public static int longestIncreasingSubsequence(int arr[]) {
        int prev = -1;
        int dp[][] = new int[arr.length][arr.length+1]; // we are taking arr.length+1 as prev starts with -1 hence we are doing co-ordinate shift by one index.
        for(int row[] : dp)
            Arrays.fill(row,-1);
        return lengthIncreasing(0,arr,prev,dp);
    }
    public static int lengthIncreasing(int index, int[] arr,int prev,int dp[][]){
        //base case
        if(index==arr.length) return 0;
        if(dp[index][prev+1]!=-1) return dp[index][prev+1];
        //take
        int take = 0;
        if(prev ==-1 || arr[prev] < arr[index])
            take = 1 + lengthIncreasing(index+1,arr,index,dp);
        //dontTake
        int dontTake = 0  + lengthIncreasing(index+1,arr,prev,dp);
        return dp[index][prev+1] =  Integer.max(take,dontTake);
    }

}
```

# [Interleaved Strings](https://leetcode.com/problems/interleaving-string)

`TC: O(n^m) where n is length of string a, and m is length of string b`

```java
//solution 1 : recursive solution, will lead to TLE
class Solution {
    public boolean isInterleave(String a, String b, String c) {
        if(a.length() + b.length() != c.length()) return false;
        return isPossible(0,0,a,b,c);
    }
    public boolean isPossible(int i , int j, String a, String b, String c){
        ///base case
        if(i == a.length() && j == b.length()) return true;
        boolean left = false;
        boolean right = false;
        if(i< a.length() && a.charAt(i) == c.charAt(i+j)){
             left =  isPossible(i+1,j,a,b,c);
        }
        if (j < b.length() && b.charAt(j) == c.charAt(i+j)){
             right = isPossible(i,j+1,a,b,c);
        }
        return left || right;
    }
}
```

**Optimal Solution** `TC: O(n^2)`

```java
class Solution {
    public boolean isInterleave(String a, String b, String c) {
        int dp[][] = new int[a.length()+1][b.length()+1];
        for(int row[] : dp) Arrays.fill(row,-1);
        if(a.length() + b.length() != c.length()) return false;
        return isPossible(0,0,a,b,c,dp);
    }
    public boolean isPossible(int i , int j, String a, String b, String c,int[][] dp){
        ///base case
        if(i == a.length() && j == b.length()) return true;
        if(dp[i][j]!=-1) return dp[i][j]==1;
        boolean left = false;
        boolean right = false;
        if(i< a.length() && a.charAt(i) == c.charAt(i+j)){
             left =  isPossible(i+1,j,a,b,c,dp);
        }
        if (j < b.length() && b.charAt(j) == c.charAt(i+j)){
             right = isPossible(i,j+1,a,b,c,dp);
        }
        dp[i][j] = (left || right) ? 1: 0;
        return dp[i][j]==1 ? true : false;
    }
}
```