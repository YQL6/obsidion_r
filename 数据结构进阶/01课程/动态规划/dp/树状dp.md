---
tags:
  - 树状dp
---
树形dp是一种思想，无固定算法；其建立在<mark style="background: #FF5582A6;">树状结构</mark>的基础上。
思路为：<mark style="background: #FF5582A6;">树形结构</mark>存储数据，在<mark style="background: #FF5582A6;">树上</mark>来进行状态转移；

<mark style="background: #FF5582A6;">动态规划</mark>本来就是使用<mark style="background: #FF5582A6;">递归</mark>进行备忘录写，而<mark style="background: #FF5582A6;">树形结构就是使用递归</mark>算法来解决问题，两者天然合适；

树形dp中，将<mark style="background: #FF5582A6;">一颗子树/节点</mark>看为一个<mark style="background: #FF5582A6;">状态</mark>， <mark style="background: #FF5582A6;">状态转移</mark>就在父节点与<mark style="background: #FF5582A6;">子节点/兄弟节点/树与子树间</mark>

解题实现——》递归+备忘录——》<mark style="background: #FF5582A6;">dfs的剪枝优化</mark>；
搜索方式：
1. 叶子——》根：先从根向下遍历，在返回时，使用回溯算法来记录叶子传来的信息；
2. 根——》叶子：dfs进行预处理，在从新向下获取最后答案；

<mark style="background: #FF5582A6;">存储实现</mark>：从父子兄弟选一个 + 对子节点的遍历 + 可能不是二叉树——》特殊的图——》<mark style="background: #FF5582A6;">链式前向星</mark>

常见问题：
1. 选择节点类问题（最大独立集问题）：相邻节点不可同时存在，求最值；
2. 树上背包问题：背包 + 一个物品x只有选择了它的主件/根节点，才可以选择次物品；

解题步骤：
1. 确定节点代表的状态：状态一般为当前节点的子树的某种性质；
2. 确定状态转移方程：父子，兄弟；
3. 备忘录+dfs



[lg 1352](https://www.luogu.com.cn/problem/P1352)
# P1352 没有上司的舞会

## 题目描述

某大学有 $n$ 个职员，编号为 $1\ldots n$。

他们之间有从属关系，也就是说他们的关系就像一棵以校长为根的树，父结点就是子结点的直接上司。

现在有个周年庆宴会，宴会每邀请来一个职员都会增加一定的快乐指数 $r_i$，但是呢，如果某个职员的直接上司来参加舞会了，那么这个职员就无论如何也不肯来参加舞会了。

所以，请你编程计算，邀请哪些职员可以使快乐指数最大，求最大的快乐指数。

## 输入格式

输入的第一行是一个整数 $n$。

第 $2$ 到第 $(n + 1)$ 行，每行一个整数，第 $(i+1)$ 行的整数表示 $i$ 号职员的快乐指数 $r_i$。

第 $(n + 2)$ 到第 $2n$ 行，每行输入一对整数 $l, k$，代表 $k$ 是 $l$ 的直接上司。

## 输出格式

输出一行一个整数代表最大的快乐指数。

## 输入输出样例 #1

### 输入 #1

```
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5

```

### 输出 #1

```
5

```

## 说明/提示

#### 数据规模与约定

对于 $100\%$ 的数据，保证 $1\leq n \leq 6 \times 10^3$，$-128 \leq r_i\leq 127$，$1 \leq l, k \leq n$，且给出的关系一定是一棵树。

>[!summary]+ 思路
>对于每一个人来不来——》dp[i][0/1]表示第i个人0不来/1来时的以它为子树的整个子树的值
>对与一个根节点：树与其子树状态转移
>1. 根节点来：自己的值，两个子树不来的整个子树的值;y为子树下标，dp[x][1] += sum(dp[y][0]) 
>2. 根节点不来：子树的和，但是子树根节点可以来，也可以不来，所以取最大值；dp[x][0] + =sum(max(dp[y][0] , dp[y][1]));
>最后找到根节点，取最大的；
>初始状态：dp[x][1] = v[x];//刚开始都以为自己来
>dp[x][0] = 0;


```java
import java.util.Scanner;
import java.util.*;
public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        Graph graph = new Graph(n);
        for(int i = 1 ; i <= n ; i++){
            graph.v[i] = sc.nextInt();
        }
        for(int i = 1 ; i <= n - 1 ; i++){
            int u = sc.nextInt();
            int v = sc.nextInt();
            graph.add(v , u  , 0);
        }
        System.out.println(graph.dp());
    }
}
class Edge{
    int next;
    int arc;
    public Edge(int next, int arc){
        this.next = next;
        this.arc = arc;
    }
}
class Graph{
    int[] visited = new int[6005];
    int[] v = new int[6005];
    int[] head  = new int[6005];
    Edge[] edges;
    int count = 0;

    int[][] dp = new int[head.length][2];

    public Graph(int n){
        edges = new Edge[2 * n];
        for(int i = 0 ; i < head.length ;  i++){
            head[i] = -1;
        }
    }
    public void add(int x , int y , int weight){
        edges[count] = new Edge(head[x] ,  y);
        head[x] = count;
        count++;
        visited[y] = 1;
    }
    public int dp(){
        int root = 0;
        for(int i = 1 ; i < visited.length ; i++){
            if(visited[i] == 0){
                root = i;
                break;
            }
        }
        dfs(root);
        return Math.max(dp[root][0] , dp[root][1]);
    }
    public void dfs(int x){
        dp[x][0] = 0;
        dp[x][1] = v[x];
        int sum = 0;
        for(int i = head[x] ; i != -1 ; i = edges[i].next){
            dfs(edges[i].arc);
            sum += dp[edges[i].arc][0];
        }
        dp[x][1] += sum;
        for(int i = head[x] ; i != -1 ; i = edges[i].next){
            dp[x][0] += Math.max(dp[edges[i].arc][1] , dp[edges[i].arc][0]);
        }

    }
}
```

最大独立集问题解决：
1. dp[x]  [0/1] :以x为根节点的子树的最大权值
2. dp[x]  [0]  += Math.max(dp[v]  [0/1]);
3. dp[x]  [1]  += dp[v]  [0];
4. dp初始化可以独立，也可以在遍历过程中初始化；
5. 充分体现了叶子——》根的特点，首先是dfs向下遍历，是底层的dp表完成dfs，回来后在对当前根节点赋值；

