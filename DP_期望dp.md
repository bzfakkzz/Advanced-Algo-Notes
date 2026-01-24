[E - Toward 0 (atcoder.jp)](https://atcoder.jp/contests/abc350/tasks/abc350_e?lang=en)



求从N变为0的花费期望最小为多少



分析：



`f[i]`记录i变为0至少需要多少花费，`f[0]=0`



选择第一种：

`f[i]=f[i/a]+x`



选择第二种：

$f_i=\dfrac 1 6\sum_{j=1}^6f_{\frac{i}{j}}+y$

递归时，发现$j=1$时会循环递归调用$f_i$，

公式变形得：$f_i=\dfrac 1 5\sum_{j=2}^6f_{\frac{i}{j}}+\dfrac 6 5 y$



用记忆化搜索避免重复搜索



```cpp
long double dp(int x)
{
    if(x==0)return 0;

    if(f[x])return f[x]; // 记忆化

    long double ans1=dp(n/a)+x;

    long double ans2=0;

    for(int i=2;i<=6;i++)ans2+=dp(x/i)/5;

    ans2+=6.0/5*y;

    return f[n]=min(ans1,ans2);
}

void solve()
{
    cin>>n>>a>>x>>y;

    printf("%.10Lf",dp(n));
}
```


