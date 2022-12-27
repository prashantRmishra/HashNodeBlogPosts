# Strongly connected components in the graph

**Problem**:
You are given an unweighted directed graph of 'V' vertices and 'E' edges. Your task is to print strongly connected components (SCCs) present in the graph.

**Input format**:
You are given 'n' which is no. of vertices in the graph(0-based indexed)
'edges' 2d array of size m*2, where m is no. of edges and 2 is two vertices between which the edge exists. That is `edges[i][0]---->edges[i][1]` there is an edge between node `0` and `1`.

`TC: topo sort = O(N+E) + 
transpose of the graph = O(N+E) +
dfs on topo sort with transpose graph = O(N+E)
`

```java
import java.util.*;
public class Solution {

    public static List<List<Integer>> stronglyConnectedComponents(int n, int[][] edges) {
        List<List<Integer>> list = new ArrayList<>();
        // Write your code here
        // we identify which are the strogly conected component,
        // strongly connected components are the components where all the
        //are reachable by every other node in the component.
        //steps to solve this using kosaraju's SCC
        //1. find toposort of the graph.
        //2. transpose the graph(change direction of the edges)
        //3. do dfs on the toposort with the transpose graph.
        //creating adjacency list
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        ArrayList<ArrayList<Integer>> tadj  = new ArrayList<>();
        for(int i =0;i<n;i++) {
            adj.add(new ArrayList<>());
            tadj.add(new ArrayList<>());
        }
        for(int ed [] : edges) {
            //creating adjacency graph
            ArrayList<Integer> temp = adj.get(ed[0]);
            temp.add(ed[1]);
            adj.set(ed[0],temp);
            //creating transpose adjacency graph
            ArrayList<Integer> temp2 = tadj.get(ed[1]);
            temp2.add(ed[0]);
            tadj.set(ed[1],temp2);
        }
         
        int visited[] = new int[n];
        Stack<Integer> stack = new Stack<>();
        for(int i =0;i<n;i++){
            if(visited[i]==0){
                dfs(i,visited,stack,adj);
            }
        }
        //now stack has topo sorted nodes in it.
        Arrays.fill(visited,0);
       while(!stack.isEmpty()){
           int node   = stack.pop();
           if(visited[node] ==0){
               
               ArrayList<Integer> connComp = new ArrayList<>();
               dfsOnTransposeGraph(node,visited,tadj,connComp);
               list.add(connComp);
           } 
       }
        return list;
    }
     public static void dfsOnTransposeGraph(int node, int[] visited, 
                          ArrayList<ArrayList<Integer>> adj,ArrayList<Integer> connComp){
        visited[node] = 1;
        connComp.add(node);
        for(Integer it : adj.get(node)){
            if(visited[it]==0) {
                dfsOnTransposeGraph(it,visited,adj,connComp);
            }
        }
    }
    public static void dfs(int node, int[] visited, Stack<Integer> stack, 
                          ArrayList<ArrayList<Integer>> adj){
        visited[node] = 1;
        for(Integer it : adj.get(node)){
            if(visited[it]==0) dfs(it,visited,stack,adj);
        }
        stack.push(node);
    }
}
```