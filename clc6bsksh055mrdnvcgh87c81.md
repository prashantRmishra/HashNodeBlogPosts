# String


# [Reverse words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

TC `O(n)` where n is number of words in the String

```java
import java.util.StringJoiner;
class Solution {
    public String reverseWords(String s) {
        String[] ch = s.split(" ");
        int i =0,j=ch.length/2;
        while(i<j){
            String temp = ch[i];
            ch[i] = ch[ch.length-i-1];
            ch[ch.length-i-1] = temp;
            i++;
        }
        StringJoiner sJoiner = new StringJoiner(" ");
        for(int k =0;k<ch.length;k++){
            if(!ch[k].equals(""))
                sJoiner.add(ch[k]);
        }
        return sJoiner.toString().trim();
    }
}
```
# [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
TC: O(n), where n is size of the String.
```java
class Solution {
    public int romanToInt(String s) {
        HashMap<Character,Integer> map =new HashMap<>();
        map.put('I',1);
        map.put('V',5);
        map.put('X',10);
        map.put('L',50);
        map.put('C',100);
        map.put('D',500);
        map.put('M',1000);
        int integerValue = 0;
        int i = s.length()-1;
        while(i>0){
            if(s.charAt(i)=='V' && s.charAt(i-1)=='I'){
                integerValue+=4;
                i=i-2;
            }
            else if(s.charAt(i)=='X' && s.charAt(i-1)=='I'){
                integerValue+=9;
                i = i-2;
            }
            else if(s.charAt(i)=='L' && s.charAt(i-1)=='X'){
                integerValue+=40;
                i = i-2;
            }
            else if(s.charAt(i)=='C' && s.charAt(i-1)=='X'){
                integerValue+=90;
                i = i-2;
            }
            else if(s.charAt(i)=='D' && s.charAt(i-1)=='C'){
                integerValue+=400;
                i = i-2;
            }
            else if(s.charAt(i)=='M' && s.charAt(i-1)=='C'){
                integerValue+=900;
                i = i-2;
            }
            else{
                
                integerValue+=map.get(s.charAt(i));
                i--;
            }
            
        }
        if(i==0) integerValue+=map.get(s.charAt(i));
        return integerValue;
    }
}


```

# [Sentences that contains all the given phrases](https://www.geeksforgeeks.org/sentence-that-contains-all-the-given-phrases/)

Note: I was asked this question in MSCI coding interview question

```java
/*package whatever //do not write package name here */

import java.io.*;
import java.util.*;

class GFG {
	public static void main (String[] args) {
		List<String> sentence= new ArrayList<>();
		List<String> queries = new ArrayList<>();
		sentence.add("bob and alice like to text each other");
		sentence.add("bob does not like to ski but does not like to fall");
		sentence.add("Alice likes to ski");
		
		queries.add("bob alice");
		queries.add("alice");
		queries.add("like");
		queries.add("non occurance");
		System.out.println(get(sentence,queries));
		
	}
	public static List<List<Integer>> get(List<String> sentence, List<String> q){
    	List<List<Integer>> list = new ArrayList<>();
    	for(int j =0;j<q.size();j++){
    	        List<Integer> res = new ArrayList<>();
    	        String str[] = q.get(j).split(" ");
    	        for(int i =0;i<sentence.size();i++){
    	            String sen = sentence.get(i);
    	            boolean present = true;
    	            for(String s : str){
    	                if(!sen.contains(s)){
    	                    present = false;
    	                    break;
    	                    
    	                }
    	            }
    	            if(present){
    	                res.add(i);// sentence index that had s got added
    	            }
    	        }
    	        if(res.isEmpty()){
    	            List<Integer> temp = new ArrayList<>();
    	            temp.add(-1);
    	            list.add(temp);
    	        }
    	        else list.add(res);
    	         
    	    }
    	return list;
	}
}
```
# [Longest palindromic substring](https://leetcode.com/problems/longest-palindromic-substring/)
TC:O(n^2) for using two loops, space c: O(1)

```java
class Solution {
    public String longestPalindrome(String s) {
        //the lcs approach won't work here
        //we will have to use traditional approach of two loops for finding longest
        //palindromic subsequence
        int start=0;
        int length = 1;
        int n = s.length();
        if(n<2) return s;
        for(int i =0;i<n;i++){
            int low = i-1;
            int high = i+1;
            //below will check till how long the index values of high are equal to current index
            //which will be palindrome obviously
            while(high < n && s.charAt(high) == s.charAt(i)){
                high++;
            }
            //same logic as above for low index
            while(low>=0 && s.charAt(low) == s.charAt(i)){
                low--;
            }
            //finally low and high together
            while(low >=0 && high<n && s.charAt(low)== s.charAt(high)){
                low--;
                high++;
            }
            int len  = high-low -1;
            if(len> length){
                length = len;
                start = low+1;
            }
            
        }
        //if we were to return length of the logest palindromic substring we could have returned 'length';
        return s.substring(start,start+length);
    }
}

```
# [Compare verisons](https://leetcode.com/problems/compare-version-numbers/)

`TC: O(n) ,where n is the length of the longest version string`

```java
class Solution {
    public int compareVersion(String version1, String version2) {
        version1 = version1+".0";
        version2 = version2+".0";
        String[] a = version1.split("\\.");
        String[] b = version2.split("\\.");
        
        int exL = Math.abs(a.length - b.length);
        
        if(a.length > b.length){
            char c = '0';
            while(exL-->0){
                version2 = version2+"."+c;
            }
        }
        else {
            char c = '0';
            while(exL-->0){
                version1 = version1+"."+c;
            }
        }
    
        
        /// now two strings are equal
        a= version1.split("\\.");
        b = version2.split("\\.");
        System.out.println(a.length + " "+ b.length);
        for(int i =0;i<a.length;i++){
            int p = Integer.parseInt(a[i]);
            int q = Integer.parseInt(b[i]);
            if(p > q) return 1;
            if(p < q) return -1;
        }
        return 0;
    }
}
``` 

[Palindrome partitioning](https://leetcode.com/problems/palindrome-partitioning/submissions/)
`TC: exponential : O(2^n)`

```java
class Solution {
	List<List<String>> getPartitions(String s) {
	    // add logic here 
		List<List<String>> list = new ArrayList<>();
		f(s,0,new ArrayList<>(),list);
		return list;
	}
	public void f(String s, int index, List<String> l,List<List<String>> list){
		if(index>=s.length()){
			list.add(new ArrayList<>(l));
			return;
		}
		for(int i = index;i<s.length();i++){
			if(!isP(s,index,i)) continue;
			l.add(s.substring(index,i+1));
			f(s,i+1,l,list);
			l.remove(l.size()-1);
		}
	}
	public boolean isP(String s, int i,int e){
		while(i<=e){
			if(s.charAt(i++)!=s.charAt(e--)) return false;
		}
		return true;
	}
}
```
