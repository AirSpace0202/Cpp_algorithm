## [数字三角形](https://www.acwing.com/problem/content/900/)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510;

int n;
int w[N][N], f[N][N];               //w[][]表示读入的三角形，f[][]表示走过的路线加起来的数

int main()
{
    cin >> n;
    for(int i = 1; i <= n; i ++)						//读入数字三角形
        for(int j = 1; j <= i; j ++)
            cin >> w[i][j];
            
    for(int i = 1; i <= n; i ++) f[n][i] = w[n][i];     //初始化最底层，从下往上做
    
    for(int i = n - 1; i > 0; i --)			//从第n - 1层开始分析
        for(int j = 1; j <= i; j ++)
            f[i][j] = max(f[i + 1][j] + w[i][j], f[i  + 1][j + 1] + w[i][j]);
    		//f[i + 1][j]表示f[i][j]左下方的点，f[i + 1][j + 1]表示f[i][j]右下方的点
    cout << f[1][1] << endl;
    
    return 0;
}
```





## [最长上升子序列](https://www.acwing.com/problem/content/897/)

![image-20230804173956056](C:\Users\29089\AppData\Roaming\Typora\typora-user-images\image-20230804173956056.png)

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n;
int a[N], f[N];			//a[]表示输入的数字，f[]表示最长上升子序列长度

int main()
{
    cin >> n;
    for(int i = 1; i <= n; i ++) cin >> a[i];		//处理输入
    
    for(int i = 1; i <= n; i ++)
    {
        f[i] = 1;       //只有a[i]一个数;
        for(int j = 1; j < i; j ++)					//遍历第i个数前面的数
            if (a[j] < a[i])
                f[i] = max(f[i], f[j] + 1);			//如果a[j] < a[i]的话说明是递增的，那么长度加1
    }
    
    int res = 0;
    for(int i = 1; i <= n; i ++) res = max(res, f[i]);
    
    cout << res << endl;
    
    return 0;
}
```



## [最短编辑距离](https://www.acwing.com/problem/content/904/)

状态表示：f（i, j）表示a字符串的1~i个字符与b字符串的1~j个字符相匹配

状态计算：1）增：f（i, j - 1）+ 1表示a的前i个与b的前j - 1个相匹配，再加上一个字母即可

​					2）删：f（i - 1, j）+ 1

​					3）改：f（i - 1, j - 1）+ 1 / 0，表示a的前i - 1个与b的前j - 1个相匹配，若最后一个也匹配就加0，否则加1

```c++
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 1010;

int n, m;
int f[N][N];
char a[N], b[N];

int main()
{
    cin >> n >> a + 1 >> m >> b + 1;				//下标从1开始防止f[i - 1]越界
    
    for(int i = 0; i <= m; i ++ ) f[0][i] = i;//增加操作，表示a的第0个要与b的第i个相匹配就要进行m次增加
    for(int i = 0; i <= n; i ++ ) f[i][0] = i;//删除操作，表示a的第i个要与b的第0个相匹配就要进行n次删除
    
    for(int i = 1; i <= n; i ++ )
        for(int j = 1; j <= m; j ++ )
        {
            f[i][j] = min(f[i][j - 1] + 1, f[i - 1][j] + 1);
            f[i][j] = min(f[i][j], f[i - 1][j - 1] + (a[i] != b[j]));
        }
        
    cout << f[n][m] << endl;
    
    return 0;
}
```







## [怪盗基德的滑翔翼（最长上升子序列）](https://www.acwing.com/problem/content/1019/)

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int a[N], f[N];
int n;

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> n;
        for(int i = 1; i <= n; i ++) cin >> a[i];
        //正向求解LIS问题
        int res = 0;
        
        for(int i = 1; i <= n; i ++) 
        {
            f[i] = 1;
            for(int j = 1; j < i; j ++)
                if (a[i] > a[j]) f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }
        
        //反向求解res
        for(int i = n; i > 0; i --)
        {
            f[i] = 1;
            for(int j = n; j > i; j --)
                if (a[i] > a[j]) f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }
        
        cout << res << endl;
        
    }
    
    return 0;
}
```







## 最长上升子序列[II](https://www.acwing.com/problem/content/898/)

lower_bound函数用于查找容器中大于等于某值的数，返回这个数的指针，三个参数分别为容器的开头和末尾和某值

```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
    int n;
    cin >> n;
    vector<int> arr(n);
    for(int i = 0; i < n; i ++) cin >> arr[i];
    
    vector<int> stk;
    stk.push_back(arr[0]);
    
    for(int i = 1; i < n; i ++)
    {
        if (arr[i] > stk.back()) stk.push_back(arr[i]);
        else *lower_bound(stk.begin(), stk.end(), arr[i]) = arr[i];
    }
    
    cout << stk.size() << endl;
    
    return 0;
}
```





## [石子合并（区间DP）](https://www.acwing.com/problem/content/284/)

![image-20230804200723461](C:\Users\29089\AppData\Roaming\Typora\typora-user-images\image-20230804200723461.png)

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310;

int n;
int s[N];               //前缀和数组
int f[N][N];            //f[][]表示区间范围

int main()
{
    cin >> n;
    for(int i = 1; i <= n; i ++) cin >> s[i], s[i] += s[i - 1];     //读入操作并处理前缀和数组
    
    for(int len = 2; len <= n; len ++)                  //先枚举区间长度
        for(int i = 1; i + len - 1 <= n; i ++)			//枚举区间长度下的左端点
        {
            int j = i + len - 1;						//j为区间右端点
            f[i][j] = 1e8;
            for(int k = i; k < j; k ++)					//枚举分界点
                f[i][j] = min(f[i][j], f[i][k] + f[k + 1][j] + s[j] - s[i - 1]);	//状态转移方程
        }
    //s[j] - s[i - 1]就是最后两堆合并的代价，也就是i ~ j的和，就是s[j] - s[i - 1]
    cout << f[1][n] << endl;		
    
    return 0;
}
```





