# Minimum number of Insertion and deletion needed to convert String A to String B or make both the strings equal (same as lcs)

**Problem**:
Given two strings str1 and str2. The task is to remove or insert the minimum number of characters from/in str1 so as to transform it into str2. It could be possible that the same character needs to be removed/deleted from one point of str1 and inserted to some another point.

**Example 1**:

```
Input: str1 = "heap", str2 = "pea"
Output: 3
```
**Solution**:
```
Intuition : Find the largest common subsequence of the two strings and substract it from 
the length of the two strings and finally add the subtractions. i.e. m+n - 2 *lcs(StringA, StringB)
```
**Bottom up Approach(Memoization)** : 
Time complexity : `O(m*n)` where is `m` and `n` is the length of two strings `a` and `b`
space complexity : o(m*n) for using `2d dp` and `O(m+n)` for auxiliary stack space because in worst case we will make `m+n` recursive calls.

[Coding Ninjas Problem](https://www.codingninjas.com/codestudio/problems/can-you-make_4244510?source=youtube&campaign=striver_dp_videos&utm_source=youtube&utm_medium=affiliate&utm_campaign=striver_dp_videos&leftPanelTab=0)
```java
import java.util.*;
public class Solution {
    public static int canYouMake(String str, String ptr) {
        // Write your code here.
        // we will use bottom up approach for solving this problem.
        int dp[][] =new int[str.length()][ptr.length()];
        for(int row[] : dp){
            Arrays.fill(row,-1);
        }
        return str.length()+ptr.length() - 2 *lcs(str,ptr, str.length()-1, ptr.length()-1,dp);
    }
    public static int lcs(String str, String ptr, int m,int n,int dp[][]){
        if(m<0 || n<0){
            return 0;
        }
        if(dp[m][n]!=-1) return dp[m][n];
        if(str.charAt(m)==ptr.charAt(n)){
            dp[m][n] = 1 + lcs(str,ptr,m-1,n-1,dp);
        }
        else{
            dp[m][n] = 0 +  Integer.max(lcs(str,ptr,m-1,n,dp), lcs(str,ptr,m,n-1,dp));
        }
        return dp[m][n];
    }

}
```
**Top Down Approach(Tabulation)** : 
Time complexity : `O(m*n)` where is `m` and `n` is the length of two strings `a` and `b`
space complexity : o(m*n) for using `2d dp`

[GeeksForGeeksProblem](https://practice.geeksforgeeks.org/problems/minimum-number-of-deletions-and-insertions0209/1)
```java
class Solution
{
	public int minOperations(String str1, String str2) 
	{ 
	   // we will use top down approach to solve this 
	   int dp[][] =new int[str1.length()+1][str2.length()+1];
	   for(int i =0;i<=str1.length();i++){
	       dp[i][0] = 0;
	   }
	   for(int i =0;i<=str2.length();i++){
	       dp[0][i] = 0;
	   }
	   for(int i =1;i<=str1.length();i++){
	       for(int j =1;j<=str2.length();j++){
	           if(str1.charAt(i-1)==str2.charAt(j-1)){
	               dp[i][j] = 1 + dp[i-1][j-1];
	           }
	           else dp[i][j] = Integer.max(dp[i-1][j],dp[i][j-1]);
	       }
	   }
	   return str1.length() + str2.length() - 2 * dp[str1.length()][str2.length()];
    }
}
```