[BFS（走迷宫）](https://www.acwing.com/problem/content/846/)

```c++
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

typedef pair<int, int> PII;					//用于存储坐标

const int N = 110;

int n, m;
int g[N][N];                                //g数组用于存储地图
int d[N][N];                                //d数组用于存储的是每一个点与起始位置的距离
PII q[N * N];                               //q表示队列,表示位置

int bfs()
{
    int hh = 0, tt = 0;                     //队头和队尾都为0
    q[0] = {0, 0};
    memset(d, -1, sizeof(d));               //将d中的所有值设为-1表示还未走过
    d[0][0] = 0;                            //表示d[0][0]已经走过且距离起始点距离为0
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};       //表示四个方位，上右下左
    
    while (hh <= tt)
    {
        auto t = q[hh ++];							//将队头放入队列，扩展队头
        for(int i = 0; i < 4; i ++)						//四个方位进行尝试
        {
            int x = t.first + dx[i], y = t.second + dy[i];
            if (x >= 0 && x < n && y >= 0 && y < m && g[x][y] == 0 && d[x][y] == -1)   //不越界且下一																				个位置能走且没走过
            {
                d[x][y] = d[t.first][t.second] + 1;			//与起始点距离加1
                q[++ tt] = {x, y};
            }
        }
    }
    return d[n - 1][m - 1];						//返回右下角坐标距离起始点的距离
}

int main()
{
    cin >> n >> m;
    
    for(int i = 0; i < n; i ++)						//读入整个地图	
        for(int j = 0; j < m; j ++)
            cin >> g[i][j];
    
    cout << bfs() << endl;
    
    return 0;
}
```





[BFS（八数码）](https://www.acwing.com/problem/content/847/)

将每一个状态看成一个结点，从初始状态走到最终状态，边的权重为1，因此转化成bfs求最短路问题

1、queue<string> q用于存储字符串的状态

2、unordered_map<string, int> d关联string和int，string表示状态，int表示初始状态到达某个状态的距离

```c++
#include <iostream>
#include <cstring>
#include <unordered_map>
#include <algorithm>
#include <queue>

using namespace std;

int bfs(string start)
{
    string end = "12345678x";
    queue<string> q;                                        //状态是一个一维数组存储在队列中
    unordered_map<string, int> d;                           //key和value的关系，d用于关联string和int，string表示状态，int表示到初始状态到某个状态的距离
    q.push(start);                		                          //队列q用来放当前状态
    d[start] = 0;										//start状态距离起点为0
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};       //四个方位
    
    while (q.size())
    {
        auto t = q.front();                                 //t表示一个状态
        q.pop();
        
        int distance = d[t];                                //用变量存储此时的value
        if (t == end) return distance;                      //到达了end状态，返回的距离就是答案
        
        int k = t.find('x');                                //k表示当前状态中x的下标
        int x = k / 3, y = k % 3;                           //表示x在3 * 3方阵中的横纵坐标
        
        for(int i = 0; i < 4; i ++)
        {
            int a = x + dx[i], b = y + dy[i];               //x周围的元素
            if (a >= 0 && a < 3 && b >= 0 && b < 3)
            {
                swap(t[k], t[a * 3 + b]);				//利用下标进行交换里面的字符
                if (!d.count(t))						//当前状态为出现过就放入q中
                {
                    d[t] = distance + 1;
                    q.push(t);
                }
                swap(t[k], t[a * 3 + b]);
            }
        }
    }
    return -1;
}

int main()
{
    string start;
    for(int i = 0; i < 9; i ++)			//读入9个字符存入start中
    {
        char c;
        cin >> c;
        start += c;
    }
    cout << bfs(start) << endl;
    return 0;
}
```

