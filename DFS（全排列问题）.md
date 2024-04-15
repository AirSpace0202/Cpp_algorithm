[DFS（全排列问题）](https://www.acwing.com/problem/content/844/)

```c++
#include <iostream>

using namespace std;

const int N = 10;

bool st[N];								//用于标记当前数字是否枚举过
int q[N];								//表示数字排列的方案
int n;

void dfs(int u)					//u表示当前走到的层数，第0层不放数字
{
    if (u == n)													//表示已经走到了最底层就输出方案
    {
        for(int i = 0; i < n; i ++) cout << q[i] << " ";
        cout << endl;
        return ;
	}
    for(int i = 1; i <= n; i ++)  
        if (!st[i])								//如果当前数字没被走过
    	{
        	q[u] = i;							//将数字列入排列方案
            st[i] = true;						//标记该数字
            dfs (u + 1);						//递归到下一层
            st[i] = false;						//递归到最底层时恢复现场将该数字标记为未走过
		}
}

int main()
{
    cin >> n;
    
    dfs (0);
    
    return 0;
}
```

