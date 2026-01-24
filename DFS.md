[L3-001 凑零钱 - 团体程序设计天梯赛-练习集 (pintia.cn)](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805054207279104&page=1)



凑和m，求最小字典序



分析：



每个入口可以进入一个数字，

一共$10^4$个入口，每个入口都能往后最多递归100层

$(10^4)^{100}=10^{400}$



sort一遍，dfs找最小字典序



剪枝：

1. `cur+a[i]<=m   // 往后凑能拼出m的`

2. `if(cur+sum[tem]<m)return;   // 后面数字全部加上也不满足条件`
   
   优化到$10^4$，相当于最多只看100个数，$100*100$



```cpp
void dfs(int cur,int tem)
{
    if(!ans.empty())return;    // 找到答案

    if(cur==m){
        ans=v; return;
    }

    if(tem==n+1)return;        // 防止越界

    if(cur+s[tem]<m)return;    // 把后面的都加上都没可能凑到m

    for(int i=tem;i<=n;i++)
    {
        if(cur+a[i]<=m)
        {
            dfs(cur+a[i],i+1);
        }
    }
}

void solve()
{
    cin>>n>>m;

    for(int i=1;i<=n;i++)cin>>a[i];

    sort(a+1,a+n+1);

    for(int i=n;i>=1;i--)s[i]=s[i+1]+a[i]; // 求后缀和，统计加上后面的数有没有可能凑到m

    dfs(0,1);

    if(ans.empty())puts("No Solution!");
    else 
    {
        for(int i=0;i<ans.size();i++)
        {
            cout<<ans[i];
    
            if(i==ans.size()-1)break;

            cout<<' ';
        }
    }
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

const int N = 1e4 + 10;

int n;
int m;
int a[N];
ll s[N];
vector<int>ans, v;

void dfs(int cur,int tem)
{
	if (!ans.empty() || cur + s[tem] < m)return;	// 剪枝

	if (cur == m)
	{
		if (ans.empty())ans = v;

		return;
	}

	if (tem == n + 1)return;

	for (int i = tem; i <= n; i++)
	{
		if (a[i] + cur <= m)
		{
			v.push_back(a[i]);

			dfs(a[i] + cur, i+1);

			v.pop_back();
		}
	}
}

int main()
{
	cin >> n >> m;

	for (int i = 1; i <= n; i++)cin >> a[i];

	sort(a + 1, a + n + 1);

	for (int i = n; i >= 1; i--)s[i] = s[i + 1] + a[i];

	dfs(0, 1);

	if (ans.empty())puts("No Solution");
	else
	{
		for (int i = 0; i < ans.size(); i++)
		{
			cout << ans[i];

			if (i == ans.size() - 1)break;

			cout << ' ';
		}
	}

	return 0;
}
```







[L2-052 吉利矩阵 - 团体程序设计天梯赛-练习集 (pintia.cn)](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1781658570803388427&page=1)



给定L和n，问$n\times n$的矩阵，每个元素都是非负整数，

各行各列之和都是L，问有多少种方案



$2\le L\le 9,2\le n \le 4$



分析：



w[i]记录第i行所有数之和，

r[i]记录第i列所有数之和，

将数组映射到从0开始的一维，0~n-1



满足条件：



1. w[i]和r[j]均$\le L$
   `w[x/n]+i <= l && r[x%n]+i <= l`
   
   

2. 有可能凑到$L$
   将还剩的行和列加上有可能$\ge L$
   `w[x/n]+(n-1-x%n)*l >=l && r[x%r]+(n-1-x/n)*l >= l`
   
   

