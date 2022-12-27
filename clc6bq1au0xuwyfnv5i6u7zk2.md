# Rotting oranges

[Problem](https://leetcode.com/problems/rotting-oranges/)

```java
//my solution
class Solution {
    public int orangesRotting(int[][] grid) {
        //n2 to find the first rotten tomato
        Queue<Indices> q  = new LinkedList<>();

        for(int i =0;i<grid.length;i++){
            for(int j =0;j<grid[0].length;j++){
                if(grid[i][j]!=2) continue;
                q.add(new Indices(i,j));
            }
        }
        //do breadth first search to set all the tomatos rotten
        int timer = 0;
        while(!q.isEmpty()){
            int size = q.size();
             boolean newRotting = false;
            while(size-->0){
                Indices in = q.remove();
                 //left
                 if(in.j-1>=0 && grid[in.i][in.j-1]==1){
                     grid[in.i][in.j-1] = 2; 
                     newRotting  = true;
                     q.add(new Indices(in.i,in.j-1));
                 }
                //right
                if(in.j+1<grid[0].length && grid[in.i][in.j+1] ==1){
                    grid[in.i][in.j+1]  =2;
                    newRotting = true;
                    q.add(new Indices(in.i,in.j+1));
                }
                //up
                if(in.i-1>=0 && grid[in.i-1][in.j] ==1){
                    grid[in.i-1][in.j] = 2;
                    newRotting = true;
                    q.add(new Indices(in.i-1,in.j));
                }
                //down
                if(in.i +1 < grid.length && grid[in.i+1][in.j]==1){
                    grid[in.i+1][in.j] = 2;
                    newRotting  =true;
                    q.add(new Indices(in.i+1,in.j));
                }
            }
             if(newRotting) timer++;
        }
        //n^2 to check if there are any tomatos left freash if yes return -1 else return minutes elapsed.
    for(int i =0;i<grid.length;i++){
                for(int j =0;j<grid[0].length;j++){
                    if(grid[i][j]==1) return -1;
                }
        }
         for(int i =0;i<grid.length;i++){
                for(int j =0;j<grid[0].length;j++){
                    System.out.print(grid[i][j]);
                }
                System.out.println();
        }
        return timer;
    } 
}
class Indices{
    int i;
    int j;
    public Indices(int i, int j) {
        this.i =i;
        this.j = j;
    }
}
```

```java


// second solution from one of the submission that I saw
//great solution
class Solution {
    public int orangesRotting(int[][] grid) {
        int m=grid.length,n=grid[0].length,i,j,k=0,fresh=0,fr;
        for(i=0;i<m;i++)
            for(j=0;j<n;j++)
                if(grid[i][j]==1) fresh++;
        while(fresh>0){
            fr=fresh;
            for(i=0;i<m;i++){
                for(j=0;j<n;j++){
                    if(grid[i][j]==2){
                        if(i+1<m && grid[i+1][j]==1) {grid[i+1][j]=3;fresh--;}
                        if(i-1>=0 && grid[i-1][j]==1) {grid[i-1][j]=3;fresh--;}
                        if(j-1>=0 && grid[i][j-1]==1) {grid[i][j-1]=3;fresh--;}
                        if(j+1<n && grid[i][j+1]==1) {grid[i][j+1]=3;fresh--;}
                    }
                }
            }
            for(i=0;i<m;i++)
                for(j=0;j<n;j++)
                    if(grid[i][j]==3) grid[i][j]=2;
            if(fr==fresh) return -1;
            k++;
        }
        return k;
    }
}
```