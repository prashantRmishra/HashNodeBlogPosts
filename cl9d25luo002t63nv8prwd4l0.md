# Recursion

# [Subset sum](https://practice.geeksforgeeks.org/problems/subset-sums2234/1)

`TC: O(2^n) as we have to two choices at every index of the arr either to take that index or not`

```java

//User function Template for Java//User function Template for Java
class Solution{
    ArrayList<Integer> subsetSums(ArrayList<Integer> arr, int N){
        // code here
        ArrayList<Integer> list = new ArrayList<>();
        getSum(N-1,arr,0,list);
        return list;
    }
    public void getSum(int index,List<Integer> arr,int sum,ArrayList<Integer> list){
        if(index<0){
            list.add(sum);
            return ;
        }
        //take
        sum+=arr.get(index);
        getSum(index-1,arr,sum,list);
        sum-=arr.get(index);
        getSum(index-1,arr,sum,list);
        //donttake
    }
}
```
# [Subset sum II](https://www.codingninjas.com/codestudio/problems/unique-subsets_3625236?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)
`Tc: O(2^n)*k, where k is the size of the subsets that you would need to put into list (as putting subsets in list is not constant hence assuming that the average length of the subset l is k)`

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static ArrayList<ArrayList<Integer>> uniqueSubsets(int n, int arr[]) {
        Arrays.sort(arr);
        ArrayList<ArrayList<Integer>> list=  new ArrayList<>();
        getUnique(0,arr,new ArrayList<>(),list);
        return list;
    }
    public static void getUnique(int index,int arr[],ArrayList<Integer> l,ArrayList<ArrayList<Integer>> list){
        list.add(new ArrayList<>(l));
        for(int i = index;i<arr.length;i++){
            if( index!=i && arr[i] == arr[i-1]) continue;
            l.add(arr[i]);
            getUnique(i+1,arr,l,list);
            l.remove(l.size()-1);
        }
    }

}
```
# [Combination sum i.e. using concept of subset sum I](https://www.codingninjas.com/codestudio/problems/759331?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=0)

`Tc: O(2^n)*k where exponential for using recursion and for every call stack we will be adding subset(of assumed average length of k) in to list`

```java

import java.util.*;
public class Solution 
{
    public static ArrayList<ArrayList<Integer>> findSubsetsThatSumToK(ArrayList<Integer> arr, int n, int k) 
	{
        ArrayList<ArrayList<Integer>> list = new ArrayList<>();
        getSums(0,arr,new ArrayList<>(),k,list);
        return list;
    }
    public static void getSums(int index,ArrayList<Integer> arr,ArrayList<Integer> l,int target,ArrayList<ArrayList<Integer>> list){
        if(index>=arr.size()){ 
            if(target==0) list.add(new ArrayList<>(l));
            return;
        }
        //take
        l.add(arr.get(index));
        getSums(index+1,arr, l,target-arr.get(index),list);
        l.remove(l.size()-1);
        //dontTake
        getSums(index+1,arr,l,target,list);
    }
}
```
# [Combination sum II i.e. using the concept of subset sum II  ](https://www.codingninjas.com/codestudio/problems/1112622?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)

`TC: O(2^n) *K, same as combination I`
```java
import java.util.*;

public class Solution 
{
    public static ArrayList<ArrayList<Integer>> combinationSum2(ArrayList<Integer> arr, int n, int target)
    {
        Collections.sort(arr);
        ArrayList<ArrayList<Integer>> list  = new ArrayList<>();
        getUniqueSubsetEqualsTarget(0,arr,target,new ArrayList<>(),list);
        return list;
    }
    public static void getUniqueSubsetEqualsTarget(int index,ArrayList<Integer> arr,int target,ArrayList<Integer> l,ArrayList<ArrayList<Integer>> list){
        if(target==0) list.add(new ArrayList<>(l));
        for(int i = index;i<arr.size();i++){
            if(i!=index && arr.get(i)==arr.get(i-1)) continue;
            if(arr.get(i)<=target){
                l.add(arr.get(i));
                getUniqueSubsetEqualsTarget(i+1,arr,target-arr.get(i),l,list);
                l.remove(l.size()-1);
            }
        }
    }
}
```
# [Palindrome patitioning](https://www.codingninjas.com/codestudio/problems/799931?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)

`TC: O(2^n)*k because of the same approach as Combination sum`

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static List<List<String>> partition(String s) {
        // Write your code here.
        List<List<String>> list = new ArrayList<>();
        getAllPalindromicSubstrings(0,s,new ArrayList<>(),list);
        return list;
    }
    public static void getAllPalindromicSubstrings(int index, String s,
                                                  List<String> l,List<List<String>> list){
        if(index>=s.length()){
            list.add(new ArrayList<>(l));
            return;
        }
        for(int i = index;i<s.length();i++){
            if(!isPalindrome(index,i,s)) continue;
            l.add(s.substring(index,i+1));
            getAllPalindromicSubstrings(i+1,s,l,list);
            l.remove(l.size()-1);
        }
    }
    public static boolean isPalindrome(int start,int end, String s){
        while(start<=end){
            if(s.charAt(start++)!=s.charAt(end--)) return false;
        }
        return true;
    }
}
```
# [Kth Permutation sequence](https://leetcode.com/problems/permutation-sequence/)

`TC: O(2^n)`

```java
import java.util.*;

public class Solution {
    static ArrayList<Integer> l;
    static int count;
    public static String kthPermutation(int n, int k) {
        l  = null;
        count = 0;
        int arr[] = new int[n];
        for(int i =0;i<arr.length;i++){
            arr[i] = i+1;
        }
        boolean check[] = new boolean[arr.length];
        String s = "";
        if(getAllPermutations(arr,new ArrayList<>(), check, k))
        for(Integer i : l){
            s+=i;
        }
        return s;
    }
    
    public static boolean getAllPermutations(int[] arr, List<Integer> list, boolean[] check, int k){
        if(list.size()==arr.length){
            count++;
            if(count ==k){
                l = new ArrayList<>(list);
                return true;
            }
        }
        for(int i =0;i<arr.length;i++){
            if(!check[i]){
                check[i] = true;
                list.add(arr[i]);
                if(getAllPermutations(arr,list,check,k)) return true;
                list.remove(list.size()-1);
                check[i] = false;
            }
        }
        return false;
    }
}
```
