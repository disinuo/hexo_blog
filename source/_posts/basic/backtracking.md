---
title: 计算机解决问题的通用套路-回溯
date: 2021-01-14 17:01:03
mathjax: true
categories:
- 
tags: 
- 
---

## 关于回溯，你一定要知道的

* Q: 回溯的定义与核心思想
A: 根据百度百科的定义，回溯算法实际上一个类似枚举的搜索尝试过程，主要是在搜索尝试过程中寻找问题的解，当发现已不满足求解条件时，就“回溯”返回，尝试别的路径。回溯算法的基本思想是：从一条路往前走，能进则进，不能进则退回来，换一条路再试。回溯算法（Backtrack）是一种试错思想，本质上是深度优先搜索。即：从问题的某一种状态出发，依次尝试现有状态可以做出的选择从而进入下一个状态。递归这个过程，如果在某个状态无法做更多选择，或者已经找到目标答案时，则回退一步甚至多步重新尝试，直到最终所有选择都尝试过。

* Q：回溯算法涉及到的一些概念
A: 问题的解空间（Solution Space)：针对一个问题的某一种枚举元素，我们做出多次枚举，这样子下来所有的解会构成一个树形结构，树的每一个节点是该问题的一个可能解，我们把这些可能解的集合称之为解空间。
状态（**State** of this problem)：当前的搜索深度，以及该深度的一些局面信息
选择（avaiable **Choosen** based on current state) ：基于当前状态能够进一步做出的**选择**
路径（**Path** of choosen for solving this problem）：为了解决这个问题，走到当前状态，每一步做出的选择构成了一个列表，这个列表称之为**路径**
结果集（**Result**: all paths which can slove this problem)：对于求解可行解或者所有解类型的回溯问题，我们需要把所有的可行路径记录下来，这个用来存储可行路径的容器我们称之为**结果集**

Q：回溯算法的三要素
A：路径：已经做出的选择
选择列表：当前状态可以做出的选择
结束条件：选择列表为空，或者找到目标

<!-- more -->

* Q：每一类经典问题的解空间大小

