---
tags:
  - 区间dp
---
求解一个区间的最优值——》划分为多个小区间求最优值<mark style="background: #FF5582A6;">（一般是找最小消耗）</mark>，合并多个小区间得到大区间的最优值
解决：
1. 划分：有不同划分方法，枚举不同划分方法（<mark style="background: #FF5582A6;">通常是枚举区间的分割点，找到最优办法</mark>）
2. 合并

<mark style="background: #FFB86CA6;">区间dp:对区间动态规划</mark>

核心思路：状态：dp[i]  [j] 表示i ~ j 的最优解（最小消耗）
枚举所有空间：
1. 先枚举小空间为大空间做准备，1不用自己求做初始化，为后面做准备；
2. 枚举所有可能空间，从左端点来枚举，有左端点就有右端点，同时出来；
3. 对已经出来的空间进行求最优解，从左端点作为分割点，求这个小空间的最优解；
本质：将问题分为两个小空间，求最优解，但是<mark style="background: #FF5582A6;">需要小空间的最优解</mark>——》将小空间的最优解求出，<mark style="background: #FF5582A6;">保存起来</mark>，一直到整个问题的空间。
```java
for(int len = 2 ; len <= n; len++){//枚举所有长度
	for(int i = 1 ; i + len - 1 <= n ; i++){//枚举所有区间
		int j = i + len - 1;
		for(int k = i; k < j ;k++){//求区间的最优解
			dp[i][j] = Math.min(dp[i][j] , dp[i][k] + dp[k + 1][j] + 合并消耗)
		}
	}
}
```



[lg 4170](https://www.luogu.com.cn/problem/P4170)
# P4170 [CQOI2007] 涂色

## 题目描述

假设你有一条长度为 $5$ 的木板，初始时没有涂过任何颜色。你希望把它的 $5$ 个单位长度分别涂上红、绿、蓝、绿、红色，用一个长度为 $5$ 的字符串表示这个目标：$\texttt{RGBGR}$。

每次你可以把一段连续的木板涂成一个给定的颜色，后涂的颜色覆盖先涂的颜色。例如第一次把木板涂成 $\texttt{RRRRR}$，第二次涂成 $\texttt{RGGGR}$，第三次涂成 $\texttt{RGBGR}$，达到目标。

用尽量少的涂色次数达到目标。

## 输入格式

输入仅一行，包含一个长度为 $n$ 的字符串，即涂色目标。字符串中的每个字符都是一个大写字母，不同的字母代表不同颜色，相同的字母代表相同颜色。

## 输出格式

仅一行，包含一个数，即最少的涂色次数。

## 输入输出样例 #1

### 输入 #1

```
AAAAA
```

### 输出 #1

```
1
```

## 输入输出样例 #2

### 输入 #2

```
RGBGR
```

### 输出 #2

```
3
```

## 说明/提示

$40\%$ 的数据满足 $1\le n\le 10$。

$100\%$ 的数据满足 $1\le n\le 50$。

![[图片/95c2d3778258f276ab6ca782008b4421.jpg|1004]]
>[!summary]+ 思路
>对一块木板进行上色——》对一块区域进行求最小代价——》区域dp
>但是有一个条件是可以一次性对一块区域进行上色——》一次性可以对多个格子上色，从一个例子逐渐可以看出若首尾颜色相同，则可以一次性上色，中间是否还有可以在继续的区间dp划分出来——》一次性可以上色，就去掉一个，1~i——》2~i或1~i-1 取min

```java
import java.util.Scanner;
import java.util.*;
public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        int len = str.length();
        str = " " + str;
        int[][] dp = new int[len + 1][len + 1];
        for(int i = 0; i <= len ; i++){
            for(int j = 0 ; j <= len ; j++) {
                if (i == j) {
                    dp[i][j] = 1;

                }
            }
        }
        for(int length = 2; length <= len; length++){
            for(int i = 1 ; i + length - 1 <= len; i++){
                int j = i + length - 1;
                if(str.charAt(i) == str.charAt(j)){
                    dp[i][j] = Math.min(dp[i + 1][j] , dp[i][j - 1]);
                }else{
                    dp[i][j] = len + 1;
                    for(int k = i ; k < j ; k++){
                        dp[i][j] = Math.min(dp[i][k] + dp[k + 1][j] , dp[i][j]);
                    }
                }
            }
        }
        System.out.println(dp[1][len]);
    }
}
```

使用场景：
1. 可以看到三层far循环——》O(N ^ 3)——》数据范围小
2. 区域求最值
<mark style="background: #FF5582A6;">1，2加在一起优先考虑区间dp</mark>
