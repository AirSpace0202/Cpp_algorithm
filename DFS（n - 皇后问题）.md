[DFS（n - 皇后问题）](https://www.acwing.com/problem/content/845/)

[对角线问题](https://www.acwing.com/problem/content/discussion/content/6702/)

```c++
#include <iostream>

using namespace std;

const int N = 20;

int n;
char g[N][N];											//用于输出方案
bool row[N], col[N], dg[N], udg[N];						//dg[]为对角线（所有右上 -> 左下的连线），udg[]为反对角线（所以左上 -> 右下的连线）

void dfs(int x, int y, int s)                           //x,y表示当前枚举到的位置，s表示此时皇后的个数
{
    if (y == n) y = 0, x ++;                            //当纵坐标到最后，就跳转到下一行的第一个位置
    if (x == n)                                         //当前位置在最后一行
    {
        if (s == n)                                     //如果此时皇后数量是题目中规定的数量就进行输出
        {
            for(int i = 0; i < n; i ++) puts(g[i]);
            puts("");
        }
        return ;
    }
    
    dfs (x, y + 1, s);                                  //第一种情况，表示不放皇后
    
    //对角线问题
    if (!row[x] && !col[y] && !dg[x + y] && !udg[x - y + n])	//第二种情况，表示放皇后
    {
        g[x][y] = 'Q';		
        row[x] = col[y] = dg[x + y] = udg[x - y + n] = true;
        
        dfs (x, y + 1, s + 1);
        
        row[x] = col[y] = dg[x + y] = udg[x - y + n] = false;       //恢复现场
        g[x][y] = '.';
    }
}

int main()
{
    cin >> n;
    for(int i = 0; i < n; i ++)
        for(int j = 0; j < n; j ++)
            g[i][j] = '.';
    
    dfs (0, 0, 0);
    
    return 0;
}
```

​	