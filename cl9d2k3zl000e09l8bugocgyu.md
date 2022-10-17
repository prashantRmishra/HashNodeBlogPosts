# Brides in the graph

```java
/*
Bridges in the graph.
Those edges in the graph whose removal will result 
in 2 or more components in the graph are called as bridges in the graph.
Consider the below example:
1-------2
|       |
|       |
4-------3
|
|
5-------6
		/\
	   /  \
	  7    9
	  \    /
	   \  /
	     8
		 |
		 |
		 10---11
		  |   /
		  |  /
		   12
		   
edge between 4 and 5 is called bridge, 
edge between 5 and 6 is bridge
edge between 8 and 10 is bride.

We will be using two things in the algorithm (We will be using depth first search algorithm)
Time of Insertion of the node
Lowest time of Insertion of the node 
Time complexity is : O(n +e)
Space complexity is : O(2n)~ O(n)
*/   
class Main{
	public static void main(String args[]){
		int n = 5;
        ArrayList<ArrayList<Integer> > adj = new ArrayList<ArrayList<Integer> >();
		
		for (int i = 0; i < n; i++) 
			adj.add(new ArrayList<Integer>());
			
		adj.get(0).add(1);
		adj.get(1).add(0);

		adj.get(0).add(2);
		adj.get(2).add(0);

		adj.get(1).add(2);
		adj.get(2).add(1);

		adj.get(1).add(3);
		adj.get(3).add(1);

		adj.get(3).add(4);
		adj.get(4).add(3);
			
		Main obj = new Main(); 
		obj.printBridges(adj, n); 
	}
	public void printBrides(ArrayList<ArrayList<Integer>> adjList, int n){
		int visited[] = new int[n];
		int timeOfInsertion[] = new int[n];
		int lowestTimeOfInstion[] = new int[n];
		int timer =0;
		for(int i =0;i<n;i++){
			if(visited[i] ==0){
				dfs(i,-1,visited,timeOfInsertion,lowestTimeOfInstion,adjList,timer);
			}
		}
	}
	public void dfs(int node, int parent, int visted[], int timeOfInsertion[], int lowestTimeOfInstion[], int timer){
		visited[node] = 1;
		timeOfInsertion[node] = lowestTimeOfInstion[node] = timer++;
		for(Integer adjNode : adjList.get(node)){
			if(adjNode == parent) continue; // if adjacent node is parent node just continue
			if(visited[adjNode] == 0){
				dfs(adjNode, node,visited,timeOfInsertion,lowestTimeOfInstion,adjList,timer);
				lowestTimeOfInstion[node] = Integer.min(lowestTimeOfInstion[node],lowestTimeOfInstion[adjNode]);
				// the case that check if the current edge is could be bridge or not
				if(lowestTimeOfInstion[adjNode] > timeOfInsertion[node]){
					System.out.println(adjNode + " " + node);
				}
			}
			else lowestTimeOfInstion[node] = Integer.min(lowestTimeOfInstion[node],timeOfInsertion[adjNode]);
		}
	}
```

[For more detailed explanation refer](https://www.youtube.com/watch?v=2rjZH0-2lhk&list=PLgUwDviBIf0rGEWe64KWas0Nryn7SCRWw&index=24)

**[Articulation point in graph](https://www.youtube.com/watch?v=3t3JHswP7mw&list=PLgUwDviBIf0rGEWe64KWas0Nryn7SCRWw&index=25)**
	