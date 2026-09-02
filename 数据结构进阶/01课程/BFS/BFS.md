---
tags:
  - BFS
---
搜索：也就是对<mark style="background: #FF5582A6;">状态空间</mark>进行枚举，通过穷尽所有的可能来找到<mark style="background: #FF5582A6;">最优解</mark>，或者<mark style="background: #FF5582A6;">统计</mark>合法解的<mark style="background: #FF5582A6;">个数</mark>。搜索有很多优化方式，如减小状态空间，更改搜索顺序，剪枝等。
BFS：按圈层搜索    <mark style="background: #FF5582A6;">找最优解！！！</mark>
在搜索算法中，BFS是用队列实现广度搜索的算法，可以寻找<mark style="background: #FF5582A6;">最短距离</mark>，借组<mark style="background: #FF5582A6;">队列</mark>实现

例如：从起点到一点用BFS搜索的算法遍历所用的时间就是这个点的最短时间/最短路径。
DFS不行，DFS是直接走到底，不确定是不是从长路径走过来的，BFS是一圈一圈向外扩散，第一层是邻接点，以此分层，<mark style="background: #FF5582A6;">最短距离：第一次遍历到一个节点的距离/时间——》最短</mark>
![[数据结构进阶/01课程/BFS/Drawing 2026-03-25 12.26.37.excalidraw|Drawing 2026-03-25 12.26.37.excalidraw]]
综上所述，可得：
1. 最优解：BFS
2. 可行解：根据题目分析，模拟，怎么遍历
	1. 深度：DFS
	2. 广度：BFS
<mark style="background: #FF5582A6;">本质：将树或图的操作全部融入到BFS中</mark>


[lk p1443](https://www.luogu.com.cn/problem/P1443)
# P1443 马的遍历

## 题目描述

有一个 $n \times m$ 的棋盘，在某个点 $(x, y)$ 上有一个马，要求你计算出马到达棋盘上任意一个点最少要走几步。

## 输入格式

输入只有一行四个整数，分别为 $n, m, x, y$。

## 输出格式

一个 $n \times m$ 的矩阵，代表马到达某个点最少要走几步（不能到达则输出 $-1$）。

## 输入输出样例 #1

### 输入 #1

```
3 3 1 1

```

### 输出 #1

```
0 3 2    
3 -1 1    
2 1 4    
```

## 说明/提示

### 数据规模与约定

对于全部的测试点，保证 $1 \leq x \leq n \leq 400$，$1 \leq y \leq m \leq 400$。

2022 年 8 月之后，本题去除了对输出保留场宽的要求。为了与之兼容，本题的输出以空格或者合理的场宽分割每个整数都将判作正确。

马走日，数组存放着点的位置,走动位置如下：
![[数据结构进阶/01课程/BFS/Drawing 2026-03-25 21.23.34.excalidraw|Drawing 2026-03-25 21.23.34.excalidraw]]
>[!summary]+ 思路
>起始点位置有八个方向移动，分别建立x,y改变位置的数组，方便操作，x，y的数组要对应
>这八个方向要注意边界判断，分别是八个状态——》图的节点，存入数组中——》BFS遍历
>防止重复遍历——》visited，存放状态——》queue，构建一个类型存放数值对


```java
import java.util.Scanner;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        int x = sc.nextInt();
        int y = sc.nextInt();
        xiangqi f = new xiangqi();
        f.ma(n , m , x , y);
    }
}
class Node{
    int x;
    int y;
    public Node(int x , int y){
        this.x = x;
        this.y = y;
    }
}
class xiangqi{
    int[][] ans = new int[405][405];
    int[][] visited = new int[405][405];
    int[] x_move = {1,1,-1,-1,2,2,-2,-2};
    int[] y_move = {2,-2,2,-2,1,-1,1,-1};
    public void ma(int n , int m ,int x ,int y ){
        for(int i = 0 ; i < 405 ; i++){
            for(int j = 0 ; j < 405 ; j++){
                ans[i][j] = -1;
            }
        }
        ans[x][y] = 0;
        visited[x][y] = 1;
        ArrayDeque<Node> q = new ArrayDeque<>();
        Node node = new Node(x , y);
        q.add(node);
        while(!q.isEmpty()){
            Node tmp = q.remove();

            for(int i = 0; i < 8;i++){
                int tmp_x = tmp.x + x_move[i];
                int tmp_y = tmp.y + y_move[i];
                if(tmp_x <= n && tmp_x > 0 && tmp_y > 0 && tmp_y <= m && visited[tmp_x][tmp_y] == 0){
                    Node tmp_node = new Node(tmp_x, tmp_y);
                    q.add(tmp_node);
                    visited[tmp_x][tmp_y] = 1;
                    ans[tmp_x][tmp_y] = ans[tmp.x][tmp.y] + 1;
                }
            }
        }
        for(int i = 1 ; i <= n ; i++){
            for(int j = 1; j <= m ; j++){
                System.out.print(ans[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```
>[!summary] + 知识点
>如果多次使用固定顺序的计算可将其总结为一个/几个数组，下标为对应关系！！！

