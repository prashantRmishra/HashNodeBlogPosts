# Longest Palindromic subsequence (Same as Lcs)

Given a string `s`, find the longest palindromic subsequence's length in `s`.

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

 

**Example 1**:

```
Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".
```
**Solution**:
**Memoization approch**:
Time complexity : o(n*m) and space complexity : o(n*m) + o(n) (o(n) is extra stack space utilized)
```java
//intuition
/*
normal or brute force way: generate all possible subsequences and find the largest subsequence that is palindrome;
optimal approach : 
Using Logic of longest common subsequence;
example : if s = bbbab, lets reverse it as r = babbb
now the we can perform longest common subsequence to get result as 3 (bbb) and that will be the required result;


*/
class Solution {
    public int longestPalindromeSubseq(String s) {
        //bottom up approach
        StringBuilder sb = new StringBuilder();
        sb.append(s);
        String r= sb.reverse().toString();
        int dp[][] = new int[s.length()][r.length()];
        for(int row[]: dp){
            Arrays.fill(row,-1);
        }
        return longestPalindromicSubsequence(s.length()-1,r.length()-1,s,r,dp);
    }
    
    //Memoization approach
    public int longestPalindromicSubsequence(int i,int j, String a, String b,int dp[][]){
        if(i<0 || j<0){
            return 0;
        }
        if(dp[i][j]!=-1) return dp[i][j];
        if(a.charAt(i)==b.charAt(j)){
           return dp[i][j] = 1 + longestPalindromicSubsequence(i-1,j-1,a,b,dp);
        }
        return dp[i][j]= 0 + Integer.max(longestPalindromicSubsequence(i,j-1,a,b,dp),
                              longestPalindromicSubsequence(i-1,j,a,b,dp));
    }
}
```
**Tabulation Approach**:
Time complexity : o(n*m) and space complexity : o(n*m)

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        //bottom up approach
        StringBuilder sb = new StringBuilder();
        sb.append(s);
        String r= sb.reverse().toString();
        int dp[][] = new int[s.length()+1][r.length()+1];
        
        //base case
        //its 1 base indexing
        //lets put first row and first column as 0
        for(int i = 0;i<=s.length();i++){
            dp[i][0] = 0;
            dp[0][i] = 0;
        }
        for(int i =1;i<=s.length();i++){
            for(int j =1;j<=r.length();j++){
                if(s.charAt(i-1)==r.charAt(j-1)){
                    dp[i][j] = 1 + dp[i-1][j-1];
                }
                else dp[i][j] = 0 + Integer.max(dp[i][j-1],dp[i-1][j]);
            }
        }
        return dp[s.length()][r.length()];
    }
}
```