```cpp
void dfs(int x) // 当前下标为x
{
    if(x==n*n)
    {
        for(int i=0;i<n;i++)
        {
            if(w[i]!=l||r[i]!=l)return;
        }

        ans++;

        return;
    }

    for(int i=0;i<=l;i++)
    {
        if(w[x/n]+i>l||r[x%n]+i>l)break;

        w[x/n]+=i;
        r[x%n]+=i;

        if(w[x/n]+(n-1-x%n)*i>=l&&r[x%n]+(n-1-x/n)*i>=l)
        {
            dfs(x+1);
        }

        w[x/n]-=i;
        r[x%n]-=i;
    }
}

void solve()
{
    cin>>l>>n;

    dfs(0);

    cout<<ans<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 10;

int l, n;
int ans;
int w[N], r[N];

// w[i]：第i行总和，当前行，所有列元素的总和
// r[i]：第i列总和，当前列，所有行元素的总和

void dfs(int x)
{
    if (x == n * n)
    {
        for (int i = 0; i < n; i++)
        {
            if (w[i] != l || r[i] != l)return;
        }

        ans++;

        return;
    }

    for (int i = 0; i <= l; i++)
    {
        if (w[x / n] + i > l || r[x % n] + i > l)break; // 1. 加上当前数字会溢出

        w[x / n] += i, r[x % n] += i;

        if (w[x / n] + l * (n - 1 - x % n) >= l 
            && r[x % n] + l * (n - 1 - x / n) >= l)    // 2. 可能凑到l
        {
            dfs(x + 1);
        }

        w[x / n] -= i, r[x % n] -= i;    // 还原
    }
}

int main()
{
    cin >> l >> n;

    dfs(0);

    cout << ans << endl;

    return 0;
}
```