## [树形DP](https://www.acwing.com/problem/content/287/)

![](https://cdn.acwing.com/media/article/image/2022/03/26/98857_bd915ef2ad-%E6%B2%A1%E6%9C%89%E4%B8%8A%E5%8F%B8%E7%9A%84%E8%88%9E%E4%BC%9A%EF%BC%8CDP%E7%8A%B6%E6%80%81%E5%88%86%E6%9E%90.jpg)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 6010;

int n;
int happy[N];               //表示高兴度
int h[N], e[N], ne[N], idx;
int f[N][2];                //f[u][0]表示不选u这个结点，f[u][1]表示选u这个结点而且不能选u的子节点
bool has_father[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++;
}

void dfs(int u)
{
    f[u][1] = happy[u];                         //先将u结点的高兴度加上
    
    for(int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];                           //j表示i的每个子节点
        dfs(j);								//递归每个子节点直到为叶子结点
        
        f[u][0] += max(f[j][0], f[j][1]);
        f[u][1] += f[j][0];                     //选了u就不能选u的子节点所以加上f[j][0]
        
    }
}

int main()
{
    cin >> n;
    for(int i = 1; i <= n; i ++) cin >> happy[i];               //读入每个职工的高兴度
    
    memset(h, -1, sizeof h);
    for(int i = 0; i < n - 1; i ++)
    {
        int a, b;
        cin >> a >> b;                                  //读入a和b之间的关系
        has_father[a] = true;                           //表示a有父节点
        add(b, a);                                      //b是a的父节点
    }
    
    int root = 1;
    while (has_father[root]) root ++;                   //当当前节点有父节点时就移到下一个结点
    
    dfs(root);
    
    cout << max(f[root][0], f[root][1]) << endl;
    
    return 0;
}
```





## [记忆化搜索](https://www.acwing.com/problem/content/903/)

![](https://cdn.acwing.com/media/article/image/2022/03/29/98857_dc76c836af-%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20220329210348.png)

![](https://cdn.acwing.com/media/article/image/2022/03/29/98857_3fe39957af-%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20220329212159.png)

+1是因为将走过之后的点当成起始点，那么原先的点就要加上

```c++
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 310;

int n, m;
int f[N][N];                //状态转移式
int h[N][N];                //读入网格滑雪场
int dx[4] = {-1, 0, 1, 0};
int dy[4] = {0, 1, 0, -1};

int dp(int x, int y)
{
    int &v = f[x][y];
    if (v != -1) return v;
    v = 1;
    for(int i = 0; i < 4; i ++)				//遍历四个方位
    {
        int xx = x + dx[i];
        int yy = y + dy[i];
        if (xx >= 1 && xx <= n && yy >= 1 && yy <= m && h[x][y] > h[xx][yy]) //不越界且可以滑
            v = max(v, dp(xx, yy) + 1);
    }
    return v;
}

int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= m; j ++)
            cin >> h[i][j];								//读入网格滑雪场
            
    memset(f, -1, sizeof f);							//初始化状态转移式
    int res = 0;
    
    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= m; j ++)
            res = max(res, dp(i, j));					//遍历一遍更新最大值
            
    cout << res << endl;
    
    return 0;
}
```

注意：

要记得把f数组初始化为−1

定义v的时候注意要加上&

如果这个点没计算过，v记得要赋值为1

v=max(v,dp(xx, yy)+1),不要忘了 +1

记得要返回v

