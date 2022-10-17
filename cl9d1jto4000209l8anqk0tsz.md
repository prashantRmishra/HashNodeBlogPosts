# Recursion and backtracking

# [All Permutations of the string](https://leetcode.com/problems/permutations/)

`Tc: O(2^n)`

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static List<String> findPermutations(String s) {
        List<String> list = new ArrayList<>();
        char ch[] = s.toCharArray();
        int check[]  =  new int[s.length()];
        getAllPermutations(ch,"",list,check);
        return list;
    }
    public static void getAllPermutations(char[] ch, String s, List<String> list,int[] check){
        if(s.length() == ch.length) {
            list.add(s);
            return;
        }
        for(int i =0;i<ch.length;i++){
            if(check[i] ==0){
                check[i] = 1;
                s+=ch[i];
                getAllPermutations(ch,s,list,check);
                s = s.substring(0,s.length()-1);
                check[i] = 0;
            }
        }
    }
}
```
# [Rat in a maze](https://practice.geeksforgeeks.org/problems/rat-in-a-maze-problem/1)

`TC: O(4^n) as we have 4 choices at every instance, move left, right, top and bottom`
```java
// m is the given matrix and n is the order of matrix
class Solution {
    public static ArrayList<String> findPath(int[][] m, int n) {
        ArrayList<String> list = new ArrayList<>();
        getPath(0,0,m,n,"",list);
        if(list.isEmpty()) list.add(("-1"));
        
        return list;
    }
    public static void getPath(int i, int j, int [][]m, int n, String s, ArrayList<String> list){
        if(i==n-1 && j==n-1 && m[i][j]==1) {
            list.add(s);
            return;
        }
        if(i<0 || j<0 || i>=n || j>=n || m[i][j]==0 || m[i][j] ==2) return;
        m[i][j] = 2;
        //left
        s+="L";
        getPath(i,j-1,m,n,s,list);
        s = s.substring(0,s.length()-1);
          //right
        s+="R";
        getPath(i,j+1,m,n,s,list);
        s = s.substring(0,s.length()-1);
        //top
        s+="U";
        getPath(i-1,j,m,n,s,list);
        s = s.substring(0,s.length()-1);
        //bottom
        s+="D";
        getPath(i+1,j,m,n,s,list);
        s = s.substring(0,s.length()-1);
        m[i][j] = 1;
        
    }
}
```

# [Word break II similar to palindrome partitioning](https://www.codingninjas.com/codestudio/problems/983635?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)
[Palindrome partitioning](https://dev.to/prashantrmishra/recursion-4301)
`TC: O(2^n)`
```java
import java.util.* ;
import java.io.*; 
import java.util.*;

public class Solution {

	public static ArrayList<String> wordBreak(String s, ArrayList<String> dictionary) {
		// Write your code here.
        ArrayList<String> list = new ArrayList<>();
        HashSet<String> set  = new HashSet<>();
        for(String str  : dictionary) set.add(str);
        getWords(0,s,"",list,set);
        //System.out.println(list);
        return list;
	}
    public static void getWords(int index,String s , String temp, ArrayList<String> list,HashSet<String> set){
        if(index>=s.length()){
            list.add(temp.trim());
            return;
        }
        for(int i = index;i<s.length();i++){
            if(!set.contains(s.substring(index,i+1))) continue;
            int len = temp.length();
            temp+=s.substring(index,i+1)+" ";
            getWords(i+1,s,temp,list,set);
            temp = temp.substring(0,len);
        }
    }
}
```

# [M-coloring](https://practice.geeksforgeeks.org/problems/m-coloring-problem-1587115620/1#)

`Expected Time Complexity: O(M^N)`
```java
class solve {
    // Function to determine if graph can be colored with at most M colors
    // such
    // that no two adjacent vertices of graph are colored with same color.
    public boolean graphColoring(boolean graph[][], int m, int n) {
        int colorArr[] = new int[n];
       
      
        return mColoringPossible(0,colorArr,n,m,graph);
    }
    public boolean mColoringPossible(int node, int colorArr[],int n,int m, boolean[][]graph){
        if(node>=n) return true;
        for(int i =1;i<=m;i++){
            if(canColor(node,i,colorArr,graph)){
                colorArr[node] = i;
                if(mColoringPossible(node+1,colorArr,n,m,graph)) return true;
                colorArr[node] =0;
            }
        }
        return false;
    }
    public boolean canColor(int node, int nodeColor,int[] colorArr, boolean[][] graph){
        for(int col =0;col<graph.length;col++){
            if(graph[node][col]){
                if(colorArr[col] ==nodeColor) return false;
            }
        }
        return true;
    }
}
```
