# Minimum Insertion steps needed to make a string palindrome (Same as LCS)

**Problem**:
Given a string `s`. In one step you can insert any character at any index of the string.

Return the minimum number of steps to make s palindrome.

A Palindrome String is one that reads the same backward as well as forward.

 

**Example 1**:

```
Input: s = "zzazz"
Output: 0
Explanation: The string "zzazz" is already palindrome we don't need any insertions. 
```
**Solution**:
**Tabulation approach**
Time complexity : O(n*m), space complexity : o(n*m) for `dp` array
```java
class Solution {
    public int minInsertions(String s) {
        //going with the hint given 
        //i.e. finding length of the longest palindromic subsequence x
        //now , s.length()-x = n ,so n insertion are needed to make the entire string as palindromic string
        String r = new StringBuilder(s).reverse().toString();
        int dp[][] = new int[s.length()+1][r.length()+1];// 1 based indexing
        for(int i =0;i<=s.length();i++){
            dp[i][0] =0;
            dp[0][i] =0;
        }
        for(int  i =1;i<=s.length();i++){
            for(int j=1;j<=r.length();j++){
                if(s.charAt(i-1)==r.charAt(j-1)){
                    dp[i][j] = 1 + dp[i-1][j-1];
                }
                else dp[i][j] = Integer.max(dp[i][j-1],dp[i-1][j]);
            }
        }
        return s.length() - dp[s.length()][r.length()];
    }
}
```