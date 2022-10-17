# Binary Trees

[Types of trees](https://cs.stackexchange.com/questions/32397/is-there-a-difference-between-perfect-full-and-complete-tree)

**Problems**:

# [Left view of the binary tree]()

TC: O(n) , as we will be traversing n nodes of the tree

```java
class Tree{
	int currentMax = Integer.MIN_VALUE;
	public ArrayList<Integer> leftView(Node node){
	    ArrayList<Integer> list = new ArrayList<>();
		traverseForLeftView(node,0,list);
		return list;
	}
	public void traverseForLeftView(Node node,int level,ArrayList<Integer> list){
		if(node==null) return;
		if(level > currentMax){
			list.add(node.data);
			currentMax = level;
		}
		traverseForLeftView(node.left,level+1,list);
		traverseForLeftView(node.right,level+1,list);
	}
}
```
# [Bottom view of binary tree](https://practice.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1)

TC : O(NlogN) + O(N) ~ O(NlogN), since we are using TreeMap it will take nlogn time for sorting entries based on keys

```java
class Solution
{
    //Function to return a list containing the bottom view of the given tree.
    public ArrayList <Integer> bottomView(Node root)
    {
        //we will use map and Queue for this
        ArrayList<Integer> list = new ArrayList<>();
        Map<Integer,Integer> map = new TreeMap<>();
        Queue<Pair> q = new LinkedList<>();
        if(root==null) return null;
        q.add(new Pair(root,0)); //root is at level 0
        while(!q.isEmpty()){
            Pair p = q.remove();
            Node n = p.getNode();
            int level = p.getLevel();
            map.put(level,n.data);
            if(n.left!=null){
                q.add(new Pair(n.left,level-1));
            }
            if(n.right!=null){
                q.add(new Pair(n.right,level+1));
            }
        }
        for(Map.Entry<Integer,Integer> e : map.entrySet()) list.add(e.getValue());
        return list;
    }
}
class Pair{
    Node node;
    int level;
    public Pair(Node n, int l){
        this.node = n;
        this.level = l;
    }
    public Node getNode(){
         return this.node;
    }
    public int getLevel(){
         return this.level;
    }
}
```

# [Top view of binary tree](https://practice.geeksforgeeks.org/problems/top-view-of-binary-tree/1)

TC : o(nlogn) for using TreeMap<>();

```java
class Solution
{
    //Function to return a list of nodes visible from the top view 
    //from left to right in Binary Tree.
    static ArrayList<Integer> topView(Node root)
    {
        // we can solve it with the same depth first search approach we used for bottom view of the tree
        ArrayList<Integer> list = new ArrayList<>();
        Map<Integer,Integer> map = new TreeMap<>();
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(root,0));
        while(!q.isEmpty()){
            Pair p = q.remove();
            Node n = p.getNode();
            int level = p.getLevel();
            if(!map.containsKey(level))
                map.put(level,n.data);
            if(n.left!=null) q.add(new Pair(n.left,level-1));
            if(n.right!=null) q.add(new Pair(n.right,level +1));
            
        }
        for(Map.Entry<Integer,Integer> e : map.entrySet()) list.add(e.getValue());
        return list;
        
    }
}
class Pair{
    Node node;
    int level;
    public Pair(Node n, int l){
        this.node = n;
        this.level = l;
    }
    public Node getNode(){
        return this.node;
    }
    public int getLevel(){
        return this.level;
    }
}
```
# [maximum width in a binary tree](https://www.codingninjas.com/codestudio/problems/maximum-width-in-binary-tree_763671?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=0)

TC: O(n), as we are traversing n nodes of the tree

```java
import java.util.*;
public class Solution {
	public static int getMaxWidth(TreeNode root) {
        if(root ==null) return 0;
		// Write your code here.
        //this we can solve using depth first search algorithm and keep track of 
        //max no. of nodes in a given level
        Queue<TreeNode> q = new LinkedList<>();
        int max = 0;
        q.add(root);
        while(!q.isEmpty()){
            int n = q.size();
            max  = Integer.max(max,n);
            for(int i =0;i<n;i++){
                TreeNode node = q.remove(); 
                if(node.left!=null) q.add(node.left);
                if(node.right!=null) q.add(node.right);
            }
        }
        return max;  
	}
}
```

# [Maximum width of a binary tree when null nodes are also considered between leftMost node and rightMost node at each level](https://leetcode.com/problems/maximum-width-of-binary-tree/submissions/)

TC: O(N) where n is the no. of nodes

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        //we will use strivers approach for solving this problem
       
        int maxWidth = 0;
        Queue<Pair<TreeNode,Integer>> q = new LinkedList<>();
        if(root== null) return 0;
        q.add(new Pair(root,0));
        while(!q.isEmpty()){
            int size = q.size();
            int first = 0;
            int last  =0;
            int minIndex  = q.peek().getValue();
            for(int i =0;i<size;i++){
                Pair<TreeNode,Integer> p = q.remove();
                TreeNode n = p.getKey();
                int index = p.getValue()-minIndex;
                if(i ==0) first = index;
                if(i ==size-1) last = index;
                if(n.left!=null) q.add(new Pair(n.left,2*index +1));
                if(n.right!=null) q.add(new Pair(n.right,2*index +2));
            }
            maxWidth = Integer.max(maxWidth,last-first +1);
            
        }
        return maxWidth;
    }
}

```
# [Mirror a binary tree](https://practice.geeksforgeeks.org/problems/mirror-tree/1)
`TC: O(n), where n is no. of nodes in the tree`

```java
class Solution {
    // Function to convert a binary tree into its mirror tree.
    void mirror(Node node) {
        dfs(node);
    }
    public Node dfs(Node node){
        if(node ==null) return node;
        Node left = dfs(node.left);
        Node right = dfs(node.right);
        Node temp = node.left;
        node.left = right;
        node.right = temp;
        return node;
    }
}
```