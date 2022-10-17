# Arrays


# [Largest sum contiguous sub-array](https://practice.geeksforgeeks.org/problems/kadanes-algorithm-1587115620/1)

```java
class Solution{
    // arr: input array
    // n: size of array
    //Function to find the sum of contiguous subarray with maximum sum.
    long maxSubarraySum(int arr[], int n){
        // Your code here
        //we will use kadane's algorithms for solving this problem
        long max = Integer.MIN_VALUE;
        long currentMax = 0;
        for(int i =0;i<arr.length;i++){
            currentMax+=arr[i];
            if(currentMax>max) max = currentMax;
            if(currentMax<0) currentMax=0;
        }
        return max;
    }
}
```
[Search in a row-wise and column wise sorted matrix](https://practice.geeksforgeeks.org/problems/search-in-a-matrix17201720/1)

```java
class Sol
{
    public static int matSearch(int mat[][], int N, int M, int X)
    {
        for(int i =0;i<mat.length;i++){
            if(binarySearch(mat[i],0,mat[i].length-1,X) ==1) return 1;
        }
        return 0;
    }
    public static int binarySearch(int arr[], int start, int end, int target){
    
        if(start<=end){
        int mid = start + (end-start)/2;
        if(arr[mid] == target) return 1;
        if(arr[mid] < target) return binarySearch(arr, mid+1, end, target);
        else return binarySearch(arr,start,mid-1, target);
        }
        return 0;
    }
}
```
# [Program for Array rotation](https://practice.geeksforgeeks.org/problems/rotate-array-by-n-elements-1587115621/1)

Brute force Approach(This will lead to TLE)
```java
class Solution
{
    //Function to rotate an array by d elements in counter-clockwise direction. 
    static void rotateArr(int arr[], int d, int n)
    {
        for(int i =0;i<d;i++){
            rotate(arr);
        }
    }
    public static void rotate(int arr[]){
         int i =0;
         int temp = arr[i];
         while(i<arr.length-1){
             arr[i] = arr[i+1];
             i++;
         }
         arr[arr.length-1] = temp;
    }
}
```
Optimized Approach
TC : O(n) , space complexity : O(N) for using extra space for result array

```java
class Solution
{
    //Function to rotate an array by d elements in counter-clockwise direction. 
    static void rotateArr(int arr[], int d, int n)
    {
        int i = 0,j =d % n;
        int result[]  = new int[n];
        while(j<n){
            result[i++] = arr[j++];
        }
        int index= 0;
        while(index<d && i < n){
            result[i++] = arr[index++];
        }
        for(int k =0;k<n;k++) arr[k]  = result[k];
    }
}
```
# [Print given matrix in a spiral form](https://practice.geeksforgeeks.org/problems/spirally-traversing-a-matrix-1587115621/1)

Time complexity: O(MxN) for traversing the matrix, space complexity: O(MxN) for using ArrayList<Integer> which will be of size MxN 

```java
class Solution
{
    //Function to return a list of integers denoting spiral traversal of the matrix.
    static ArrayList<Integer> spirallyTraverse(int matrix[][], int r, int c)
    {
        // code here 
        //top down right bottom 
        ArrayList<Integer> list = new ArrayList<>();
        int topLeft = 0,
        topRight= c-1,
        bottomLeft = 0, bottomRight =r-1;
        int chance = 0;//1,2,3
        while(topLeft<=bottomRight && bottomLeft <=topRight){
            if(chance ==0){
                //move left to right
                for(int i = bottomLeft;i<=topRight;i++){
                    list.add(matrix[topLeft][i]);
                }
                topLeft++;
                chance =1;
            }
            else if(chance ==1){
                //move from top to bottom
                for(int i = topLeft;i<=bottomRight;i++){
                    list.add(matrix[i][topRight]);
                }
                topRight--;
                chance = 2;
            }
            else if(chance ==2){
                //move right to left
                for(int i = topRight;i>=bottomLeft;i--){
                    list.add(matrix[bottomRight][i]);
                }
                bottomRight--;
                chance = 3;
            }
            else {
                //move from bottom to top
                for(int i = bottomRight;i>=topLeft;i--){
                    list.add(matrix[i][bottomLeft]);
                }
                bottomLeft++;
                chance = 0;
            }
        }
        return list;
    }
}
```
[Count Pair with given sum] ()

Solution 1: We can use o(n^2) solution, using two for loops to find pairs that will add up to the target.
Solution 2: We can also use backtracking to find all the possible pairs whose sum is the target (time complexity will be (2^n) as every index will have two choices take or not take

```java
class Solution {
    int getPairsCount(int[] arr, int n, int k) {
        //ArrayList<Integer> list = new ArrayList<>();
        //return pairs(0,arr,k,list);
        Arrays.sort(arr);
        int i =0, j = n-1;
        while(i<j){
            if(arr[i]+arr[j] ==k) {
                
            }
        }
    }
    public int pairs(int index,int[] arr, int target,ArrayList<Integer> list){
        //base case 
       
        if(index >= arr.length){
                 if(target==0){
                     //System.out.println("here" +list.size());
                     return list.size() ==2 ? 1 : 0;
                 }
            return 0;
        }
        if(target==0){ 
            //System.out.println("here" + list.size());
            return list.size() ==2 ? 1 : 0;
        }
        //let's take it
        int left  =0;
        if(target>=arr[index]){ 
          list.add(arr[index]);
          left = pairs(index+1,arr,target-arr[index],list); 
          //don't take, I am adding it here because of the if condition we have used
          list.remove(list.size()-1);
        }
        int right = pairs(index+1,arr,target,list);
        return left + right;
    }
}
```

But both of this solution will lead to TLE

Optimal Solution : 
_Using HashMap_

TC: O(n)
```java
class Solution {
    int getPairsCount(int[] arr, int n, int k) {
        // we will use a simple hashing technique,
        // first we will find the frequency of all the elements int the array.
        Map<Integer,Integer> map = new HashMap<>();
        for(int i : arr) map.put(i,map.getOrDefault(i,0)+1);
        //we will increment count value based on value of (k-arr[i]) as key in the 
        //map
        //for example we have k =6, arr=1,5,0,
        // there is only one pair 1,5 that form 6
        //map = 1=1,5=1,0=1
        // so, map.get(6-1) = map.get(5) = 1
        //map.get(6-5) = map.get(1) = 1;
        //map.get(6-0) = map.get(6) = 0;
        //total = 1+1+0 = 2;
        //hence result will be 2/2 = 1 (we divided the result by 2 because it contains duplicate
        // as (1,5) and (5,1) hence only one uniqueue pair is taken into consideration)
        int count =0;
        for(int i =0;i<n;i++){
            count  = count + map.getOrDefault(k-arr[i],0);
            //we have to avoid pair of the same index element like( arr[i],arr[i])
            if(k == arr[i]+arr[i]) count--;
        }
        return count/2;
    }
}

```

# [Trapping rain water problem](https://practice.geeksforgeeks.org/problems/trapping-rain-water-1587115621/1)


TC : O(n) and space complexity = O(2n)~O(n)  for using leftMaxima and rightMaxima arrays
```java
class Solution{
    
    // arr: input array
    // n: size of array
    // Function to find the trapped water between the blocks.
    static long trappingWater(int arr[], int n) { 
        // Your code here
        int[] leftMaxima = new int[n];
        int[] rightMaxima = new int[n];
        int currentMaxima = Integer.MIN_VALUE;
        for(int i =0;i<n;i++){
            if(currentMaxima < arr[i]) {
                currentMaxima = arr[i];
                leftMaxima[i] = arr[i];
            }
            else
                leftMaxima[i] = currentMaxima;
        }
        currentMaxima = Integer.MIN_VALUE;
        for(int i = n-1;i>=0;i--){
            if(currentMaxima < arr[i]){
                 currentMaxima = arr[i];
                 rightMaxima[i] = arr[i];
            }
            else
                rightMaxima[i] = currentMaxima;
        }
        int loggedWater = 0;
        for(int i =0;i<n;i++){
            loggedWater+=Integer.min(leftMaxima[i],rightMaxima[i])-arr[i];
        }
        return loggedWater;
    } 
}
```
# [Remove duplicate from the sorted array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

```java
//TC: o(N) + o(N) for two passes,space complexity : o(N) for using list
//not optimal
class Solution {
    public int removeDuplicates(int[] nums) {
      
        List<Integer> list = new ArrayList<>();
        int count = 0;
        for(int i : nums)
            if(!list.contains(i))
               list.add(i);
        int index = 0;
        for(Integer val: list){
            nums[index] = val;
            index++;
        }
        return list.size();
    }
}
//TC: o(N) , space complexity: O(1)
//optimized
class Solution {
    public int removeDuplicates(int[] nums){
        int i =0,j = i+1;
        while(j<nums.length){
            if(nums[i] == nums[j]){
                j++;
                continue;
            }
            if(nums[i] !=nums[j]){
                int temp = nums[j];
                nums[i+1] = nums[j];
                nums[j] = temp;
                i++;
            }
        }
        return i+1;
    }
}
```

# [Maximum product in contiguous subarray](https://www.codingninjas.com/codestudio/problems/1115474?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)
TC:O(n),SC:constant
**Inspired from Kadane's algorithm**
```java
import java.util.ArrayList;

public class Solution {
	public static int maximumProduct(ArrayList<Integer> list, int n) {
		int a,b,c;
        a=b=c= list.get(0);
        for(int i =1;i<list.size();i++){
            int currentVal = list.get(i);
            int temp = Integer.max(currentVal, Integer.max(a*currentVal,b*currentVal));
            b = Integer.min(currentVal, Integer.min(a*currentVal,b*currentVal));
            a = temp;
            c = Integer.max(a,c);
        }
        return c;
	}
}
```

# [Generate Pascal's triangle](https://leetcode.com/problems/pascals-triangle/)
`TC: O(NM) where N is the no. of rows and M is max of elements in the row`
```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        // we will use simple approach
        List<List<Integer>> list = new ArrayList<>();
        for(int i =1;i<=numRows;i++){ // to keep track of row of the pyramid
            List<Integer> l = new ArrayList<>();
            int size = i;
            for(int j =0;j<size;j++){
                if(j==0) l.add(1); // it its the first index it will have 1
                
                else{// for any index between first and last 
                    //get the last row list
                    List<Integer> temp = list.get(list.size()-1);  
                    while(j<size-1){ // we will run while loop before last index
                        l.add(temp.get(j) + temp.get(j-1)); // think about it what it does,
                        /*
                            it the repvious row had 1 2 2 1
                            next row will have     1 3 3 3 1 this is what line 15 calculates 
                        */
                        j++; 
                    }
                    
                    l.add(1); // it is last index it will have value 1
                }
            }
            list.add(l);
             System.out.println(list + " size "+ size);
        }
        return list;
    }
}
```

# [Sort 0,1 and 2](https://leetcode.com/problems/sort-colors/)
Approach 1 : TC : O(NlogN)
```java
class Solution {
    public void sortColors(int[] nums) {
        //way one we can use hashmap for solving this
        Map<Integer,Integer> map = new TreeMap<>();
        for(int i =0;i<nums.length;i++) map.put(nums[i],map.getOrDefault(nums[i],0)+1);
        
        
        int index = 0;
        for(Map.Entry<Integer,Integer> entry: map.entrySet()){
            for(int i =0;i<entry.getValue();i++){
                nums[index++] = entry.getKey();
            }
        }
    }
}
```
Approach 2 : TC : O(n)
[Strivers sde sheet solution](https://takeuforward.org/data-structure/sort-an-array-of-0s-1s-and-2s/)

[Merge intervals](https://leetcode.com/problems/merge-intervals/)

Optimal solution : TC: O(NlogN) + O(N) , nlogn for sorting and n for iterating

```java
class Solution {
    public int[][] merge(int[][] intervals) {
    
        if(intervals.length ==0 || intervals ==null) return new int[0][];
        
        Arrays.sort(intervals,(a,b)->a[0]-b[0]); // this is great snippet for sorting 
        List<int[]> list = new ArrayList<>();
        int start = intervals[0][0];
        int end = intervals[0][1];
        for(int i[] : intervals){
            if(end >= i[0]){
                end = Integer.max(end,i[1]);
            }
            else{
                list.add(new int[]{start,end});
                start  = i[0];
                end = i[1];
            }
        }
        list.add(new int[]{start,end});
        return list.toArray(new int[0][]);
    } 
}
```

# [Finding missing and repeating number](https://www.codingninjas.com/codestudio/problems/873366?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=0)

```java
import java.util.*;


//Solution 1 : using circular sorting (Leads to TLE)
// public class Solution {

//     public static int[] missingAndRepeating(ArrayList<Integer> arr, int n) {
//         // Write your code here
//         //we can use  circular sorting for this
//         //remember circular sorting is only applicable , if the if the values in the array are in the range of length of the array.
//         int i =0;
//         while(i<n){
//             int j = arr.get(i)-1;
//             if(arr.get(j)!=arr.get(i)){
//                 swap(arr,i,j);
//             }
//             else i++;
//         }
//         int result[] = new int[2];
//         for(int k =0;k<n;k++){
//             if(arr.get(k)!=k+1){
//                 result[0] = k+1;
//                 result[1] = arr.get(k);
//                 break;
//             }
//         }
//         return result;
       
//     }
//      public static void swap(ArrayList<Integer> arr,int i,int j){
//             int temp = arr.get(i);
//             arr.set(i,arr.get(j));
//             arr.set(j,temp);
//         }
// }
//Solution 2 : using HashMap TC: O(n)
public class Solution{
    public static int[] missingAndRepeating(ArrayList<Integer> arr,int n){
        HashMap<Integer,Integer> map = new HashMap<>();
        int result[]= new int[2];
        Arrays.fill(result,-1);
        
        for(Integer i : arr) map.put(i,map.getOrDefault(i,0)+1);
        //stem.out.print(map);
        for(int i =0;i<n;i++){
            if(map.containsKey(i+1)){
                if(map.get(i+1) > 1)
                    result[1] = (i+1);
            }
            else result[0] = i+1;
            if(result[0]!=-1 && result[1]!=-1) break;
        }
        return result;
    }
}


```

# [ Pow(x, n)](https://leetcode.com/problems/powx-n/)

TC:O(logn)

```java
//this is naive approach its not optimal its gonna take O(N) time
// class Solution {
//     public double myPow(double x, int n) {
//         double result = 1.0;
        
//         for(int i =0;i<Math.abs(n);i++){
//             result = result*x;
//         }
//         if(n<0) return 1/result;
//         return result;
//     }
// }
//we will use something called binary exponential approach
//this uses the concept of odd and even exponential 
//if x^n if n is even it can be written as (x^2)^(n/2) 
//if x^n is odd then it can be written as x* (x^(n-1)) here (x^(n-1)) is again even so same approach
//solution:
class Solution{
    public double myPow(double x, int n){
        double result =1.0; 
        long nn =n;
        if(n<0) nn = -1*nn;
        while(nn > 0){
            if(nn%2==0){
                x =  x*x;
                nn = nn/2;
            }
            else{
                result = result*x;
                nn = nn-1;
            }
        }
        if(n<0) result = (double)( 1.0)/(double)(result);
        return result;
        
    }
}
``` 

# [Count Inversion](https://www.codingninjas.com/codestudio/problems/615?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1) 

`TC: O(NLogN) as we are using merge sort as optimal solution, brute force will take O(N^2) `

```java
import java.util.* ;
import java.io.*; 
/*
// Approach 1: brute force approach
public class Solution {
    public static long getInversions(long arr[], int n) {
        // Write your code here.
        long result = 0;
        for(int i =0;i<arr.length;i++){
            for(int j =i+1;j<arr.length;j++){
                if(arr[i] > arr[j]) result++;
            }
        }
        return result;
    }
}
*/
//using merge sort time complexity is : O(nlogn)
public class Solution{
    public static long getInversions(long arr[],int n){
        return merge(0,n-1,arr);
    }
    public static long merge(int start, int end, long[] arr){
        long count = 0;
      
        if(end > start){
            int mid = start + (end-start)/2;
            count+=merge(start,mid,arr);
            count+=merge(mid+1,end,arr);
            count+=sort(start,mid,end,arr);
        }
        return count;
    }
    public static long sort(int s,int m,int e, long[] arr){
        long[] p = new long[m-s+1];
        long[] q = new long[e-m];
        for(int i =0;i<p.length;i++) p[i] = arr[i+s];
        for(int i =0;i<q.length;i++) q[i] = arr[i+m+1];
        int i=0,j=0,k=s;
        long count = 0;
        while(i<p.length && j < q.length){
            if(p[i]<=q[j]){
                arr[k] = p[i];
                k++;i++;
            }
            else{
                arr[k] = q[j];
                count+=p.length-i; // just think about it
                //if p[i] > q[j] then all the elements after ith index in p array
                //will also be greater than q[j] because p and q are sorted
                //hence total pairs = totalNoOfElements between ith index and last index including ith and last element as well = p.length-i;
                k++;j++;
            }
        }
        while(i<p.length){
            arr[k] = p[i];
            i++;k++;
        }
        while(j<q.length){
            arr[k]=q[j];
            j++;k++;
        }
        return count;
    }
}
```

# [Rerverse Pair](https://leetcode.com/problems/reverse-pairs/)

`TC: O(NlogN)`

```java
class Solution {
    public int reversePairs(int[] nums) {
        return merge(0,nums.length-1,nums);
    }
    public  int merge(int start, int end, int[] arr){
        int count = 0;

        if(end > start){
            int mid = start + (end-start)/2;
            count+=merge(start,mid,arr);
            count+=merge(mid+1,end,arr);
            count+=sort(start,mid,end,arr);
        }
        return count;
    }
    public  int sort(int s,int m,int e, int[] arr){
        int[] p = new int[m-s+1];
        int[] q = new int[e-m];
        int count =0;
        int t = m+1;
        for(int u =s;u<=m;u++){
            while(t<=e && arr[u] > (long) 2*arr[t]){
                t++;
            }
            count+=t-(m +1);
        }
        
        for(int i =0;i<p.length;i++) p[i] = arr[i+s];
        for(int i =0;i<q.length;i++) q[i] = arr[i+m+1];
        int i=0,j=0,k=s;
        while(i<p.length && j < q.length){
            if(p[i]<=q[j]){
                arr[k] = p[i];
                k++;i++;
            }
            else{
                arr[k] = q[j];
                k++;j++;
            }
        }
        while(i<p.length){
            arr[k] = p[i];
            i++;k++;
        }
        while(j<q.length){
            arr[k]=q[j];
            j++;k++;
        }
        return count;
    }
}
```

