Dijkstra算法（未优化版）

```c++
//由于图的边数较多，为稠密图，所以用邻接矩阵进行存储，若为稀疏图则用邻接表进行存储
#include <iostream>
#include <algorithm>
#incldue <cstring>

using namespace std;

const int N = 510;

int n, m;
int dist[N];			//表示当前点距离第一个点的距离
int g[N][N];			//邻接矩阵，存储稠密图
bool st[N];				//判断当前点是否为最短距离的点

int Dijkstra()
{
	memset(dist, 0x3f, sizeof dist);	  //将dist全设为无穷大（0x3f表示无穷大）
    dist[1] = 0;						//第一个点到自身的距离为0
    for(int i = 0; i < n; i ++)
    {
        int t = -1; 
        for(int j = 1; j <= n; j ++)
            if (!st[j] && (t == -1 || dist[t] > dist[j]))	//j还未走且dist[t]比dist[j]大
                t = j;
       	st[t] = true;
        for(int j = 1; j <= n; j ++) 
            dist[j] = min(dist[j], dist[t] + g[t][j])	//从1t到j和从1到t再到j的距离取最小值
    }
    if (dist[n] == 0x3f3f3f3f) return -1;			//如果dist[n]还为无穷大的话输出-1
    return dist[n];
}

int main()
{
    cin >> n >> m;
    memset(g, 0x3f, sizeof g);
    while (m -- )
    {
        int x, y, z;
        cin >> x >> y >> z;
        g[x][y] = min(g[x][y], z);			//如果有重边则选择最小的作为距离
	}
    cout << Dijkstra() << endl;
    return 0;
}
```

