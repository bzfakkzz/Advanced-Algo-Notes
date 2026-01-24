[P1004 [NOIP 2000 提高组] 方格取数 - 洛谷](https://www.luogu.com.cn/problem/P1004)



`f[i][j][k][l]`表示第一条路径走到`(i,j)`第二条路径走到`(k,l)`的最大值

循环中需要判断当前方格`(i,j)`和`(k,l)`是否相同，从而确定当前一步能获取的权值



```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

int n;
int u, v, w;
int l;
ll a[10][10];
ll f[10][10][10][10];

// 同时走步数，如果两条路走到不同的方格就权值累加计算，否则只计算一次

void solve()
{
    cin >> n;

    while (cin >> u >> v >> w, u || v || w)
    {
        a[u][v] = w;
    }

    f[1][1][1][1] = a[1][1];

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            for (int k = 1; k <= n; k++)
            {
                l = i + j - k;

                if (l<1 || l>n)continue;

                w = a[i][j];

                if (i != k && j != l)w += a[k][l];

                f[i][j][k][l] = max(f[i][j][k][l], f[i - 1][j][k - 1][l] + w);
                f[i][j][k][l] = max(f[i][j][k][l], f[i][j - 1][k][l - 1] + w);
                f[i][j][k][l] = max(f[i][j][k][l], f[i - 1][j][k][l - 1] + w);
                f[i][j][k][l] = max(f[i][j][k][l], f[i][j - 1][k - 1][l] + w);
            }
        }
    }

    cout << f[n][n][n][n];  // 第一条路径走到(n,n)，第二条路径走到(n,n)
}

int main()
{
    solve();

    return 0;
}
```



[Problem - B - Codeforces](https://codeforces.com/contest/2023/problem/B)



分析：



`ans=max(ans,s[i]-f[i])` ，

从idx一直submit，submit所有<=idx的任务   



f[i]表示跳到任何`<= i`的最小花费

> > `while(!q.empty() && q.top().second<i) q.pop();`



小根堆存放所有`{最小花费，i}`

1. 从>=i一路submit到i

2. 从点i，skip到b[i]
   
   

```cpp
void solve()
{
    cin>>n;

    for(int i=1;i<=n;i++)cin>>a[i];
    for(int i=1;i<=n;i++)cin>>b[i];
    for(int i=1;i<=n;i++)f[i]=2e18;

    priority_queue<pii,vector<pii>,greater<pii>> q;

    q.push({0,1});

    sum=0,ans=0;

    for(int i=1;i<=n;i++)
    {    
        while(!q.empty()&&q.top().second<i)q.pop();

        if(q.empty())break;

        f[i]=q.top().first;

        if(b[i]<=i)continue;

        q.push({q.top().first,b[i]});
    }

    for(int i=1;i<=n;i++)sum+=a[i],ans=max(ans,sum-f[i]);

    cout<<ans<<endl;
}
```





[0最小权值 - 蓝桥云课 (lanqiao.cn)](https://www.lanqiao.cn/problems/1563/learning/)



如果一个结点 v 有左子树 L, 右子树 R，分别有 C(L) 和 C(R) 个结点，

则 $W(v)=1+2W(L)+3W(R)+(C(L))^2C(R)$



树的权值定义为树的根结点的权值。

对于一棵有 2021 个结点的二叉树，树的权值最小可能是多少？



分析：



`f[i]`表示当前树节点总数为`i`时的最小权值，初始状态`f[0]=0`



第一重循环枚举当前节点总数`i`，第二重循环枚举当前左子树节点个数`j`，

**根节点算一个结点**，此时右子树节点个数为`i-1-j`



```cpp
void solve()
{
    memset(f,0x3f,sizeof(f));

    f[0]=0;

    for(int i=1;i<=2021;i++)  // 总结点个数
    {
        for(int j=0;j<=i-1;j++)  // 左子树结点个数
        {
            f[i]=min(f[i],1+2*f[j]+3*f[i-1-j]+1ll*j*j*(i-1-j));
        }
    }

    cout<<f[2021]<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

const int N = 2e3 + 50;

ll f[N];

int main()
{
    memset(f, 0x3f, sizeof(f));

    f[0] = 0;

    for (int i = 1; i <= 2021; i++)  // 当前节点总数（包括根节点）
    {
        for (int j = 0; j <= i - 1; j++)  // 枚举左子树结点
        {
            f[i] = min(f[i], 1 + 2 * f[j] + 3 * f[i - 1 - j] + 1ll * j * j * (i - 1 - j));
        }
    }

    cout << f[2021] << endl;

    return 0;
}
```





[0费用报销 - 蓝桥云课 (lanqiao.cn)](https://www.lanqiao.cn/problems/2195/learning/)



要求小明提交的票据中任意两张的日期差不小于 K 天, 且总金额不得超过实际 差旅费用 M



公司的同事们一起给小明凑了 N 张票据, 

小明现在想要请你帮他整理一 下, 从中选取出符合财务要求的票据, 并使总金额尽可能接近 M



分析：



`f[i][j]`表示到第`i`个票据为止能/否总金额为`j`，

初始状态`f[0][0]`



sort一遍所有数据，

用idx记录当前满足要求的最大坐标，`while(a[i].first-a[idx+1].first>=k)idx++;`



`f[i][j]=f[i-1][j]   // 承接上一个状态`

`f[i][j]|=f[idx][j-a[i].second]   // 选择当前票据`



```cpp
const d=[0,31,28,31,30,31,30,31,31,30,31,30,31]

struct node
{
    int d,v;

    bool operator<(const node&w)
    {
        if(d<w.d)return true;
        else if(d==w.d&&v<w.v)return true;
        else return false;
    }
}a[N];

void solve()
{
    cin>>n>>m>>k;

    for(int i=1;i<=12;i++)s[i]=s[i-1]+d[i];

    for(int i=1;i<=n;i++)
    {
        cin>>mon>>day>>v;

        a[i]={s[mon-1]+day,v};  // 1. 之前的mon-1个月 2. 这个月的day天
    }

    sort(a+1,a+n+1);

    idx=0;

    for(int i=1;i<=n;i++)
    {
        while(a[i].d-a[idx+1].d>=k)idx++;  // 1. 天数满足要求

        for(int j=0;j<=m;j++)
        {
            f[i][j]=f[i-1][j];

            if(j>=a[i].d)f[i][j]|=f[idx][j-a[i].v];  // 2. 钱数满足要求
        }        
    }

    for(int i=m;i>=0;i--)
    {
        if(f[n][i])
        {
            cout<<i<<endl; break;
        }
    }

    return 0;    
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> pii;

const int N = 1e3 + 10, M = 5e3 + 10;
const int d[20] = { 0,31,28,31,30,31,30,31,31,30,31,30,31 };

int n, m, k;
int mon, day, v;
pii a[N];
int s[20];
bool f[N][M];

// f[i][j]表示遍历到第i个位置为止，总价值为j是否能满足

int main()
{
    cin >> n >> m >> k;

    for (int i = 1; i <= 12; i++)
    {
        s[i] = s[i - 1] + d[i];
    }

    for (int i = 1; i <= n; i++)
    {
        cin >> mon >> day >> v;

        a[i] = { s[mon - 1] + day,v };  // 走到当前日期一共用了多少天
    }

    sort(a + 1, a + n + 1);

    f[0][0] = true;

    int idx = 0;

    for (int i = 1; i <= n; i++)
    {
        // 越往后满足要求的j越大

        while (a[i].first - a[idx + 1].first >= k)idx++;  // 走到刚好满足要求的idx

        for (int j = m; j >= a[i].second; j--)
        {
            f[i][j] = f[i - 1][j];

            f[i][j] |= f[idx][j - a[i].second];
        }
    }

    for (int i = m; i >= 0; i--)
    {
        if (f[n][i])
        {
            cout << i << endl;

            break;
        }
    }

    return 0;
}
```





[E-来硬的_牛客小白月赛92 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/81126/E)



求在题给条件下得到m单位铁矿石的最小时间



分析：



`f[i][j][k]`表示遍历到第`i`块煤，至少得到`j`单位铁矿石，当前是/否使用过魔法的最小时间



1. k==0
   
   1. 没有用上当前煤就直接得到`j`单位铁矿石
   
   2. 用上当前煤得到`j`单位铁矿石
   
   `f[i][j][0]=min(f[i-1][j][0],f[i-1][max(0,j-a[i].x)][0]+a[i].y);`

2. k==1
   
   1. 没有用上当前煤就直接得到`j`单位铁矿石
   
   2. 魔法在之前已经被用过，当前就只能普通烧煤
   
   3. 魔法在之前没被用过，当前直接用魔法
   
   `f[i][j][1]=f[i-1][j][1];`
   `f[i][j][1]=min(f[i][j][1],f[i-1][max(0,j-a[i].x)][1]+a[i].y);`
   `f[i][j][1]=min(f[i][j][1],f[i-1][max(0,j-2*a[i].x)][0]+a[i].y/2);`
   
   

最终结果是`min(f[n][m][0],f[n][m][1])`，

遍历完所有`a[i]`，求得到m个单位铁矿石最少需要多少时间



```cpp
void solve()
{
    cin>>n>>m;

    for(int i=1;i<=n;i++)
    {
        cin>>a[i].x>>a[i].y;
    }

    vector<vector<vector<ll>>>f(n+1,vector<vector<ll>>(m+1,vector<ll>(2,inf)));

    f[0][0][0]=0;

    for(int i=1;i<=n;i++)
    {
        for(int j=0;j<=m;j++)
        {
            f[i][j][0]=min(f[i-1][j][0],f[i-1][max(0,j-a[i].x)][0]+a[i].y);

            f[i][j][1]=min(f[i-1][j][1],
             min(f[i-1][max(0,j-a[i].x)][1]+a[i].y,f[i-1][max(0,j-2*a[i].x)][0]+a[i].y/2));
        }
    }

    cout<<min(f[n][m][0],f[n][m][1])<<endl;
}
```



```cpp
#include <bits/stdc++.h>
#include <functional>

#define alls(a) a.begin(),a.end()
#define emb emplace_back
#define pub push_back
#define pob pop_back
#define puf push_front
#define pof pop_front
#define fi first
#define se second
#define No puts("No")
#define Yes puts("Yes")
#define NO puts("NO")
#define YES puts("YES")

using namespace std;
typedef long long ll;
//typedef __int128 lll; // G++(32位)不支持
typedef unsigned long long ull;
typedef pair<int, int> pii;

const int N = 1e5 + 10;
const int mo = 1e9 + 7;
const ll inf = 2e18 + 10;

int n, m;

void solve()
{
    cin >> n >> m;

    vector<vector<vector<ll>>>f(n + 1, vector<vector<ll>>(m + 1, vector<ll>(2, inf))); // 元素都在后面声明

    vector<pii>a(n + 1, { 0,0 });

    for (int i = 1; i <= n; i++)
    {
        cin >> a[i].first >> a[i].second;
    }

    // f[i][j][0/1]表示遍历到第i块煤为止，烧了j单位铁矿石，
    // 当前是否用过魔法，至少需要多少时间

    f[0][0][0] = 0;

    for (int i = 1; i <= n; i++)
    {
        for (int j = 0; j <= m; j++)
        {
            // 当前为止没用过魔法

            // 1. 之前已经烧到j的状态，f[i-1][j][0]
            // 2. 这一次刚好烧到j的状态，f[i-1][max(0,j-a[i].first)][0]+a[i].second

            f[i][j][0] = min(f[i - 1][max(0, j - a[i].first)][0] + a[i].second, f[i - 1][j][0]);

            // 当前为止用过魔法

            // 1. 之前已经烧到j的状态，f[i-1][j][1]
            // 2. 当前位置刚好使用魔法，f[i-1][max(0,j-2*a[i].first)][0]+a[i].second/2
            // 3. 之前位置已经使用魔法，当前位置不能用魔法，f[i-1][max(0,j-a[i].first)][1]+a[i].second

            f[i][j][1] = min(f[i - 1][max(0, j - a[i].first)][1] + a[i].second,
                min(f[i - 1][max(0, j - 2 * a[i].first)][0] + a[i].second / 2, f[i - 1][j][1]));
        }
    }

    cout << min(f[n][m][0], f[n][m][1]) << endl;
}

int main()
{
    int t;

    //cin >> t;

    t = 1;

    while (t--)solve();

    return 0;
}
```





[P1558 - [蓝桥杯2021初赛] 砝码称重 - New Online Judge (ecustacm.cn)](http://oj.ecustacm.cn/problem.php?id=1558)



每个砝码只能用一次，

问最多能表示多少种重量



分析：



砝码的状态：

1. 不放`j`

2. 放左边`max(0,abs(j-w[i]))`

3. 放右边`min(sum,j+w[i])`
   
   

`f[i][j]`表示枚举到第i个砝码，重量为j是否能被表示出来



最后统计`f[n][i]`的总和



```cpp
void solve()
{
    cin>>n;

    for(int i=1;i<=n;i++)cin>>w[i],sum+=w[i];

    f[0][0]=true;

    for(int i=1;i<=n;i++)
    {
        for(int j=0;j<=sum;j++)
        {
            f[i][j]=f[i-1][j]||f[i-1][min(sum,w[i]+j)]||f[i-1][max(0,abs(j-w[i]))];
        }
    }

    for(int i=1;i<=sum;i++)ans+=f[n][i];

    cout<<ans<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 110, M = 1e5 + 10;

int n, w[N];
int sum, ans;
bool f[N][M];

int main()
{
    cin >> n;

    for (int i = 1; i <= n; i++)cin >> w[i], sum += w[i];

    f[0][0] = true;

    for (int i = 1; i <= n; i++)
    {
        for (int j = 0; j <= sum; j++)
        {
            f[i][j] = f[i - 1][j] || f[i - 1][min(j + w[i], sum)] || f[i - 1][abs(j - w[i])];
        }
    }

    for (int i = 1; i <= sum; i++)ans += f[n][i];

    cout << ans << endl;

    return 0;
}

```





[Problem - G - Codeforces](https://codeforces.com/contest/1955/problem/G)



给定一个数组a，

要求从a[1][1]出发走到a[n][m]使得路径上所有数的gcd最大，

输出最大的gcd



分析：



`n,m<=100`，`a[i][j]<=1e6`，`1e7`，

枚举所有a[1][1]的因子进行循环，用dp判断当前因子是否合法，

`f[i][j]`表示因子为x时是否能到达`a[i][j]`，更新ans



```cpp
bool check(int x)
{
    for(int i=1;i<=n;i++)for(int j=1;j<=m;j++)f[i][j]=false;

    f[1][1]=true;

    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            if(i==1&&j==1)continue;

            if((f[i][j-1]||f[i-1][j])&&a[i][j]%x==0)f[i][j]=true;// 1. 能被转移 2. 满足gcd==x
        }
    }

    return f[n][m];  // 判断在gcd==x的情况下是否能走到a[n][m]
}

void solve()
{
    cin>>n>>m;

    for(int i=1;i<=n;i++)for(int i=1;i<=m;i++)cin>>a[i][j];

    for(int i=1;i*i<=a[1][1];i++)
    {
        if(a[1][1]%i==0)
        {
            if(check(i))ans=max(ans,i);
            if(check(a[1][1]/i))ans=max(ans,a[1][1]/i);
        }
    }

    cout<<ans<<endl;
}
```



```cpp
#include <bits/stdc++.h>
#include <functional>

#define alls(a) a.begin(),a.end()
#define emb emplace_back
#define pub push_back
#define pob pop_back
#define puf push_front
#define pof pop_front
#define fi first
#define se second
#define No puts("No")
#define Yes puts("Yes")
#define NO puts("NO")
#define YES puts("YES")

using namespace std;
typedef long long ll;
//typedef __int128 lll; // G++(32位)不支持
typedef unsigned long long ull;
typedef pair<int, int> pii;

const int N = 110;
const int mo = 1e9 + 7;
const int inf = 2e9 + 10;

int ans;
int n, m;
int a[N][N];
bool f[N][N];

bool check(int x)
{
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            f[i][j] = false;
        }
    }

    f[1][1] = true;

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            if (i == 1 && j == 1)continue;

            if ((f[i - 1][j] || f[i][j - 1]) && a[i][j] % x == 0)f[i][j] = true;
        }
    }

    return f[n][m];
}

void solve()
{
    cin >> n >> m;

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            cin >> a[i][j];
        }
    }

    ans = 1;

    for (int i = 1; i * i <= a[1][1]; i++)
    {
        if (a[1][1] % i == 0)
        {
            if (check(i))ans = max(ans, i);
            if (check(a[1][1] / i))ans = max(ans, a[1][1] / i);
        }
    }

    cout << ans << endl;
}

int main()
{
    int t;

    cin >> t;

    //t = 1;

    while (t--)solve();

    return 0;
}
```
