---
tags:
  - DFS
  - 暴搜
---
搜索算法中，dfs也常用于网格搜索，搜索过程考虑4或8个方向，同时注意不要错过网格边界
<mark style="background: #FF5582A6;">本质：是对数据结构在图的搜索，可能是有向图也可能是无向图</mark>

对可能出现的结果，构造状态，用DFS或BFS搜索遍历，与枚举区别仅是方法不同
DFS：使用递归操作遍历，对<mark style="background: #FF5582A6;">搜索树，图遍历</mark>，（逻辑上）树与图自己根据题目构造，分析题目的过程就是树构建的过程

[lg p1706](https://www.luogu.com.cn/problem/P1706)
# P1706 全排列问题

## 题目描述

按照字典序输出自然数 $1$ 到 $n$ 所有不重复的排列，即 $n$ 的全排列，要求所产生的任一数字序列中不允许出现重复的数字。

## 输入格式

一个整数 $n$。

## 输出格式

由 $1 \sim n$ 组成的所有不重复的数字序列，每行一个序列。

每个数字保留 $5$ 个场宽。

## 输入输出样例 #1

### 输入 #1

```
3
```

### 输出 #1

```
    1    2    3
    1    3    2
    2    1    3
    2    3    1
    3    1    2
    3    2    1

```

## 说明/提示

$1 \leq n \leq 9$。

>[!summary]+ 思路
>全排列：第一个位置从小大放数据，第二个位置要去掉第一个位置的数······以此类推第n个数据要减掉n - 1个数据，前面数据放的时候，标记，
>每个位置操作相同——》大问题化小问题——》递归
>可以构建树（逻辑上的），模拟逻辑上DFS遍历过此树的操作
>字典序：a——z，1——9
>此题递归：dfs+模拟的树，dfs逻辑上遍历模拟的树，模拟树——》各位不同的数

![[图片/a4aaf9c0965b245f8a551d52e85b6d3c.jpg]]

```java
import java.util.Scanner;
import java.util.*;
public class Main {
    static int n;
    static int visited[];
    static int ans[];
    //向左找第一个比自身大的数。右--->左 底到顶递减栈
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        visited = new int[n + 1];
        ans = new int[n + 1];
        dfs(1);
    }
    public static void dfs(int d ){
        if(d > n){
            for(int j = 1; j <= n; j++){
                System.out.printf("%5d" , ans[j]);
            }
            System.out.println();
            return;
        }
        for(int i = 1 ; i <= n ; i++){//模拟了树的一层情况，认为有一个根节点
            if (visited[i] == 0) {
                visited[i] = 1;
                ans[d] = i;
                dfs(d + 1);
                visited[i] = 0;//完成此路径的遍历操作，消除此路的操作，回溯
            }//一个深度遍历完成返回换另一条路
        }
        return;
    }
}
```

>[!summary]+回溯
>递归中需要撤销本层的操作，普通递归结束后直接返回

>[!summary]+ 重点
>根据题目自己构造树或图，让dfs遍历，并不是实现图或树，而是dfs过程中直接模拟图或树,对岛屿进行四个方向DFS，相当于根节点向子树for遍历

[lk 200](https://leetcode.cn/problems/number-of-islands/)
给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例 1：**

**输入：**grid = [
  ['1','1','1','1','0'],
  ['1','1','0','1','0'],
  ['1','1','0','0','0'],
  ['0','0','0','0','0']
]
**输出：**1

**示例 2：**

**输入：**grid = [
  ['1','1','0','0','0'],
  ['1','1','0','0','0'],
  ['0','0','1','0','0'],
  ['0','0','0','1','1']
]
**输出：**3

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` 的值为 `'0'` 或 `'1'`

>[!summary]+ 思路
>本质是几个联通的图，遍历图，1——》节点
>找连通的图——》遍历所有的点，一个未遍历过的点，就代表一个未连通的图，加1并标记，以此节点对四周遍历，dfs，bfs都可以，遍历的节点标记

```java
import java.util.Scanner;  
import java.util.*;  
public class lk {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
  
    }  
}  
class Solution {  
    public void dfs(char[][] grid, int[][] v, int x, int y) {  
        int length = grid.length;  
        int height = grid[0].length;  
        v[x][y] = 1;  
        if (x + 1 < length && grid[x + 1][y] == '1' && v[x + 1][y] != 1) {  
            dfs(grid, v, x + 1, y);  
        }  
        if (y + 1 < height && grid[x][y + 1] == '1' && v[x][y + 1] != 1) {  
            dfs(grid, v, x, y + 1);  
        }  
        if (x - 1 >= 0 && grid[x - 1][y] == '1' && v[x - 1][y] != 1) {  
            dfs(grid, v, x - 1, y);  
        }  
        if (y - 1 >= 0 && grid[x][y - 1] == '1' && v[x][y - 1] != 1) {  
            dfs(grid, v, x, y - 1);  
        }  
    }  
  
    public int numIslands(char[][] grid) {  
        int sum = 0;  
        int length = grid.length;  
        int height = grid[0].length;  
        int visited[][] = new int[length][height];  
        for (int i = 0; i < length; i++) {  
            for (int j = 0; j < height; j++) {  
                if (grid[i][j] == '1' && visited[i][j] == 0) {  
                    sum += 1;  
                    dfs(grid, visited, i, j);  
                }  
            }  
        }  
        return sum;  
    }  
  
}
```

>[!summary]+ 改进
>不加visited数组，直接将char数组的'1'改为’0’

>[!summary]+ 解题
>网格类型的题将格子视为节点，遍历所有格子，类似数组存储图的形式，没必要构造图，没思路·可以按题目画出图

>[!summary]+ 总结
>列举状态空间——》树，图
>DFS：大问题——》小问题
>树——》明显分层的问题递归解决 ，全排列——》第一个空，第一层，for模拟一层

