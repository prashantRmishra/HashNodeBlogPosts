# ("Graph") 797. All Paths From Source to Target

Given a **directed acyclic graph** (DAG) of n nodes labeled from 0 to n - 1, find all possible paths from node 0 to node n - 1 and return them in any order.
 
The graph is given as follows: graph[i] is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node graph[i][j]).

**Example**: 
![graph](https://cdn.hashnode.com/res/hashnode/image/upload/v1672151533172/m5XXo9VZL.jpeg)

```
Input: graph = [[1,2],[3],[3],[]]
Output: [[0,1,3],[0,2,3]]
Explanation: There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
```

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        // we have to find all the possible path from source to destination.
        // we can do that with depth first search.
        //we can keep track of visited nodes in a list in a depth first search manner.
        //if we reach the target node we will store list 'l', and we will backtrack to adjacent nodes of the previous node to check all the possible paths
        List<List<Integer>> list = new ArrayList<>();
        dfs(0,graph,new ArrayList<>(Arrays.asList(0)),list); //initially we are also adding 0 in the list l, as the starting node
        return list;
    }
    public void dfs(int node, int[][] graph,List<Integer> l, List<List<Integer>> list){
        if(node ==graph.length-1){
            list.add(new ArrayList<>(l));
            return;
        }
        // below loop will give array of all adjacent nodes of current node i.e 'node'
        for(int i : graph[node]){
            //take
            l.add(i);
            dfs(i,graph,l,list);
            //don't take
            l.remove(l.size()-1);
        }
    }
}
```