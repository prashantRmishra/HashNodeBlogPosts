# Minimum Spanning Tree (Prims Algorithm)

Given a weighted, undirected and connected graph of V vertices and E edges. The task is to find the sum of weights of the edges of the Minimum Spanning Tree.
**Example 1:**
 ![image](https://cdn.hashnode.com/res/hashnode/image/upload/v1672151605202/YR3zfzG3g.png)
![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1672151606860/ETJBBe8hb.png)
 
The Spanning Tree resulting in a weight
of 4 is shown above. 

```java
class Solution
{
    //Function to find sum of weights of edges of the Minimum Spanning Tree.
    // its similar to dijkstra's single source shortest path algorithm
    static int spanningTree(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj) 
    {
        // Add your code here
        boolean mstSet[] = new boolean[adj.size()];// this is nothing but visited nodes , that have become part of already chosen
        int key[] = new int[adj.size()];
        Arrays.fill(key,1000000000);// writing 1e9 means the nodes are yet to be discovered
        key[0] =0; // node 0 
        PriorityQueue<Pair> q = new PriorityQueue<>((a,b)->a.getValue()-b.getValue());
        //let the starting node be 0
        q.add(new Pair(key[0],0)); //distance of 0 from 0 is 0
        while(!q.isEmpty()){
           
            Pair p = q.remove();
            mstSet[p.getKey()] = true;
             //System.out.println("node is "+p.getKey() + " d from node 0 is "+ p.getValue());
            for(List<Integer> l : adj.get(p.getKey())){
                // below if statement will mean that this adjacent node of node p.getKey()  has not been taken
                if(mstSet[l.get(0)]== false && key[l.get(0)] > l.get(1)){
                    key[l.get(0)] = l.get(1);
                    q.add(new Pair(l.get(0),l.get(1)));
                }
            }
        }
        //for(int i : mstSet) System.out.print(i+" ");
        return Arrays.stream(key).reduce(0,(a,b)->a+b);
    }
}

```