# 深度优先算法（DFS）

## 概念
通过递归的方式，不断向下探索，直到找到解，一种经典的搜索算法

## 模板
```
//首先定义剪枝函数,去除不符合条件的节点
check():
    if 满足条件: return true
    else: return false
   
//定义DFS函数 不断调用自身
dfs():
    if 找到所求解: 输出结果或进行其他操作
    if check = true:
        进行某种处理(并标记已访问)
        dfs()//调用自身 传入更新后的参数
        (回溯)
```
## 常见问题
### [USACO1.5] 八皇后 Checker Challenge

#### 题目描述

一个如下的 $6 \times 6$ 的跳棋棋盘，有六个棋子被放置在棋盘上，使得每行、每列有且只有一个，每条对角线（包括两条主对角线的所有平行线）上至多有一个棋子。

![](https://cdn.luogu.com.cn/upload/image_hosting/3h71x0yf.png)

上面的布局可以用序列 $2\ 4\ 6\ 1\ 3\ 5$ 来描述，第 $i$ 个数字表示在第 $i$ 行的相应位置有一个棋子，如下：

行号 $1\ 2\ 3\ 4\ 5\ 6$

列号 $2\ 4\ 6\ 1\ 3\ 5$

这只是棋子放置的一个解。请编一个程序找出所有棋子放置的解。  
并把它们以上面的序列方法输出，解按字典顺序排列。  
请输出前 $3$ 个解。最后一行是解的总个数。

#### 输入格式

一行一个正整数 $n$，表示棋盘是 $n \times n$ 大小的。

#### 输出格式

前三行为前三个解，每个解的两个数字之间用一个空格隔开。第四行只有一个数字，表示解的总数。

#### 样例 #1

##### 样例输入 #1

```
6
```

##### 样例输出 #1

```
2 4 6 1 3 5
3 6 2 5 1 4
4 1 5 2 6 3
4
```

###### 提示

【数据范围】  
对于 100% 的数据， 6 ≤ 𝑛 ≤ 13  

#### 代码
```
//n皇后 dfs回溯
#include<bits/stdc++.h>
using namespace std;
int n,queen[15],cal;  
//判断行列是否可行
bool check(int x,int y){
	//注意i的范围 只有i<=x成立 
    for(int i=1;i<=x;i++){
        if(queen[i]==y) return false;
        if(i+queen[i]==x+y) return false;
        if(i-queen[i]==x-y) return false;
    }
    return true;
}

void search(int h){
	//printf("%d",h); 
    if(h>n){
        cal++;
        if(cal<=3){
            for(int i=1;i<=n;i++){
                printf("%d ",queen[i]);
            }
            printf("\n");
        }
    }
    //dfs
    for(int l=1;l<=n;l++){
        if(check(h,l)){
            queen[h]=l;
            //调试
			//for(int j=1;j<n;j++) printf("%d ",queen[j]); 
            search(h+1);
            queen[h]=0;
        }
    }

}

int main() {
    cin >> n;
    search(1);
    printf("%d",cal);
    return 0;
}
```