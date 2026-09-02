---
tags:
  - 线性dp
aliases:
---
应用场景：最优子结构 + 重叠子问题 ——》常用于最优求解
动态规划做题步骤：
1. 找状态数组：
	1. 确定状态数组：尝试一句话描述题目，涉及的参数就是状态数组的下标（某某状态下的值，某某状态就是下标）自底向上分析，可能是一维，可能是二维；
	2. 记忆化搜索：自底向上看——》状态数组；
2. 确定状态转移方程（一个状态怎么由另一个状态得到，初始状态自己分析）；
3. 初始状态；
4. 写状态转移部分代码
5. 输出答案

线性动态规划/线性dp：具有线性阶段性划分的动态规划算法——》如同线性函数一般，一个状态对应一个值，且状态由前面的状态所得；

常见题目：
1. 01背包
2. 序列问题：最长上升子序列，最长公共子序列
3. 最短编辑距离问题；

[lg3637](https://www.luogu.com.cn/problem/B3637)
# B3637 最长上升子序列

## 题目描述

这是一个简单的动规板子题。

给出一个由 $n(n\le 5000)$ 个不超过 $10^6$ 的正整数组成的序列。请输出这个序列的**最长上升子序列**的长度。

最长上升子序列是指，从原序列中**按顺序**取出一些数字排在一起，这些数字是**逐渐增大**的。

## 输入格式

第一行，一个整数 $n$，表示序列长度。

第二行有 $n$ 个整数，表示这个序列。

## 输出格式

一个整数表示答案。

## 输入输出样例 #1

### 输入 #1

```
6
1 2 4 1 3 4
```

### 输出 #1

```
4
```

## 说明/提示

分别取出 $1$、$2$、$3$、$4$ 即可。

>[!summary]+ 思路 最大上升子序列
>最长子序列不是连续的！！！而且可能在后面遇到比上一个最大的小的一段子序列可能比大的长度长
>DFS逻辑上遍历所有子序列，时间复杂度高
>动态规划：
>1. 最后答案一定是以某一个数为结尾的子序列，那么使dp[i] 为a[i] 为结尾的最大子序列；
>2. 假定dp[n] 与 dp[0] ~dp[n - 1] 间的关系：在 0 ~ n - 1 间找一个比a[n]小的一个子序列，将a[n] 加到a[i]后面构成一个新的子序列，因为前面已经是个结尾最大的了，所以在各最大挑一个最大的即可；


```java
import java.util.Scanner;
import java.util.*;
public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int max = 1;
        int[] arr = new int[n];
        int[] dp = new int[n];
        dp[0] = 1;
        for(int i = 0 ; i < n ; i++){
            arr[i] = sc.nextInt();
        }
        for(int i = 1 ; i < n ;i++){
            dp[i] = 1;
            for(int j = 0 ; j < i ; j++){
                if(arr[i] > arr[j]){
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                    max = Math.max(max, dp[i]);
                }
            }
        }
       System.out.print(max);
    }
}
```

[lk 095](https://leetcode.cn/problems/qJnOS7/)

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

**示例 1：**

**输入：**text1 = "abcde", text2 = "ace" 
**输出：**3  
**解释：**最长公共子序列是 "ace" ，它的长度为 3 。

**示例 2：**

**输入：**text1 = "abc", text2 = "abc"
**输出：**3
**解释：**最长公共子序列是 "abc" ，它的长度为 3 。

**示例 3：**

**输入：**text1 = "abc", text2 = "def"
**输出：**0
**解释：**两个字符串没有公共子序列，返回 0 。

**提示：**

- `1 <= text1.length, text2.length <= 1000`
- `text1` 和 `text2` 仅由小写英文字符组成。


>[!summary] + 思路 公共子序列
>子序列：顺序同即可，不必由大小比较
>从后面找，最后一个相等的字符一定是最长子序列的一部分，第一个不一定是
>dp[n][m]为第一个字符串的0~n ，第二个字符串的0~m的最大公共子序列；
>分析：
>1. 若最后一个相等：dp[n][m] = dp[n - 1][m - 1] + 1;
>2. 若最后一个不相等：
>	1. 字符串二的最后一个不起作用：dp[n][m] = dp[n][m - 1];
>	2. 字符串一的最后一个不起作用：dp[n][m] = dp[n - 1][m];
>	3. 字符串一二都不起作用：dp[n][m] = dp[n - 1][m - 1];此公式包含在1，2中

```java
lass Solution {

    public int longestCommonSubsequence(String text1, String text2) {

        int n_text1 = text1.length();

        int n_text2 = text2.length();

        text1 = " " + text1;

        text2 = " " + text2;

        int[][] dp = new int[n_text1 + 1][n_text2 + 1];

        for (int i = 0; i < n_text1; i++) {

            for (int j = 0; j < n_text2; j++) {

                dp[i][j] = 0;

            }

        }

        for (int i = 1; i <= n_text1; i++) {

            for (int j = 1; j <= n_text2; j++) {

                if(text1.charAt(i) == text2.charAt(j)){

                    dp[i][j] = dp[i - 1][j - 1] + 1;

                }else{

                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);

                }

            }

        }

        return dp[n_text1][n_text2];

    }

}
```
>[!summary]+ 反思
>使用字符相加使数组从1开始

[lg 2758](https://www.luogu.com.cn/problem/P2758)

# P2758 编辑距离

## 题目描述

设 $A$ 和 $B$ 是两个字符串。我们要用最少的字符操作次数，将字符串 $A$ 转换为字符串 $B$。这里所说的字符操作共有三种：

1. 删除一个字符；
2. 插入一个字符；
3. 将一个字符改为另一个字符。

$A, B$ 均只包含小写字母。

## 输入格式

第一行为字符串 $A$；第二行为字符串 $B$；字符串 $A, B$ 的长度均小于 $2000$。

## 输出格式

只有一个正整数，为最少字符操作次数。

## 输入输出样例 #1

### 输入 #1

```
sfdqxbw
gfdgw

```

### 输出 #1

```
4

```

## 说明/提示

对于 $100 \%$ 的数据，$1 \le |A|, |B| \le 2000$。
>[!summary] + 思路 最小编辑距离
>优先保存更多相等的字符；
>先从最后一个字符看：
>1. 最后一个字符相同，操作数与A的1~n - 1 和B 的 1~ m - 1的操作数同，即把a[1]~a[n - 1] 变为 b[1] ~b[m - 1]操作数同；dp[n][m] = dp[n - 1][m - 1];
>2. 最后一个字符不相同：有三个操作，取min
>	1. 执行替换操作：将a[n]变为b[m],操作数为1 + 把a[1]~a[n - 1] 变为b[1]~b[m - 1]的操作数；dp[n][m] = 1+ dp[n - 1][m - 1];
>	2. 执行删除操作：可能a[1] ~ a[n - 1]同，删掉a[n],操作数 1+ 把a[1] ~ a[n - 1]变为b[1] ~ b[m]的操作数；dp[n][m] =1 +  dp[n - 1][m];
>	3. 执行添加操作：a[n + 1]与b[m]同，操作数1 + 把a[1]~a[n] 变为b]1] ~ b[m - 1]的操作数；dp[n][m] =1+ dp[n][m - 1];
>3. dp[n][m] = x:将a[1] ~ a[n]变为b[1] ~ b[m]的最小操作数是x
>4. 初始化：d[i][0] 与 dp[0][j] ，第一个是只有A无B ， dp[i][0] = i;执行i次删除操作；第二个是只有B无A，执行j次添加操作；

```java
import java.util.Scanner;
import java.util.*;
public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String str1 = sc.nextLine();
        String str2 = sc.nextLine();
        int len1 = str1.length();
        int len2 = str2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];
        for(int i = 0 ; i <= len1 ; i++){
            dp[i][0] = i;
        }
        for(int j = 0 ; j <= len2 ; j++){
            dp[0][j] = j;
        }
        for(int i = 1 ; i <= len1 ; i++){
            for(int j = 1 ; j <= len2 ; j++){
                if(str1.charAt(i-1) == str2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1];
                }else{
                    dp[i][j] = 1 +  Math.min(dp[i - 1][j - 1] ,Math.min(dp[i-1][j], dp[i][j-1]));
                }
            }
        }
        System.out.println(dp[len1][len2]);
    }
}
```

总结：
1. 分析状态时，不考虑前面状态怎么实现的，默认已知；
2. dp方程的状态：看值随什么东西转移；
3. 找到所有会使状态转移的情况；
4. 通常从结尾处分析状态，常常会对尾部进行操作，如加到末尾，增删等；