[0玩具蛇 - 蓝桥云课 (lanqiao.cn)](https://www.lanqiao.cn/problems/1022/learning/)



分析：



dfs(int x,int y,int cur)  // 当前枚举到位置(x,y)，(x,y)坐标对应序号为cur



```cpp
const int dx[]={1,-1,0,0};
const int dy[]={0,0,1,-1};

void dfs(int x,int y,int cur)
{
    if(cur>=16){ans++; return;}

    int xx,yy;

    for(int i=0;i<4;i++)
    {
        xx=x+dx[i],yy=y+dy[i];

        if(xx>=1&&xx<=4&&yy>=1&&yy<=4&&!st[xx][yy])
        {
            st[xx][yy]=true; dfs(xx,yy,cur+1); st[xx][yy]=false;
        }
    }
}

void solve()
{
    for(int i=1;i<=4;i++)
    {
        for(int j=1;j<=4;j++)
        {
            st[i][j]=true; dfs(i,j,1); st[i][j]=false;
        }
    }

    cout<<ans<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

const int dx[]={1,-1,0,0};
const int dy[]={0,0,1,-1};

const int N=10;

ll ans;
bool st[N][N];

// 当前遍历到(x,y)坐标，当前坐标为第cur个格子 

void dfs(int x,int y,int cur)
{
    if(cur>=16)
    {
        ans++; return;
    }

    int xx,yy;

    for(int i=0;i<4;i++)
    {
        xx=dx[i]+x,yy=dy[i]+y;

        if(xx>=1&&xx<=4&&yy>=1&&yy<=4&&st[xx][yy]==false)
        {
            st[xx][yy]=true;

            dfs(xx,yy,cur+1);

            st[xx][yy]=false;
        }
    }
}

int main()
{
    for(int i=1;i<=4;i++)
    {
        for(int j=1;j<=4;j++)
        {
            st[i][j]=true;

            dfs(i,j,1);

            st[i][j]=false;
        }
    }

    cout<<ans<<endl;

    return 0;
}
```





























[0最大数字 - 蓝桥云课 (lanqiao.cn)](https://www.lanqiao.cn/problems/2193/learning/)



求能获得的最大数字



分析：



位数不超过20，dfs枚举操作



当前位为9，不需要任何操作，直接操作下一位

否则，如果有a，就给当前位加，dfs，

如果能用b变成9，就变成9，dfs



当前已经遍历到最后一位就更新ans



```cpp
void dfs(int cnt,ll cur)  // 当前已经遍历了cnt位，当前值为cur
{
    if(cnt==len)
    {
        ans=max(ans,cur);

        return;
    }

    if(s[cnt]=='9')
    {
        dfs(cnt+1,cur*10+9); return;
    }

    int tem=min(a,'9'-s[cnt]);

    a-=tem;

    dfs(cnt+1,cur*10+tem+s[cnt]-'0');

    a+=tem;

    if(b>=s[cnt]-'0'+1)
    {
        b-=s[cnt]-'0'+1;

        dfs(cnt+1,cur*10+9);
    }
}

void solve()
{
    cin>>s>>a>>b;

    ans=atoi(s.c_str());

    len=s.size();

    dfs(0,0);

    cout<<ans<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

int len;
int a, b;
ll ans;
string s;

void dfs(int cnt, ll cur)  // 已经遍历cnt个数字，当前值为cur
{
    if (cnt >= len)
    {
        ans = max(ans, cur);

        return;
    }

    if (s[cnt] == '9')
    {
        dfs(cnt + 1, cur * 10 + 9);
    }

    int tem = min(a, '9' - s[cnt]);

    a -= tem;

    dfs(cnt + 1, cur * 10 + (s[cnt] + tem - '0'));

    a += tem;

    tem = s[cnt] - '0' + 1;

    if (tem <= b)
    {
        b -= tem;

        dfs(cnt + 1, cur * 10 + 9);

        b += tem;
    }
}

int main()
{
    cin >> s >> a >> b;

    ans = atoi(s.c_str());

    len = s.size();

    dfs(0, 0);

    cout << ans << endl;

    return 0;
}
```





[D - Grid and Magnet (atcoder.jp)](https://atcoder.jp/contests/abc351/tasks/abc351_d)



'#'是磁铁，'.'是空地，行走时在磁铁周围会被吸走，

问从任一空地出发，最多能走多少空地



分析：



用id标记连通块，遇到一块没被搜过的空地就搜索这个连通块



```cpp
const int dx[]={1,-1,0,0};
const int dy[]={0,0,1,-1};

bool check(int x,int y)
{
    int xx,yy;

    for(int i=0;i<4;i++)
    {
        xx=x+dx[i],yy=y+dy[i];

        if(xx>=1&&xx<=n&&yy>=1&&yy<=m&&g[xx][yy]=='#')return false;
    }

    return true;
}

void dfs(int x,int y,int id)
{
    cur++;

    st[x][y]=id;  // 将当前合法点加入连通块

    if(!check(x,y))return;

    // check过g[xx][yy]!='#'

    int xx,yy;

    for(int i=0;i<4;i++)
    {
        xx=dx[i]+x,yy=dy[i]+y;

        // 1. 满足坐标要求 2. 没有被当前连通块计算过

        if(xx>=1&&xx<=n&&yy>=1&&yy<=m&&st[xx][yy]!=st[x][y])
        {
            dfs(xx,yy,id);
        }
    }
}

void solve()
{
    cin>>n>>m;

    for(int i=1;i<=n;i++)for(int j=1;j<=m;j++)cin>>g[i][j];

    int id=0;

    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            if(g[i][j]=='.'&&!st[i][j])  // 没有被计算过连通块中
            {
                cur=0;

                dfs(i,j,++id);

                ans=max(ans,cur);
            }
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

const int N = 1e3 + 10;
const int mo = 1e9 + 7;
const int inf = 2e9 + 10;

const int dx[] = { 1,-1,0,0 };
const int dy[] = { 0,0,1,-1 };

int h, w;
int cur, idx, ans;
char g[N][N];
int st[N][N];

bool check(int x, int y)
{
    int xx, yy;

    for (int i = 0; i < 4; i++)
    {
        xx = x + dx[i], yy = y + dy[i];

        if (xx >= 1 && xx <= h && yy >= 1 && yy <= w)
        {
            if (g[xx][yy] == '#')return false;
        }
    }

    return true;
}

void dfs(int x, int y, int id)
{
    cur++;

    st[x][y] = id;

    if (!check(x, y))return;  // 周围有磁铁不能走

    int xx, yy;

    for (int i = 0; i < 4; i++)
    {
        xx = dx[i] + x, yy = dy[i] + y;

        // 标号和当前不同，没搜过

        if (xx >= 1 && xx <= h && yy >= 1 && yy <= w && st[xx][yy] != st[x][y])
        {
            if (g[xx][yy] != '#')dfs(xx, yy, id);
        }
    }
}

void solve()
{
    cin >> h >> w;

    for (int i = 1; i <= h; i++)
    {
        for (int j = 1; j <= w; j++)
        {
            cin >> g[i][j];
        }
    }

    for (int i = 1; i <= h; i++)
    {
        for (int j = 1; j <= w; j++)
        {
            if (g[i][j] == '.' && !st[i][j])  // 1. 为空 2. 没有被搜过
            {
                cur = 0;

                dfs(i, j, ++idx);  // 打标记

                ans = max(ans, cur);
            }
        }
    }

    cout << ans << endl;
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





[L2-026 小字辈 - 团体程序设计天梯赛-练习集 (pintia.cn)](https://pintia.cn/problem-sets/994805046380707840/exam/problems/994805055679479808?type=7&page=1)



分析：



用dep数组记录当前节点的辈分，maxx更新一下，

最后查找辈分==maxx的放入答案中



```cpp
void dfs(int u,int cur)
{
    for(auto v:e[u])
    {
        dep[v]=cur+1;

        maxx=max(maxx,cur+1);

        dfs(v,cur+1);
    }
}

void solve()
{
    cin>>n;

    for(int i=1;i<=n;i++)
    {
        cin>>u;

        if(u==-1)a.push_back(i);
        else e[u].push_back(i);  // 存孩子，一层层往下找
    }

    maxx=1;

    for(auto u:a)
    {
        dep[u]=1;

        dfs(u,1);
    }

    cout<<maxx<<endl;

    for(int i=1;i<=n;i++)if(dep[i]==maxx)ans.push_back(i);

    sort(alls(ans));

    for(int i=0;i<ans.size()-1;i++)cout<<ans[i]<<' ';

    cout<<ans.back();
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;

int n, x;
int maxx;
int cnt;
map<int, int>mp;  // pa son
vector<int>son[N];
vector<int>pa;
int dep[N];

void dfs(int id, int cur)
{
    maxx = max(maxx, cur);

    for (auto u : son[id])
    {
        dep[u] = max(dep[u], cur + 1);

        dfs(u, cur + 1);
    }
}

int main()
{
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        cin >> x;

        mp[i] = x;

        if (x == -1)pa.push_back(i);
        else
        {
            son[x].push_back(i);
        }
    }

    for (auto u : pa)
    {
        dep[u] = 1;

        dfs(u, 1);
    }

    cout << maxx << endl;

    vector<int>ans;

    for (int i = 1; i <= n; i++)
    {
        if (dep[i] == maxx)ans.push_back(i);
    }

    for (int i = 0; i < ans.size() - 1; i++)
    {
        printf("%d ", ans[i]);
    }

    printf("%d", ans.back());

    return 0;
}
```





[L2-020 功夫传人 - 团体程序设计天梯赛-练习集 (pintia.cn)](https://pintia.cn/problem-sets/994805046380707840/exam/problems/994805059118809088?type=7&page=1)



分析：



st数组记录当前人是否得道，

`dfs(int u,double cur)`，当前是哪个节点，传到当前一代还剩多少



```cpp
void dfs(int cur,double p)
{
    if(st[cur])
    {
        sum+=p*v[cur][0];

        return;
    }

    for(auto u:e[cur])
    {    
        dfs(u,p*(1-1.0*r/100));
    }
}

void solve()
{
    cin>>n>>z>>r;

    for(int i=0;i<n;i++)
    {
        cin>>k;

        if(k==0)
        {
            st[i]=true;

            cin>>cnt;

            v[i].push_back(cnt);
        }
        else
        {
            for(int j=0;j<k;j++)cin>>x,v[i].push_back(x);
        }
    }

    dfs(0,z);

    cout<<(long long)sum<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;

int n, k, x, cnt;
bool st[N];
vector<int>a[N];
double z, r, sum;

void dfs(int id, double p)
{
    if (st[id])
    {
        sum += p * a[id][0];
    }
    else
    {
        for (auto u : a[id])
        {
            dfs(u, p * (1 - 1.0 * r / 100));
        }
    }
}

int main()
{
    cin >> n >> z >> r;

    for (int i = 0; i < n; i++)
    {
        cin >> k;

        if (k == 0)
        {
            cin >> cnt;

            a[i].push_back(cnt);

            st[i] = true;
        }
        else
        {
            for (int j = 1; j <= k; j++)
            {
                cin >> x;

                a[i].push_back(x);
            }
        }
    }

    dfs(0, z);

    cout << (long long)sum;

    return 0;
}
```





[L2-016 愿天下有情人都是失散多年的兄妹 - 团体程序设计天梯赛-练习集 (pintia.cn)](https://pintia.cn/problem-sets/994805046380707840/exam/problems/994805061769609216?type=7&page=1)



给出多次询问，问两个id是否能结婚



分析：



用e数组存放当前节点的双亲，

每次a都往上走，标记4层以内的祖先，再走b观察是否有祖先重合，如果有就不能结婚



```cpp
void dfs(int u,int cur)
{
    if(cur>=4)return;  // 已经搜到高祖，返回

    for(auto v:e[u])
    {
        if(st[v]){f=true;return;} // 重复遍历祖宗节点，return
        else {st[v]; dfs(v,cur+1);}
    }
}

void solve()
{
    cin>>n;

    for(int i=1;i<=n;i++)
    {
        cin>>id>>c>>fa>>ma;

        sex[id]=c;

        if(fa!=-1){sex[fa]='M'; e[id].push_back(fa);}  // 通过子节点往上走搜索父节点

        if(ma!=-1){sex[ma]='F'; e[id].push_back(ma);}
    }

    cin>>m;

    for(int i=1;i<=m;i++)
    {
        cin>>a>>b;

        if(sex[a]==sex[b])puts("Never Mind");
        else 
        {
            f=false;

            memset(st,0,sizeof(st));  // 初始化访问数组

            dfs(a,0); dfs(b,0);  // 看有无祖宗节点重复访问

            if(!f)puts("Yes"); else puts("No");
        }
    }
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;

int n, k;
int a, b;
int id, fa, ma;
char c;
vector<int>e[N];
bool st[N];
char sex[N];
bool f;

void dfs(int x, int cur)
{
    if (cur >= 4)return;

    for (auto u : e[x])
    {
        if (!st[u])
        {
            st[u] = true;

            dfs(u, cur + 1);
        }
        else
        {
            f = true;

            return;
        }
    }
}

int main()
{
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        cin >> id >> c >> fa >> ma;

        sex[id] = c;

        if (fa != -1)
        {
            e[id].push_back(fa);

            sex[fa] = 'M';
        }

        if (ma != -1)
        {
            e[id].push_back(ma);

            sex[ma] = 'F';
        }
    }

    cin >> k;

    for (int i = 1; i <= k; i++)
    {
        cin >> a >> b;

        if (sex[a] == sex[b])printf("Never Mind");
        else
        {
            memset(st, 0, sizeof(st));

            st[a] = st[b] = true;

            f = false;

            dfs(a, 0);
            dfs(b, 0);

            if (f)printf("No");
            else printf("Yes");
        }

        if (i == k)break;

        puts("");
    }

    return 0;
}
```





[P2080 - [蓝桥杯2023初赛] 飞机降落 - New Online Judge (ecustacm.cn)](http://oj.ecustacm.cn/problem.php?id=2080)



分析：



飞机数量不超过10架，

$10!=10\times 9\times8\cdots6\times 5!=10\times 9\times8\cdots6\times120\approx10^7$



深搜，time记录上一架飞机最早降落结束的时间，u记录搜索深度

`a[i].t+a[i].d`是当前飞机最晚开始降落的时间，上一架飞机必须赶得上当前飞机最晚开始降落的时间，

`a[i].t+a[i].d>=time`



```cpp
bool dfs(int u,int time)
{
    if(u>n)return true;

    for(int i=1;i<=n;i++)
    {
        if(!st[i])
        {
            if(a[i].t+a[i].d>=time)  // 上一架飞机到达的时间正好能赶上它最晚开始起飞的时间
            {
                st[i]=true; dfs(u+1,max(time,a[i].t)+a[i].l);  // 尽可能早赶上
                st[i]=false;
            }
        }        
    }

    return false;    
}

void solve()
{
    cin>>n;

    for(int i=1;i<=n;i++)cin>>a[i].t>>a[i].d>>a[i].l;

    if(dfs(1,0))YES; else NO;
}
```



```cpp
#include <bits/stdc++.h>

#define YES puts("YES")
#define NO puts("NO")

using namespace std;

const int N = 20;

struct node
{
    int t, d, l;
}a[N];

int n;
bool st[N];

bool dfs(int u, int time)
{
    if (u > n)return true;

    for (int i = 1; i <= n; i++)
    {
        if (!st[i])
        {
            if (a[i].t + a[i].d >= time)
            {
                st[i] = true;

                if (dfs(u + 1, max(a[i].t, time) + a[i].l))return true;

                st[i] = false;
            }
        }
    }

    return false;
}

void solve()
{
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        cin >> a[i].t >> a[i].d >> a[i].l;
    }

    if (dfs(1, 0))YES;
    else NO;

    for (int i = 1; i <= n; i++)st[i] = false;
}

int main()
{
    int t;

    cin >> t;

    while (t--)solve();

    return 0;
}
```





[AtCoder Beginner Contest 347 - AtCoder](https://atcoder.jp/contests/abc347)



分析：



用两倍长度扩展数组记录旋转过后的瓦片数据，

st数组记录当前瓦片是否被使用过，

vis数组记录当前点是否被覆盖，

从左往右覆盖，当前行覆盖完后继续覆盖下一行



```cpp
void dfs(int x,int y)
{
    if(x>h)
    {
        Yes; 
        exit(0);
    }

    if(y>w)dfs(x+1,1);  // 当前行被访问过了

    if(vis[x][y])dfs(x,y+1);  // 当前位置被访问过了，从左往右贴瓷砖

    bool f=true;

    for(int i=1;i<=2*n;i++)
    {    
        if(st[((i-1)%n)+1])continue;

        f=true;

        for(int xx=x;xx<x+a[i];xx++)
        {    
            for(int yy=y;yy<y+b[i];yy++)
            {
                if(xx>h||yy>w||vis[xx][yy])f=false;
            }
        }

        if(!f)continue;

        st[((i-1)%n)+1]=true;

        for(int xx=x;xx<x+a[i];xx++)
        {
            for(int yy=y;yy<y+b[i];yy++)
            {    
                vis[xx][yy]=true;
            }
        }

        dfs(x,y+1);

        st[((i-1)%n)+1]=false;

        for(int xx=x;xx<x+a[i];xx++)
        {
            for(int yy=y;yy<y+b[i];yy++)
            {    
                vis[xx][yy]=false;
            }
        }
    }
}

void solve()
{
    cin>>n>>h>>w;

    for(int i=1;i<=n;i++)
    {
        cin>>a[i]>>b[i];

        a[i+n]=b[i],b[i+n]=a[i];
    }

    dfs(1,1);

    No;
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

const int N = 30;
const int mo = 1e9 + 7;
const int inf = 2e9 + 10;

int n, h, w;
int a[N], b[N];
bool vis[N][N], st[N];

void dfs(int x, int y)
{
    if (y > w)dfs(x + 1, 1);  // 这一行贴满了去下一行
    if (x > h)
    {
        Yes;

        exit(0);
    }

    if (vis[x][y])dfs(x, y + 1);  // 从左往右贴

    bool f = true;

    for (int i = 1; i <= 2 * n; i++)
    {
        if (!st[(i - 1) % n + 1])  // 这一块没被用过
        {
            f = true;

            for (int xx = x; xx < x + a[i]; xx++)
            {
                for (int yy = y; yy < y + b[i]; yy++)
                {
                    if (xx > h || yy > w || vis[xx][yy])  // 1. 贴的时候超出范围  2. 已经被贴过
                    {
                        f = false;
                    }
                }
            }

            if (!f)continue;

            st[(i - 1) % n + 1] = true;  // 贴瓷砖，一块只能用一次

            for (int xx = x; xx < x + a[i]; xx++)
            {
                for (int yy = y; yy < y + b[i]; yy++)
                {
                    vis[xx][yy] = true;
                }
            }

            dfs(x, y + 1);

            st[(i - 1) % n + 1] = false;

            for (int xx = x; xx < x + a[i]; xx++)
            {
                for (int yy = y; yy < y + b[i]; yy++)
                {
                    vis[xx][yy] = false;
                }
            }
        }
    }
}

void solve()
{
    cin >> n >> h >> w;

    for (int i = 1; i <= n; i++)
    {
        cin >> a[i] >> b[i];

        b[i + n] = a[i], a[i + n] = b[i];  // 旋转
    }

    dfs(1, 1);

    No;
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





[C-红魔馆的馆主_牛客周赛 Round 37 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/77231/C)



分析：



根据同余定理，先将上一位的余数处理出来，尝试向后接数

0~495最多不会超过三位



```cpp
void dfs(int mod,string cur)
{
    if(cur.size()==ans.size())return;

    if(mod==0)
    {
        if(cur.size()<ans.size())ans=cur; 

        return;
    }

    for(int i=0;i<=9;i++)
    {
        dfs((mo*10+i)%495,cur+char('0'+i));
    }
}

void solve()
{
    cin>>n;

    if(n%495==0)puts("-1");
    else
    {
        ans=string(100,'1');

        dfs(n%495,"");  // 传入一个空字符串，向后接找答案

        cout<<ans<<endl;
    }
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
const int inf = 2e9 + 10;

ll x;
string ans;

void dfs(int mod, string cur)
{
    if (mod == 0)
    {
        if (cur.size() < ans.size())ans = cur;

        return;
    }

    if (cur.size() >= ans.size())return;

    for (int i = 0; i <= 9; i++)
    {
        dfs((mod * 10 + i) % 495, cur + char('0' + i));
    }
}

void solve()
{
    cin >> x;

    ans = "1000";

    x %= 495;

    if (x == 0)puts("-1");
    else
    {
        dfs(x, "");

        cout << ans << endl;
    }
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
