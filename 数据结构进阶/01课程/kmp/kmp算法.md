---
tags:
  - kmp
---
s[n]  ,  p[n] ——》p数组匹配子串s  ， 从下标0开始   
暴力匹配BF：
对每一个p[i]进行遍历，遇到不合适的直接靠for回溯，但是内部遍历时i不能动，所以加一个k来保存遍历的下标；
优化：找到时, j为遍历到length_s，是从0开始，但是因为for最后一次让j比最大下标大一，正好到length_s，当主串的下标在向后遍历的数大于等于length_p - length_s 时，还有机会，否则无机会；
返回：返回符合的字符串的第一个下标；
```java
class S{  
    char[] p;  
    char[] s;  
    public int length_p;  
    public int length_s;  
    public void getString(String p , String s){  
        this.p = p.toCharArray();  
        this.s = s.toCharArray();  
        length_p = p.length();  
        length_s = s.length();  
    }  
    public int BF(){  
  
        for(int i = 0 ; i <= length_p - length_s ; i++){  
            int k = i , j = 0;  
            for(j = 0; j < length_s ; j++){  
                if(p[k] == s[j]){  
                    k++;  
                }else{  
                    break;  
                }  
            }  
            if(j == length_s){  
                return i;  
            }  
        }  
        return -1;  
    }  
}
```
优化：
主串下标要往回走——》不让他往回走——》下标到x时不匹配，如果回走一直到x分析：
1. 中间不符合——》中断——》中间无匹配——》前缀为0个相同，p[x] 与s[0]比较，s[0]前无子串，还是不符合，不能在找前缀l——》x点无彻底无可能匹配了，x + 1
2. 下标回到一个点 i  ，使模串匹配到x——》此时匹配的串为模串的前缀，与主串的尾相同对应到模串是后缀——》最大前后缀
后面算法过程直接模串回到前缀+ 1，主串不动，若必不符合 x + 1

回到什么位置——》最大前后缀——》小了会被前面最大前缀截住，不让往后走，大了不匹配：
1. 求一个next[]数组来记录各节点返回的下标；next[]数组也是i下标的最大前后缀；
2. 注意如果从0开始前后缀是多少回到多少，因为前后缀还要加1，到要比较的位置，但是从0开始要 -1
求解：动态规划——》a[i - 1] = y 已知 ，为0~i - 2的最大前后缀，a[i]为0~i - 1的最大前后缀，要看i - 1位置是否相同判断最大前后缀
```java
public void KMP_next(){  
    next[0] = -1;  
    next[1] = 0;  
    int i = 1 , j = 0;  
    while(i < length_s - 1){  
        if(j == -1 || (s[i] == s[j])){  
            i++;  
            j++;  
            next[i] = j;  
        }else{  
            j = next[j];  
        }  
    }  
}  
public void KMP_next_val(){  
    next[0] = -1;  
    next[1] = 0;  
    int i = 1 , j = 0;  
    while(i < length_s - 1){  
        if(j == -1 || (s[i] == s[j])){  
            i++;  
            j++;  
            if(s[j] == s[i]){  
                next[i] = next[j];  
            }else{  
                next[i] = j;  
            }  
        }else{  
            j = next[j];  
        }  
    }  
}  
public int KMP(){  
    KMP_next();  
    int i = 0 , j = 0;  
    while(i < length_p && j < length_s){  
        if(j == -1 || s[j] == p[i]){  
            i++;  
            j++;  
        }else{  
            j = next[j];  
        }  
    }  
   if(j == length_s){  
       return i - j;  
   }  
   return -1;  
}
```
易错点：
1. 从0开始存储是i - j为起始位置
2. 从0开始存储最后判断是j >= length_s
3. 由于第i个位置是判断i+1的next的，所以while循环到length-2即可， i < length-1
4. 0,1为next的起始状态
5. 循环找的都是后面一个的next

next数组的含义：
1. 回到的位置next[j]  (x);
2. 最大前后缀，next是最大前后缀加1，但是如果是从0开始就是要-1，相抵消，要是从1开始就是，next[j] - 1 ; 
注意：<mark style="background: #FF5582A6;">next[j]</mark>是j - 1前面<mark style="background: #FF5582A6;">/0~j -1 的最大前后缀</mark>，一直到next[n - 1] 是0~n -1的最大前后缀——》一直找到``next[n]``,但是n位置上无字符无意义，但是可以找到n位置，不比n位置

题目：
1. 最小字符串：
	1. 第一个字符串的下标
	2. 有几个字符串
