# Minimum Spanning Tree (Kruskal's algorithm) Using Disjoint set

Time complexity is `O(mlogm) +  (m*O(4alpha)` ~ constant time = `O(1)` for disjoint set )
Hence,
Efficient time complexity is : `O(mlogm)` where `m` is the no of edges in the graph `logm` is the time complexity to sort the List of `m` edges. 

[For more information go through] (https://www.youtube.com/watch?v=1KRmCzBl_mQ&list=PLgUwDviBIf0rGEWe64KWas0Nryn7SCRWw&index=23)
```java
class Main{

 int findParent(int node,int parent[]){
        if(node == parent[node]) return node; // ie if the parent of 'node' is 'node' itself then just return the node.
        //return findParent(parent[node]);// else recursively call findParent to get the parent of this union.
        // in order to path compress (making sure that every node is connected to its parent in the given union)
        return parent[node]  = findParent(parent[node]);
        //example : 7->6->4->3 , here 3 is the parent of this union, so in order to get the parent of 7 which is 3 we can path compress it. like 7->3,6->3,4->3 etc.
    }
    void union(int u, int v,int parent[], int rank[]){
        u = findParent(u);
        v = findParent(v);
        if(rank[u] < rank[v]) parent[u] =v;
        else if (rank[u] > rank[v]) parent[v] = u;
        else parent[u] =v; // or parent[v] = u;
        // here u and v had the same rank
        //and now v became parent of u hence its rank will increase
        rank[v]++;
    }

    public static void main(String args[]) throws Exception{
        int n =5;
        ArrayList<Node> adj = new ArrayList<>();
        adj.add(new Node(0,1,2));
        adj.add(new Node(0,3,6));
        adj.add(new Node(1, 3, 8));
		adj.add(new Node(1, 2, 3));
		adj.add(new Node(1, 4, 5));
		adj.add(new Node(2, 4, 7));
        Main obj = new Main();
        obj.kruskalAlgo(adj,n);
    }
    public void kruskalAlgo(ArrayList<Node> adj, int n){
        Collections.sort(adj, new Comparator<Node> {
            public int compare(Node a, Node b){
                return a.getW()-b.getW();// sorting in ascending order of the weight;
            }
        });

        int parent[]  = new int[n];
        int rank[] = new int[n];
        //makeSet();
        for(int i =0;i<n;i++){
            parent[i] =i;
            rank[i] = 0;
        }
        int costOfMst = 0;
        ArrayList<Node> mst = new ArrayList<>();
        for(Node it : adj){
            // taking the node that is not taken already to avoid cycle.
            if(findParent(it.getU(),parent)!=findParent(it.getV(),parent)){
                costOfMst+=it.getW(); // add the cost in the costOfMst to keep track of growing cost to cover all the nodes
                mst.add(it);
                union(it.getU(),it.getV(),parent,rank); // make the node part of the union 
            }
        }
        System.out.println(costOfMst);
        for(Node it: mst){
            System.out.println(it.getU()+" - "+ it.getV());
        }
    }
    class Node {
        int u;
        int v;
        int weight;
        public Node(int u,int v, int w){
            this.u = u;
            this.v = v;
            this.weight = w;
        }
        public int getU(){
            return this.u;
        }
        public int getV(){
            return this.v;
        }
        public int getW(){
            return this.weight;
        }
    }
}
```