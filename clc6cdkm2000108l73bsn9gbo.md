# Graph Questions

\[Clone a given graph\] (https://leetcode.com/problems/clone-graph/) `TC: O(N) for n nodes in the graph` **This is nothing but Depth First Search Algorithm** \`\`\`java /\* // Definition for a Node. class Node { public int val; public List neighbors; public Node() { val = 0; neighbors = new ArrayList(); } public Node(int \_val) { val = \_val; neighbors = new ArrayList(); } public Node(int \_val, ArrayList \_neighbors) { val = \_val; neighbors = \_neighbors; } } \*/

class Solution { //we will use dfs for cloning the graph, we will traverse //the graph in depth first search manner and create a complete clone of the given graph.

public Node cloneGraph(Node node) { if(node == null) return node;

Node clone = new Node(node.val); Node\[\] visited = new Node\[101\]; // 101 is the max size of nodes that can be present in the test case. dfs(clone,node,visited); return clone;

} public void dfs(Node clone, Node actual, Node\[\] visited){ visited\[clone.val\] = clone; for(Node next : actual.neighbors){ if(visited\[next.val\] ==null) { Node newNode = new Node(next.val); clone.neighbors.add(newNode); dfs(newNode,next,visited); } else { clone.neighbors.add(visited\[next.val\]); } } } } \`\`\`

[Couse Schedule](https://leetcode.com/problems/course-schedule/) **Detecting cycle in a directed graph** `TC: O(N), for breadth first traversal of N nodes in the given graph SC:O(n) for indegree, O(n) for graph(ArrayList<>()), and O(n) for Queue So, in total 3O(N)` \`\`\`java // this is similar to finding cycle in a directed graph //we have used kahn's topological sorting algo to check if the cycle exist in the graph or not.

class Solution { public boolean canFinish(int numCourses, int\[\]\[\] prerequisites) { ArrayList&lt;ArrayList&gt; graph = new ArrayList&lt;&gt;(); for(int i =0;i&lt;numCourses;i++){ graph.add(new ArrayList&lt;&gt;()); } //getting indegrees of all the nodes int indegree\[\] = new int\[numCourses\]; for(int i =0;i&lt;prerequisites.length;i++){ indegree\[prerequisites\[i\]\[0\]\]++; }

Queue q = new LinkedList&lt;&gt;(); for(int i =0;i&lt;numCourses;i++){ if(indegree\[i\]==0) q.add(i); } // create adjacency matrix in ArrayList for(int i=0;i&lt;prerequisites.length;i++){ ArrayList l2 = graph.get(prerequisites\[i\]\[1\]); l2.add(prerequisites\[i\]\[0\]); graph.set(prerequisites\[i\]\[1\],l2); }

int count =0; while(!q.isEmpty()){ int node = q.remove(); count++; for(Integer it : graph.get(node)){ indegree\[it\]--; if(indegree\[it\] ==0) q.add(it); } } return count ==numCourses; }

} \`\`\`

[Topological sorting of the graph using depth first search](https://www.codingninjas.com/codestudio/problems/982938?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)

`TC: O(N) , for visiting N nodes` \`\`\`java import java.util.\*; public class Solution { public static ArrayList topologicalSort(ArrayList&lt;ArrayList&gt; edges, int v, int e) { // we will use depth first search for getting the topo sort of the //given graph. ArrayList list = new ArrayList&lt;&gt;(); // lets create an adjacency list of the given edges ArrayList&lt;ArrayList&gt; graph = new ArrayList&lt;&gt;(); for(int i =0;i&lt;v;i++){ graph.add(new ArrayList&lt;&gt;()); } for(ArrayList l: edges){ ArrayList temp = graph.get(l.get(0)); temp.add(l.get(1)); graph.set(l.get(0),temp); } /// lets call depth first search to get topo sort ///we will use stack data structure to get the nodes based Stack s = new Stack&lt;&gt;(); int visited\[\] = new int\[v\];

for(int i=0;i&lt;v;i++){ if(visited\[i\] ==0) { getTopoSort(i,s,graph,visited); } } //now we will pop the stack to get the nodes in order of topo sort while(!s.isEmpty()){ list.add(s.pop()); } return list; } public static void getTopoSort(int n,Stack s, ArrayList&lt;ArrayList&gt; graph,int\[\] visited){ visited\[n\] = 1; for(Integer i: graph.get(n)){ if(visited\[i\]==0){getTopoSort(i,s,graph,visited);} } s.push(n); } }

````java
[Find number of Islands](https://www.codingninjas.com/codestudio/problems/630512?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTab=1)

`Tc:n^2 * 8^n, since we will run through all the elements of the matrix and for every functions call there will be 8 choices to move to.`

```java
public class Solution 
{
    public static int getTotalIslands(int[][] mat) 
	{
        int count =0;
        for(int i =0;i<mat.length;i++){
            for(int j=0;j<mat[0].length;j++){
                if(mat[i][j]!=2 && mat[i][j] ==1){
                    traverseLand(mat,i,j);
                    count++;
                }
            }
        }
        return count;
    }
    public static void traverseLand(int [][] mat, int i,int j){
        if(i<0 || j<0 || i>=mat.length || j>=mat[0].length || mat[i][j] ==0 ||
           mat[i][j] ==2) return;
        mat[i][j] =2;
        //left, right,top and bottom movements
        traverseLand(mat,i,j-1);
        traverseLand(mat,i,j+1);
        traverseLand(mat,i-1,j);
        traverseLand(mat,i+1,j);
        //diagonal movements
        traverseLand(mat,i-1,j-1);
        traverseLand(mat,i+1,j+1);
        traverseLand(mat,i-1,j+1);
        traverseLand(mat,i+1,j-1);
    }
}
````

[Check if the given graph is bipartite or not](https://practice.geeksforgeeks.org/problems/bipartite-graph/1)

`TC : O(n), as we will be visiting at max n nodes` *Depth first search and breadth first search both approaches are given with their respective functions*

```java
class Solution
{
    public boolean isBipartite(int V, ArrayList<ArrayList<Integer>>adj)
    {
        //we will use breadh first search for finding out if a graph is bipartite or
        //not
        //a graph is called as bipartite graph if two adjacent nodes can be colored with
        //different colors
        int color[]= new int[V];
        Arrays.fill(color,-1);//intialize all the color of all the nodes as 0;
        for(int i =0;i<V;i++){
            if(color[i]==-1){
                //we are taking 0 as first color of the node i
                color[i] = 0;
                if(!possibleDFS(color,i,adj)) return false;
            }
        }
        return true;
    }
    public boolean possibleBFS(int [] color, int i, ArrayList<ArrayList<Integer>> adj){
        Queue<Integer> q = new LinkedList<>();
        q.add(i);
        while(!q.isEmpty()){
            int node  = q.remove();
            for(Integer it : adj.get(node)){
                if(color[it]==-1){
                     q.add(it);
                     color[it]  = 1-color[node];
                }
                else if(color[it]==color[node]) return false;
            }
       
        }
     return true;
    }
    public boolean possibleDFS(int [] color, int i, ArrayList<ArrayList<Integer>>adj){
    
        for(Integer it : adj.get(i)){
            if(color[it] ==-1) {
                color[it] = 1-color[i];
                if(!possibleDFS(color,it,adj)) return false;
            }
            else if(color[it]==color[i]) return false;
        }
        return true;
    }
}
```