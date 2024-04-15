[KMP算法](https://www.acwing.com/problem/content/833/)

```c++
#include <iostream>

using namespace std;

const int N = 100010, M = 1000010;

char p[N], S[M];									//p为模式串，S为文本串
int n, m;											//n为p数组大小，m为S数组大小
int ne[N];											//next数组，表示模式串前后最大公共长度

int main()
{
    cin >> n >> p + 1 >> m >> S + 1;				   //下标从1开始
    
    for(int i = 2, j = 0; i <= n; i ++)                 //求next数组,p字符串下标从1开始，所以i = 2
    {
        while (j && p[i] != p[j + 1]) j = ne[j];	   //最少移动ne[j]才能继续进行下一次比较
        if (p[i] == p[j + 1]) j ++;
        ne[i] = j;	
    }
    
    for(int i = 1, j = 0; i <= m; i ++)                 //将模式串和文本串进行匹配
    {
        while (j && S[i] != p[j + 1]) j = ne[j];        //用i 和 j + 1进行比较
        if (S[i] == p[j + 1]) j ++;
        if (j == n)                                     //j指到了末尾，匹配成功
        {
            cout << i - n  << ' ';                      //输出起始下标
            j = ne[j];                                  //最少移动ne[j]才能继续进行下一次比较
        }
    }
    
    return 0;
}
```

