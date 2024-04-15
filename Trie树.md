## [Trie](https://www.acwing.com/problem/content/837/)树（高效地存储和查找字符串集合的数据结构）

首先创建一个空的root结点作为树的根，然后将每一个字符串以树的形式进行存储，并在字符串的最后一个字符上做标记表示当前字符串结束，最后在树中进行查询

![image-20230710193624760](C:\Users\29089\AppData\Roaming\Typora\typora-user-images\image-20230710193624760.png)

```c++
#include <iostream>

using namespace std;

const int N = 100010;

int son[N][26], cnt[N], idx;    //son表示当前字符串的位置，26表示每个节点最多连26条边因为只有26个字母，cnt表示当前字符串出现的次数，idx表示当前用到的下标
char str[N];

void Insert(char str[])
{   
    int p = 0;                  //p代表根节点
    
    for(int i = 0; str[i]; i ++)
    {
        int u = str[i] - 'a';				//转换成数字进行存储
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];          //将根节点往下移
    }
    
    cnt[p] ++;                  //当前字符串出现次数+1
}

int query(char str[])
{
    int p = 0;
    
    for(int i = 0; str[i]; i ++)
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;   //如果字符串不存在就返回0，否则将根节点下移
        p = son[p][u];
    }
    
    return cnt[p];
}

int main()
{
    int n;
    cin >> n;
    while (n --)
    {
        char op[2];
        cin >> op >> str;
        if (op[0] == 'I') Insert(str);
        else cout << query(str) << endl;
    }
    
    return 0;
}


```

