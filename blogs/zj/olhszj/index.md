# 欧拉函数总结
## 定义
欧拉函数，即 $\varphi(n)$，表示小于等于 $n$ 的数中与 $n$ 互质的数的个数

## 性质
### 性质一
当 $n$ 为质数时，
$$\varphi(n)=n-1$$
 
证明：过于显然。

### 性质二
当 $n=p^k$ 时，
$$\varphi(n)=(p-1)p^{(k-1)}$$

证明：

$\varphi(n)\\$
$=\varphi(p^k)\\$
$=p^k-\frac{p^k}{p}\\$
$=p^k-p^{(k-1)}\\$
$=(p-1)p^{(k-1)}$

### 性质三
欧拉函数是积型函数，即对于任意互质的 $a$ 和 $b$，都有
$$\varphi(ab)=\varphi(a) \times \varphi(b)$$

证明：我不会，日后再证。

### 性质四
设 $n=p_{1}^{k_1}p_{2}^{k_2}...p_{n}^{k_n}$，则
$$\varphi(n)=n(1-\frac{1}{p_1})(1-\frac{1}{p_2})...(1-\frac{1}{p_k})$$

证明：

由 $\varphi$ 为积性函数及性质二，得：

$\varphi(n)\\$
$=\varphi(p_{1}^{k_1})\varphi(p_{2}^{k_2})...\varphi(p_{n}^{k_n})\\\ \\$
$=(p_1-1)p_{1}^{(k_1-1)}(p_2-1)p_{2}^{(k_2-1)}...(p_n-1)p_{n}^{(k_n-1)}\\\ \\$
$=\frac{(p_1-1)}{p_1}p_{1}^{k_1}\frac{(p_2-1)}{p_2}p_{2}^{k_2}...\frac{(p_n-1)}{p_n}p_{n}^{k_n}\\\ \\$
$=n(1-\frac{1}{p_1})(1-\frac{1}{p_2})...(1-\frac{1}{p_k})$

### 性质五

设 $n$ 为一个大于 $2$ 的整数，则所有与互质的数的和为
$$\frac{\varphi(n)}{2 }\times n$$
且 $\varphi(n)$ 为偶数

证明：我懒，以后再写。

### 性质六
$$\sum_{d \mid n}\varphi(d)=n$$
证明：

设 $f(i,n)$ 表示 $1\sim n$ 中与 $n$ 的最大公因数为 $i$ 的数的个数,则显然：
$$  f(1,n)+f(2,n)+f(3,n)+...+f(n,n)=n$$
若 $i \nmid n$，则 $f(i,n)$ 显然为 $0$。

否则相当与求：
$$\sum_{j=1}^{n}[\gcd(j,n)=i]$$
化简后得：
$$\sum_{j=1}^{\frac{n}{i}}[\gcd(j,n)=1]$$
即 $\varphi(\frac{n}{i})$

同理 $\varphi(i)$ 也会加进去

故原式成立
### 性质七
若 $p \mid n$ 且 $p^2 \mid n$ 则 $\varphi(n)=\varphi(\frac{n}{p}) \times p$

若 $p \mid n$ 且 $p^2 \nmid n$ 则 $\varphi(n)=\varphi(\frac{n}{p}) \times (p-1)$

证明：

设 $n=p^kp_{1}^{k_1}p_{2}^{k_2}...p_{n}^{k_n}$，则
 
$\varphi(n)=n(1-\frac{1}{p})(1-\frac{1}{p_1})(1-\frac{1}{p_2})...(1-\frac{1}{p_k})$

若 $p \mid n$ 且 $p^2 \mid n$(即 $k \geqslant 2$) 则

$\varphi(\frac{n}{p})=\frac{n}{p}(1-\frac{1}{p})(1-\frac{1}{p_1})(1-\frac{1}{p_2})...(1-\frac{1}{p_k})$

显然 $\varphi(n)=\varphi(\frac{n}{p}) \times p$

这是欧拉函数能被筛的重要性质
### 性质八
若 $a$ 与 $m$ 互质,则
$$a^{\varphi(m)}\equiv1(\bmod m)$$

证明：

设 $a_1,a_2,...a_{\varphi(m)}$ 为小于等于 $m$ 且与 $m$ 互质的数，因为 $a$ 与 $m$ 互质，所以 $aa_1,aa_2...aa_{\varphi(m)}$ 互不相同且与 $m$ 互质，所以 $aa_1,aa_2...aa_{\varphi(m)}$ 在 $\bmod m$ 意义下与 $a_1,a_2,...a_{\varphi(m)}$ 相等，所以

$$aa_1 \times aa_2...aa_{\varphi(m)} \equiv a_1a_2...a_{\varphi(m)}(\bmod m)$$
$$a^{\varphi(m)}(a_1a_2...a_{\varphi(m)}) \equiv a_1a_2...a_{\varphi(m)}(\bmod m)$$
所以
$$a^{\varphi(m)}\equiv1(\bmod m)$$

~~然后就结束了~~

## 求法
欧拉函数有两种求法，一种是用性质四求单个欧拉函数，一种是用性质七筛欧拉函数
### 单个欧拉函数
基于分解质因数
```cpp
long long phi(long long n)
{
	long long ph=n;
	for(int i=2;i*i<=n;i++)
	{
		if(n%i==0)//当i为n的因数时
		{
			ph=ph/i*(i-1);//性质四
			while(n%i==0) n/=i;
		}
	}
	if(n>1) ph=ph/n*(n-1);
	return ph;
}
```
### 筛欧拉函数
基于线性筛，顺便还能筛个质数
```cpp
long long phi[],prime[],idx;
bool isprime[];
void shai(long long n)
{
	phi[1]=1;
	for(int i=2;i<=n;i++)
	{
		if(!isprime[i])
		{
			prime[++idx]=i;
			phi[i]=i-1;
		}
		for(int j=1;j<=idx&&prime[j]<=n/i;j++)
		{
			isprime[p[j]*i]=true;
			if(i%prime[j]==0)//p^2|n
			{
				phi[i*prime[j]]=phi[i]*prime[j];
				break;
			}
			phi[i*p[j]]=phi[i]*(prime[j]-1);//p|n
		}
	}
}
```
<script src="https://giscus.app/client.js"
        data-repo="lyh227/lyh227.github.io"
        data-repo-id="R_kgDOLy9wWQ"
        data-category="Announcements"
        data-category-id="DIC_kwDOLy9wWc4CfrdI"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="zh-CN"
        crossorigin="anonymous"
        async>
</script>
<a href="../index.html"><button>上一页</button></a>