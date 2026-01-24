[RC-u5 相对成功与相对失败 - 2023 睿抗机器人开发者大赛CAIP-编程技能赛-本科组（省赛） (pintia.cn)](https://pintia.cn/problem-sets/1680597991209951232/exam/problems/type/7?problemSetProblemId=1680598077168017412&page=0)



给出每个人参加睿抗和玩手机的信息，

以及每个人最终的成绩排名，

求最少有多少人在填写时说谎了



分析：



用权值表示当前该人的得分

> 权值：    1    0
> 
> rk：        1    0
> 
> 玩手机：0    1

得`a[i]=f1+(1-f2)`



求最少多少人填写时说谎，

越靠前权值越高，即排名从前往后一定不上升



用f数组求最长不上升子序列，n-maxx即为答案



```cpp
void solve()
{
    cin>>n;

    for(int i=1;i<=n;i++)
    {
        cin>>f1>>f2;

        a[i]=f1+1-f2;  // 得到当前权值
    }

    for(int i=0;i<3;i++)f[i]=0;

    maxx=0;
    
    for(int i=1;i<=n;i++)
    {
        cin>>x;

        for(int j=a[x];j<3;j++)
            f[a[x]]=max(f[a[x]],f[j]+1);  // 尝试在不上升序列后接上当前元素

        maxx=max(maxx,f[a[x]]);
    }

    cout<<n-maxx<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;

int n, x;
int maxx;
bool f1, f2;
int f[3];
int a[N];

void solve()
{
	cin >> n;

	for (int i = 1; i <= n; i++)
	{
		cin >> f1 >> f2;

		a[i] = f1 + (1 - f2);
	}

	for (int i = 0; i < 3; i++)f[i] = 0;

	maxx = 0;

	for (int i = 1; i <= n; i++)
	{
		cin >> x;

		for (int j = a[x]; j < 3; j++)
		{
			f[a[x]] = max(f[a[x]], f[j] + 1);
		}

		maxx = max(maxx, f[a[x]]);
	}

	cout << n - maxx << endl;
}

int main()
{
	int t;

	cin >> t;

	while (t--)solve();

	return 0;
}
```





[0游园安排 - 蓝桥云课 (lanqiao.cn)](https://www.lanqiao.cn/problems/1024/learning/)



按照给出的先后顺序对姓名进行拼接，要求单调递减，

求包含姓名个数且最小的序列



分析：



b数组存放当前序列，f数组存放当前长度为i的最优序列，

每次用lower_bound找到当前字符串能替代的更优位置，将更大的替代掉，保证字典序最小



```cpp
void solve()
{
    cin>>s;

    x=""; x+=s[0];

    for(int i=1;i<s.size();i++)
    {
        if(s[i]>='A'&&s[i]<='Z'){a.emplace_back(x); x=s[i];}
        else x+=s[i];
    }

    a.emplace_back(x);

    for(int i=0;i<a.size();i++)
    {
        idx=lower_bound(b+1,b+maxx+1,a[i])-b;

        maxx=max(maxx,idx);

        b[idx]=a[i];

        f[idx]=f[idx-1]+a[i];
    }

    cout<<f[maxx]<<endl;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N=1e6+10;

int maxx;
string s;
vector<string>a;

int main()
{
    cin>>s;

    string x="";

    x+=s[0];

    for(int i=1;i<s.size();i++)
    {
        if(s[i]>='A'&&s[i]<='Z')
        {
            a.emplace_back(x);

            x="";

            x+=s[i];
        }
        else x+=s[i];
    }

    if(a.back()!=x)a.emplace_back(x);  // 末尾字符串 

    string f[a.size()+1],b[a.size()+1];

    for(int i=0;i<=a.size();i++)f[i]="",b[i]="";

    int idx;

    for(int i=0;i<a.size();i++)
    {
        idx=lower_bound(b+1,b+maxx+1,a[i])-b;  // 有相等的直接取代 

        maxx=max(maxx,idx);

        b[idx]=a[i];

        f[idx]=f[idx-1]+a[i];
    }

    cout<<f[maxx];

    return 0;
}
```
