## 200

数组的dfs和二叉树的dfs十分类似。  第一步都是判断边界条件，然后操作递归元素，最后再递归其他元素。

都是从原问题拆成小问题。本题原问题：数岛屿数量；子问题：判断岛屿节点四周是不是岛屿节点。

```java
class Solution {
    public int numIslands(char[][] grid) {
        int res=0;
        int m=grid.length;
        int n=grid[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='1'){
                    dfs(grid,i,j);
                    res++;
                }
            }
        }
        return res;
        
    }

    public void dfs(char[][] grid,int i,int j) {
        if(i<0||j<0||i>=grid.length||j>=grid[0].length||  grid[i][j]=='0'){
            return;
        }
        if(grid[i][j]=='1'){
            grid[i][j]='0';
        }
        dfs(grid,i+1,j);
        dfs(grid,i,j+1);
        dfs(grid,i-1,j);
        dfs(grid,i,j-1);
        
    }

}
```




## 994

数组的bfs和二叉树一样差不多，不过需要创一个辅助数组来方便bfs左右上下节点。注意队列里只存腐烂的橘子，所以要，然后每遍历一层且fresh > 0的情况下 ans++

```java
class Solution {
   
    public int orangesRotting(int[][] grid) {
        int ans = 0;
        int fresh = 0;
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        Queue<int[]> queue = new LinkedList<>();
        for(int i = 0;i < grid.length; i++){
            for(int j = 0; j < grid[i].length; j++){
                if(grid[i][j] == 2){
                    queue.offer(new int[]{i,j});
                }
                if(grid[i][j] == 1) fresh++;
            }
        }

        while(!queue.isEmpty() && fresh > 0){
            int n = queue.size();
            ans++;
            for(int i = 0; i < n; i++){
                int[] t = queue.poll();
                for(int[] d : dirs){
                    int k = t[0]+d[0];
                    int j = t[1]+d[1];
                    if(k < 0 || j < 0 || k >= grid.length || j >= grid[k].length || grid[k][j]!= 1) continue;
                    fresh--;
                    grid[k][j] = 2;
                    queue.offer(new int[]{k,j});
                }
            }
        }
        return fresh > 0 ? -1 :ans;
    }

    
}
```



## 46

这题是要用到回溯的dfs。用一个数组判断nums里的数有没有被用过，然后dfs遍历，最后回溯就完了。

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        int n = nums.length;
        List<Integer> path = Arrays.asList(new Integer[n]);
        List<List<Integer>> res = new ArrayList<>();
        boolean[] onPath = new boolean[n];
        dfs(0, nums, res, path, onPath);
        return res;
    }

    public void dfs(int i, int[] s, List<List<Integer>> res, List<Integer> path, boolean[] onPath){
        if(i == s.length){
            res.add(new ArrayList<Integer>(path));
            return;
        }

        for(int j = 0; j < s.length; j++){
            if(!onPath[j]){
                path.set(i,s[j]);
                onPath[j] = true;
                dfs(i+1, s, res, path, onPath);
                onPath[j] = false;
            }
        }
    }
}
```

注意：   List<Integer> path = Arrays.asList(new Integer[n];是直接给List这么多size，有参构造只是定容量，size= 0。
