## 	Dijkstra[求最短路](https://www.acwing.com/problem/content/851/) I

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510;

int n, m;
int g[N][N];                //g数组表示邻接矩阵
int dist[N];                //dist数组表示此时的距离第一个点的距离
bool st[N];                 //用于判断当前点是否时最短距离

int Dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;            //第一个点到自身距离为0
    for(int i = 0; i < n; i ++)
    {
        int t = -1;
        for(int j = 1; j <= n; j ++)
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        st[t] = true;
        for(int j = 1; j <= n; j ++)
            dist[j] = min(dist[j], dist[t] + g[t][j]);
    }
    if (dist[n] == 0x3f3f3f3f) return -1;
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
        g[x][y] = min(g[x][y], z);          //如果有重边则保留最短距离
    }
    cout << Dijkstra() << endl;
    return 0;
}
```





## [有边数限制的最短路](https://www.acwing.com/problem/content/855/)

bellman_ford算法

```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 510, M = 10010;

struct Edge				//保存每条边
{
    int a;
    int b;
    int w;
} e[M];

int dist[N];			//某一个点距离原点的距离
int back[N];			//用于备份数组
int n, m, k;			//k为限制条件，最多走k次

void bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for(int i = 0; i < k; i ++)
    {
        memcpy(back, dist, sizeof dist);
        for(int j = 0; j < m; j ++)
        {
            int a = e[j].a;
            int b = e[j].b;
            int w = e[j].w;
            dist[b] = min(dist[b], back[a] + w);
        }
    }
}

int main()
{
    cin >> n >> m >> k;
    for(int i = 0; i < m; i ++)
    {
        int a, b, w;
        cin >> a >> b >> w;
        e[i] = {a, b, w};
    }
    
    bellman_ford();
    
    if (dist[n] > 0x3f3f3f3f / 2) puts("impossible");
    else cout << dist[n] << endl;
    
    return 0;
}
```





## spfa[求最短路](https://www.acwing.com/problem/content/853/)

```c++
#include <iostream>
#include <queue>
#include <cstring>

using namespace std;

const int N = 100010;
typedef pair<int, int> PII;					//第一个用于存储到原点的距离，第二个用于存储下标号

int h[N], e[N], w[N], ne[N], idx = 0;
int dist[N];
bool st[N];
int n, m;

void add(int a, int b, int c)
{
    e[idx] = b; w[idx] = c; ne[idx] = h[a]; h[a] = idx ++;  
}

void spfa()
{
    queue<PII> q;								//q为二元组队列
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;								//原点为1号点，距离为0
    q.push({0, 1});								
    st[1] = true;								//1号点走过然后标记为true
    while (q.size())
    {
        PII p = q.front();						//front表示取队首，然后将其弹出
        q.pop();							
        int t = p.second;						//存储下标号
        st[t] = false;							//让其能走
        for(int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])		//取最短路
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])						//若为走过就走它然后标记为true并放进队列
                {
                    st[j] = true;
                    q.push({dist[j], j});
                }
            }
        }
    }
}

int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    spfa();
    if (dist[n] == 0x3f3f3f3f) puts("impossible");
    else cout << dist[n];
    
    return 0;
}

```



