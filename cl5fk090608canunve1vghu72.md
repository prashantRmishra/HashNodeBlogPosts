# Circular Sorting Algorithm

**Circular sorting**

Pre-requisite : elements of the array should be between 1 to length of the array
In `Circular Sorting` elements are placed at their natural indexs 
Example:` if the array is [5,4,2,1,3] => [1,2,3,4,5]`
```java
class Solution {
    public List<Integer> findDuplicates(int[] nums){
        int i =0,n  = nums.length;
        while(i<n){
            int j = nums[i]-1;
            if(nums[i]!=nums[j]){
                swap(nums,i,j);
            }
            else i++;
        }
       
    }
    public void swap(int[] nums,int i ,int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
```

**Usage**:

- It's helpful in identifying duplicates in the array

- It can also be used to find missing elements in the array