|  问题 | 解性质|枚举对象 |选择数   |搜索空间|原始解空间大小|
|  ----  | ----|---- |----  |----|----  |
|组合问题（从N个数字中选K个）|所有解| N个数字 |2（选OR不选）|子集树|$2^N$|
|组合问题（从N个数字中选K个）|所有解| K个坑位 |N（每一个坑位都有N个数字）|N叉树|$N^K$或者${N \choose K}$|
|子集问题（求包含N个数字的集合的所有子集）|所有解| N个数字 |2（选OR不选）|子集树|$2^N$|
|排列问题（求N个字符的全排列）|所有解| N个坑位 |N（每一个坑位都有N个数字）|排列树|$N!$|
|排列问题（求N个字符的全排列）|所有解| N个数字 |N(每一个数字都有N个坑位）|排列树|$N!$|
|N皇后问题|可行解  |N行|N列可以选择|排列树|$N!$|
|N皇后问题| 可行解|$N^2$个格子 |2（选OR不选）|子集树|$2^{N^2}$|
|01背包问题| 最优解|$N$个物品 |2（选OR不选）|子集树|$2^N$|

![回溯算法的典型例题和适用特点](http://cdn.b5mang.com/202111402329.png)
注意上图中的解描述向量，

从上面的表格可以看出，即使是同一个问题，如果枚举对象不一样，就会得到完全的解空间树。我们在解决实际问题时，一定要谨慎选择枚举对象。

另外，子集树、排列树是比较常见的两类解空间树。
当所给问题是从n个元素的集合S中找出S满足某种性质的子集时，相应的解空间称为子集树。例如：n个物品的0-1背包问题所相应的解空间是一棵子集树，这类子集树通常有$2^n$个叶结点，其结点总数为$2^{n+1}-1$。

当所给问题的确定n个元素满足某种性质的排列时，相应的解空间树称为排列树。排列树通常有$n!$个叶结点，因此遍历排列树需要$n!$的计算时间。

解空间树大小与树的深度以及宽度有关：深度（for循环的层数）与宽度（每层的选择数）

* Q: 回溯算法解决问题的一般步骤
A：

```cpp
    //某一个STATE，在CHOOSE的作用下，可以切换到一个新的STATE。有多少种CHOICE，就会有多少种新的STATE。
    //退出条件：（1）对于最优解：选择列表为空（2）获取单个可行解问题：选择列表为空或者找到一组解
    vector<PATH> result; //结果集用来存放路径-PATH
    vector<CHOICE> path;
    bool<STATE> visited;
    void dfs(STATE cur_state) {
        if (is_solution(cur_state)) {
            result.push_back(path);
            //这里是否return，取决于我们是仅需获取一个可行解还是要获取所有可行解
            return;
        }

        vector<CHOICE> choices = get_all_choices_by_state(cur_state);
        //这里隐含一个退出条件，如果该state下可以做出的可行选择为空，回溯也会结束。
        for (each choice : choices) {
            STATE new_state = trans_to_new_state_by_choice(cur_state, choice);
            if (visited(new_state)) { 
                continue;
            }
            path.append(choice);
            visited[new_state] = true;
            dfs(new_state);
            visited[new_state] = false;
            path.remove(choice);
        }
    }
```

* Q：写出正确的回溯算法为什么那么难？
A: 写不出正确的回溯算法主要有以下几个原因：
第一，没有合适的代码模版。上面便提供了一个比较通用的模版，建议读者在理解的基础上背过它。
第二，搞不清楚什么时候return,什么时候回溯，以及怎么回溯。一般而言，对于寻找最优解或者找到所有可行解的问题，我们需要在没有更多选择的时候进行return。而对于寻找一个合法解的情况，我们在找到一组合法解的时候，直接return即可。而对于具体可以见：[findPathInTree](../findPathInTree.html) 。
第三，无重复元素的回溯搞得定，但是不会处理有重复元素的情况。这里需要理清楚**树枝去重**与**树层去重**两个概念
可以参考：
https://zhuanlan.zhihu.com/p/339849416
https://www.jianshu.com/p/23737ee122bf

## 回溯相关题目索引

|  题目分类 | 题目名称 |考察点   |其他说明|
|  ----  | ---- |----  |----  |
|排列组合| [回溯系列-组合排列](../classic_mutant/combination_permutation.html)  ||


地上有一个 m 行和 n 列的方格，横纵坐标范围分别是 0∼m−1 和 0∼n−1。

一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格。

但是不能进入行坐标和列坐标的数位之和大于 k 的格子。

请问该机器人能够达到多少个格子？

样例1
输入：k=7, m=4, n=5

输出：20
样例2
输入：k=18, m=40, n=40

输出：1484

解释：当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。
      但是，它不能进入方格（35,38），因为3+5+3+8 = 19。

```cpp
class Solution {
public:
    int t;
    vector<bool> visit;
    int cnt;
    
    bool check(int i, int j) {
        return (i / 10 + i % 10 + j / 10 + j % 10) <= t;
    }
    
    void dfs(int i, int j, int rows, int cols){
        if (!check(i, j)) return;
        visit[i * cols + j] = true;
        cnt++;
        
        int dx[4] = {1, 0, -1, 0}, dy[4] = {0, 1, 0, -1};
        
        for (int k = 0; k < 4; ++k) {
            int ni = i + dx[k], nj = j + dy[k];
            if (ni >= 0 && ni < rows && nj >= 0 && nj < cols && !visit[ni * cols + nj]) {
                dfs(ni, nj, rows, cols);
            }
        }
    }
    
    int movingCount(int threshold, int rows, int cols)
    {
        if (!rows && !cols) return 0;
        t = threshold;
        visit = vector<bool>((rows + 1) * (cols + 1), false);
        dfs(0, 0, rows, cols);
        return cnt;
    }
};
```


请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。

路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。

如果一条路径经过了矩阵中的某一个格子，则之后不能再次进入这个格子。

注意：

输入的路径不为空；
所有出现的字符均为大写英文字母；
样例
matrix=
[
  ["A","B","C","E"],
  ["S","F","C","S"],
  ["A","D","E","E"]
]

str="BCCE" , return "true"

str="ASAE" , return "false"

```cpp
class Solution {
public:
    bool dfs(vector<vector<char>>& matrix, string &str, int u, int x, int y) {
        //视频中的错误写法（测试用例不全导致）
        //if (u == str.length()) return true;
        //if (matrix[sx][y] != str[u]) return false;

        //肯定不合适
        if (matrix[x][y] != str[u]) return false;
        //已经到底了，返回ok
        if (u == str.length() - 1) return true; 

        int t = matrix[x][y];
        matrix[x][y] = '#';

        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};

        for (int i = 0; i < 4; ++i) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < matrix.size() && b >= 0 && b < matrix[a].size()) {
                if (dfs(matrix, str, u + 1, a, b)) return true;
            }
        }

        matrix[x][y] = t;
        return false;
    }

    bool hasPath(vector<vector<char>>& matrix, string &str) {
        for (int i = 0; i < matrix.size(); ++i) {
            for (int j = 0; j < matrix[i].size(); ++j) {
                if (dfs(matrix, str, 0, i, j))
                {
                    return true;
                }
            }
        }

        return false;
    }
};

```