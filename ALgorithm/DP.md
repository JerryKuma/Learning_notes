# 动态规划

## 基本概念

一种将问题分解为逐个递进的子问题,用DP数组记录子问题的最优解,逐步推进,进而得到总问题的最优解的思路

## 常见题型

### 数字三角形模型

#### [USACO1.5] [IOI1994]数字三角形 Number Triangles

##### 题目描述

观察下面的数字金字塔。

写一个程序来查找从最高点到底部任意处结束的路径，使路径经过数字的和最大。每一步可以走到左下方的点也可以到达右下方的点。

![](https://cdn.luogu.com.cn/upload/image_hosting/95pzs0ne.png)

在上面的样例中，从 $7 \to 3 \to 8 \to 7 \to 5$ 的路径产生了最大权值。

##### 输入格式

第一个行一个正整数 $r$ ,表示行的数目。

后面每行为这个数字金字塔特定行包含的整数。

##### 输出格式

单独的一行,包含那个可能得到的最大的和。

##### 样例 #1

###### 样例输入 #1

```
5
7
3 8
8 1 0
2 7 4 4
4 5 2 6 5
```

###### 样例输出 #1

```
30
```

##### 提示

【数据范围】
对于 $100\%$ 的数据，$1\le r \le 1000$，所有输入在 $[0,100]$ 范围内。

#### 思路

1. 记忆化搜索+DFS，从下到上累加和可以重复使用
2. DP 状态转移方程：a[x][y]+=max(a[x+1][y],a[x+1][y+1]) 备份数组+当前点下行方向数组

#### 代码

```
思路1:
int n=4;
int a[9][9]={{1},
            {4,6},
            {8,3,9},
            {5,7,2,1}};
//记忆化搜索+DFS 时间复杂度o(n^2) 递归算法 
int f[9][9]//记录从下到上的累加和
int dfs(int x,int y){
    if(f[x][y]!=0) return f[x][y];//记忆化搜索  当前f有值 则直接返回 不再重复搜索 
    if(x==n-1) f[x][y]=a[x][y];//到底，直接赋值 
    else f[x][y]=a[x][y]+max(dfs(x+1,y),dfs(x+1,y+1));
    return f[x][y];
    }
}

思路2:
//DP 递推算法 时间复杂度0(n^2) 状态转移方程：a[x][y]+=max(a[x+1][y],a[x+1][y+1]) 
int b[9][9];//备份数组 
int p[9][9];//记录下一步的方向 
int main(){
	int x,y;
	//备份数组
	for(x=0;x<n;x++){
		for(y=0;y<n;y++){
			b[x][y]=a[x][y];
		}
	}
	for(x=n-2;x>=0;x--){
		for(y=0;y<=n;y++){
			if(a[x+1][y]>a[x+1][y+1]){
				a[x][y]+=a[x+1][y];
				p[x][y]=0;//向下 
			}
			else{
				a[x][y]+=a[x+1][y+1];
				p[x][y]=1;//向右下 
			}
		}
	}
	cout<<"max="<<a[0][0]<<endl;
	for(x=0,y=0;x<n-1;x++){
		cout<<b[x][y]<<"->";
		y+=p[x][y];//二维数组存储当前点的下一步方向 妙极了！！！！ y存储当前列数 
	}
	cout<<b[n-1][y]<<endl;
}
```

### 最长上升子序列

```
//AcWing 1017. 怪盗基德的滑翔翼
#include<bits/stdc++.h>
using namespace std;
 
//替换顺序
void rev(int* a,int len){
	for(int i=0;i<len/2;i++){
		int tmp;
		tmp=a[i];
		a[i]=a[len-i-1];
		a[len-i-1]=tmp;
	}
} 
 
解法1 时间复杂度o(n^2) DP:
int f[105],a[105],k,n;
int main(){
	cin>>k;
	while(k--){
		memset(a,0,sizeof(a));
		fill(f,f+105,1);
		int ans=1;
		cin>>n;
//		for(int i=0;i<n;i++){
//			printf("%d ",f[i]);
//		}
		for(int i=0;i<n;i++){
			cin>>a[i];
		}
		for(int i=0;i<n;i++){
			for(int j=0;j<i;j++){
				if(a[i]>a[j]){
					f[i]=max(f[j]+1,f[i]);
				}
			}
			ans=max(ans,f[i]);
		}
		rev(a,n);//reverse
		for(int i=0;i<n;i++){
			for(int j=0;j<i;j++){
				if(a[i]>a[j]) f[i]=max(f[j]+1,f[i]);
			}
			ans=max(ans,f[i]);
		}
		printf("%d\n",ans);
	}
} 

解法2 二分查找+动态更新b数组:
int b[105], a[105], k, n, len;
 
// 二分查找
int find(int x) {
    int L = 1, R = len, mid;
    while (L <= R) {
        mid = (L + R) / 2;
        if (x > b[mid]) L = mid + 1;
        else R = mid - 1;
    }
    return L;
}
 
int main() {
    cin >> k;
    while (k--) {
        memset(a, 0, sizeof(a));
        memset(b, 0, sizeof(b));
 
        cin >> n;
        for (int i = 0; i < n; i++) {
            cin >> a[i];
        }
 
        // 正向计算LIS
        len = 1;
        b[1] = a[0];
        for (int i = 1; i < n; i++) {
            if (a[i] > b[len]) {
                b[++len] = a[i];
            } else {
                b[find(a[i])] = a[i];
            }
        }
        int ans = len;
 
        // 反转数组后再计算LIS
        reverse(a, a + n);
        memset(b, 0, sizeof(b));
      
        len = 1;
        b[1] = a[0];
        for (int i = 1; i < n; i++) {
            if (a[i] > b[len]) {
                b[++len] = a[i];
            } else {
                b[find(a[i])] = a[i];
            }
        }
        ans = max(ans, len);
 
        cout << ans << endl;
    }
    return 0;
}
```

### 背包问题

#### 思路

确定状态变量->状态转移方程->确定边界条件

#### 01背包

每次打算放一个东西时，首先要考虑它放不放得下，放不下的话就直接不放；放得下的话，就要看放他划算还是不放它划算。

在具体代码实现过程中，列二维表格，定义二维数组f、一维数组w、一维数组c，f[i][j]表示前i个物品选择放入容量为j的背包后，背包中的最大价值,w[i]表示第i个物品的重量，c[i]表示第i个物品的价值。对二维数组f进行遍历赋值，判断条件如下：

第一种情况，当前i，j无法在背包中放下第i个物品，f[i][j]=f[i-1][j]，继承j容量背包下前i-1个物品的最大价值。

第二种情况，当前i，j可以在背包中放下第i个物品，则需比较f[i-1][j]（当前背包容量不放i的最大价值）与f[i-1][j-w[i]]+c[i]（当前背包放i导致背包容量减小至j-w[i]时，减小容量情况下背包不放i只放到i-1的最大价值）。若前者大于后者，则不放i，f[i][j]=f[i-1][j]；反之，则放i，f[i][j]=f[i-1][j-w[i]]+c[i]。即f[i][j]=max(f[i-1][j],f[i-1][j-w[i]]+c[i])

最终遍历完成后，f[a][b]即为背包容量为b时，选择前a件物品中任意件放入背包中的最大价值。

![](https://img-blog.csdnimg.cn/direct/45fc0788426a4fec98ae37d89741e01f.png)

```
//AcWing 423. 采药
#include<bits/stdc++.h>
using namespace std;
 
int t,m,f[101][1001],c[101],w[101];
int main(){
	cin>>t>>m;
	for(int i=1;i<=m;i++){
		cin>>w[i]>>c[i];
	}
	for(int i=1;i<=m;i++){
		for(int j=1;j<=t;j++){
			if(w[i]>j) f[i][j]=f[i-1][j];
			else{
				f[i][j]=max(f[i-1][j],f[i-1][j-w[i]]+c[i]);
			}
		}
	}
	printf("%d",f[m][t]);
	return 0;
}
```

#### 01背包变式

有i个工作,ti表示工作耗时,di表示工作截止时间,pi表示工作价值,求给定时间内的最大工作价值 工作不能同时进行 开始了就不能被打断直到完成

输入形式: 第一行 n m 表示n组数据 限时m; 后续n组 每组m行 ti di pi 表示第i个工作的相关信息

输出形式:n行 每行表示该组数据的最大工作价值

```
#include<bits/stdc++.h>
 
using namespace std;
 
using i64 = long long;
 
struct node{
    int t,d,p;
};
 
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
 
    int t;
    cin >> t;
 
    while (t--) {
 
        int n;
        cin >> n;
 
        vector<node> a(n);
        for(auto &i : a)
            cin >> i.t >> i.d >> i.p;
 
        sort(a.begin(),a.end(),[](node x,node y){
            return x.d < y.d;
        });
 
        const int N = 5000;
        vector<i64> dp(N + 10,INT_MIN);
 
        dp[0] = 0;
        for(int i = 0;i < n;i ++){
            for(int j = a[i].d;j>=a[i].t;j--)
                dp[j] = max(dp[j],dp[j-a[i].t]+a[i].p);
        }
 
        i64 ans = 0;
        for(int i = 0;i <= N;i ++)
            ans = max(ans,dp[i]);
 
        cout << ans << '\n';
 
    }   
 
    return 0;
}
```