2. 最大前后缀的理解：找到了最大前后缀（最大前后缀不代表它的前面都是前后缀）
	1. 最短前后缀|244：
		1. 通过next,字符串 s, l ,r ,ls < ll < lr ,s是l的相同前后缀，l是r的相同前后缀——》s是r的相同前后缀（s与l前后缀相同且s的前后缀不可能大于s，可以看作是在l的前后缀中在取前后缀）；
		2. s是l的相同前后缀，t是l的相同前后缀，ls < lt ——》s是t的相同前后缀（同理） 
		3. ``next[n] = x``：0~n - 1 , x是最大前后缀的数量表示位置——》``next[x - 1]`` : 0~x - 2——》一直向前找，一直找到``next[min]`` = 0/-1,min的位置就是最小前后缀的位置
	2. 最小循环节l：
		1. 循环字符串 s 是由不循环字符串s' 不断循环构成，由于s‘不可以循环所以s’是s的最小循环节,右图所示 l = n - next[n];注意：next[n]是整个字符串的最大前后缀
		2. 子串/非循环字符串：找最长前后缀，字符串都可以看作是由一个循环字符串截出来的一部分（可以找它的最小循环节）：l = n - ``next[n]``  
      

![[Excalidraw/重构/最短前后缀|最短前后缀|4668]]
```java
int min = x;//让min保存最大前后缀的最大前后缀，这是最小前后缀位置+1，正好表示前面的最大前后缀
while(next[min] != 0 && next[min] != -1){
	min = next[min];
}
```

![[Excalidraw/重构/最小循环节|最小循环节]]
![[Excalidraw/重构/循环字符串的子串|循环字符串的子串|1028]]

[# P3375 【模板】KMP](https://www.luogu.com.cn/problem/P3375)

# P3357 最长k可重线段集问题

# P3375 【模板】KMP

## 题目描述

给出两个字符串 $s_1$ 和 $s_2$，若 $s_1$ 的区间 $[l, r]$ 子串与 $s_2$ 完全相同，则称 $s_2$ 在 $s_1$ 中出现了，其出现位置为 $l$。  
现在请你求出 $s_2$ 在 $s_1$ 中所有出现的位置。

定义一个字符串 $s$ 的 border 为 $s$ 的一个**非 $s$ 本身**的子串 $t$，满足 $t$ 既是 $s$ 的前缀，又是 $s$ 的后缀。  
对于 $s_2$，你还需要求出对于其每个前缀 $s'$ 的最长 border $t'$ 的长度。

## 输入格式

第一行为一个字符串，即为 $s_1$。  
第二行为一个字符串，即为 $s_2$。

## 输出格式

首先输出若干行，每行一个整数，**按从小到大的顺序**输出 $s_2$ 在 $s_1$ 中出现的位置。  
最后一行输出 $|s_2|$ 个整数，第 $i$ 个整数表示 $s_2$ 的长度为 $i$ 的前缀的最长 border 长度。

## 输入输出样例 #1

### 输入 #1

```
ABABABC
ABA

```

### 输出 #1

```
1
3
0 0 1 

```

## 说明/提示

### 样例 1 解释

 ![](https://cdn.luogu.com.cn/upload/pic/2257.png)。
 
对于 $s_2$ 长度为 $3$ 的前缀 `ABA`，字符串 `A` 既是其后缀也是其前缀，且是最长的，因此最长 border 长度为 $1$。


### 数据规模与约定

**本题采用多测试点捆绑测试，共有 4 个子任务**。

- Subtask 0（30 points）：$|s_1| \leq 15$，$|s_2| \leq 5$。
- Subtask 1（40 points）：$|s_1| \leq 10^4$，$|s_2| \leq 10^2$。
- Subtask 2（30 points）：无特殊约定。
- Subtask 3（0 points）：Hack。

对于全部的测试点，保证 $1 \leq |s_1|,|s_2| \leq 10^6$，$s_1, s_2$ 中均只含大写英文字母。

```java
import java.util.Scanner;
import java.util.*;
public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String p = sc.nextLine();
        String s = sc.nextLine();
        string ans = new string(s ,p);
        ans.kmp();
    }
}
class string{
    char s[] ;
    char p[];
    int next[] = new int[100010];

    public string(String s , String p){
        this.s = s.toCharArray();
        this.p = p.toCharArray();
    }
    public void kmp_next(){
        next[0] = -1;
        int i = 0;
        int j = -1;
        while(i < s.length){
            if(j == -1 || s[i] == s[j]){
                i++;
                j++;
                next[i] = j;
            }else{
                j = next[j];
            }
        }
    }
    public void kmp(){
        kmp_next();
        if(p.length < s.length){
            for(int k = 1 ; k <= s.length ; k++){
                System.out.print(next[k] + " ");
            }
            return;
        }
        int i = 0 , j = 0;
        while(i < p.length){
            if(j == -1 || s[j] == p[i]){
                i++;
                j++;
            }else{
                j = next[j];
            }
            if(j == s.length){
                System.out.println(i - j + 1);
                j = next[s.length];
            }
        }
        for(int k = 1 ; k <= s.length ; k++){
            System.out.print(next[k] + " ");
        }
    }
}
```