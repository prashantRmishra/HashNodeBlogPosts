# Heap(Priority Queue)

# [Find kth largest element in the array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
TC:O(NlogN) as heap sort will take nlogn time to build heap

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for(int i : nums){
            q.add(i);
            while(!q.isEmpty() && q.size() > k){
                q.remove();
            }
        }
        return q.peek();
    }
}
```
# [K max sum combination](https://www.codingninjas.com/codestudio/problems/k-max-sum-combinations_975322?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=0)
`TC: O(N^2), for using two for loops`
```java
import java.util.*;
public class Solution{
	public static ArrayList<Integer> kMaxSumCombination(ArrayList<Integer> a, ArrayList<Integer> b, int n, int k){
		// Write your code here.
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for(int i  : a)
            for(int j : b)
                if(q.size()<k)
                    q.add(i+j);
                else
                    if(q.peek() < i+j){
                        q.remove();
                        q.add(i+j);
                    }
        ArrayList<Integer> list = new ArrayList<>();
        while(!q.isEmpty()) list.add(0,q.remove()); // it will add values at index 0 only by that way values will keep on shifting
        return list;
	}
}
```
# [Merge n sorted Arrays](https://www.codingninjas.com/codestudio/problems/k-max-sum-combinations_975322?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=0)

`TC: O(N^2) + O(2*N^2log(N))`
```java
import java.util.*;
public class Solution 
{
	public static ArrayList<Integer> mergeKSortedArrays(ArrayList<ArrayList<Integer>> kArrays, int k)
	{
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for(ArrayList<Integer> l : kArrays){
            for(Integer i : l){
                q.add(i);
            }
        }
        ArrayList<Integer> list = new ArrayList<>();
        while(!q.isEmpty()){
            list.add(q.remove());
        }
        return list;
	}
}
```
# [Maximum xor from an element in the array](https://leetcode.com/problems/maximum-xor-with-an-element-from-array/)

`TC: O(N*MLogM), where N is size of queries list, M is size of arr List and LogM is the cost of adding value to priority queue`
```java
import java.util.*;
public class Solution {
    public static ArrayList<Integer> maxXorQueries(ArrayList<Integer> arr, ArrayList<ArrayList<Integer>> queries) {
        PriorityQueue<Integer> q = new PriorityQueue<>((a,b)-> b-a);
        ArrayList<Integer> result = new ArrayList<>();
        for( int i =0;i<queries.size();i++){
            int X = queries.get(i).get(0);
            int A = queries.get(i).get(1);
            for(Integer j : arr){
                if(j<=A){
                    q.add(X^j);
                }
            }
            if(!q.isEmpty()){
                result.add(q.remove());
            }
            else result.add(-1);
            q.clear();
        }
        return result;
        
    }
}
```
**This is will lead to TLE**
Hence, Optimized solution can achieve this in O(N*M), ( we won't be using priority queue)

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> maxXorQueries(ArrayList<Integer> arr, ArrayList<ArrayList<Integer>> queries) {
        ArrayList<Integer> result = new ArrayList<>();
        for( int i =0;i<queries.size();i++){
            int X = queries.get(i).get(0);
            int A = queries.get(i).get(1);
            int max = -1;
            for(Integer j : arr){
                if(j<=A){
                    max = Integer.max(max,X^j);
                }
            }
            result.add(max);
        }
        return result;
        
    }
}
```