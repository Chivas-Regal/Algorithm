---
title: 并查集
---
###  
<hr>

## ABC238E_RangeSums

#### 🔗
<a href="https://atcoder.jp/contests/abc238/tasks/abc238_e"><img src="https://img-blog.csdnimg.cn/90eaebfac43c492ca3271d26b09bf540.png"></a>

#### 💡
考虑一下，在拥有 $x\to y$ 的情况下，拥有了 $y+1\to z$ ，我们会获得 $x\to z$   
如果是拥有了 $z+1\to y$ ，我们会获得 $x\to z$  
这是一个传递性的关系  
可以通过建图完成  
建立并查集，每次连通 $l-1\to r$ ，如果最后 $0$ 与 $n$ 连通，就可以传递过去  
反之不能   

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >
```cpp
const int N = 2e5 + 10;

namespace UnionSet {
        int nod[N];
        inline int Find ( int x ) { return x == nod[x] ? x : nod[x] = Find(nod[x]); }
        inline void Merge ( int x, int y ) {
                int fx = Find(x), fy = Find(y);
                if ( fx != fy ) nod[fx] = fy;
        }
}
int n, q;

int main () {
        ios::sync_with_stdio(false);
        cin >> n >> q;
        for ( int i = 0; i <= n; i ++ ) UnionSet::nod[i] = i;
        for ( int i = 0; i < q; i ++ ) {        
                int l, r; cin >> l >> r;
                UnionSet::Merge(l - 1, r);
        }        
        if ( UnionSet::Find(0) == UnionSet::Find(n) ) cout << "Yes" << endl;
        else cout << "No" << endl;
}
```
<hr>


## CodeForces1594D_TheNumberOfImposters

#### 🔗
<a href="https://codeforces.com/contest/1594/problem/D"><img src="https://i.loli.net/2021/10/09/OndVD6NqE1yFbIX.png"></a>

#### 💡
首先要从人物关系中下手  
如果一个人说另一个人是船员，那么两个要么都是船员要么都不是  
如果一个人说另一个人是冒牌，那么两个人中必定只有一个是冒牌  
  
我们要求得最大的冒牌数量，可以使用带权并查集  
就是两个人一定同一阵营`merge( x, y ), merge ( x + n, y + n )`  
一定不是同一阵营 `merge ( x, y + n ), merge ( x + n, y )`  
  
在特判的时候，如果一个人自己和自己不是同一阵营，就输出-1  
否则在遇到每个阵营的祖先的时候，从它直接代表的两个阵营中选出一个最大的即 `max(siz[Find(x)], siz[Find(x + n)])`  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >

```cpp
#include <iostream>
#include <map>
#include <vector>
#include <cstring>
#include <algorithm>

#define ll long long
using namespace std;
const int mod = 1e9 + 7;

inline ll ksm ( ll a, ll b ) {
	ll res = 1;
	while ( b ) {
		if ( b & 1 ) res = res * a % mod;
		a = a * a % mod;
		b >>= 1;
	}
	return res;
}

inline void Solve() {
	ll a, b; cin >> a >> b;
	ll res = 0;
	while ( b > 0 ) {
		ll bas = 0, sum = 0;
		while ( sum + (1ll << bas) < b ) {
			sum += (1ll << bas);
			bas ++;
		}
		res = (res + ksm(a, bas)) % mod;
		b -= (1ll << bas);
	}
	cout << res << endl;

}

int main () {
	ios::sync_with_stdio(false);
#ifndef ONLINE_JUDGE
        freopen("in.in", "r", stdin);
        freopen("out.out", "w", stdout);
#endif
	int cass; cin >> cass; while ( cass -- ) {
		Solve();
	}
        return 0;
}
```

<hr>


## HDUOJ2844_食物链

#### 🔗
http://acm.hdu.edu.cn/showproblem.php?pid=2844

#### 💡
分析到题中有三种集合关系  
  
我们在得到谁吃谁的时候无法对两者进行合并  
所以可以对a的被吃、b的吃、a的吃、b的吃进行合并  
即两种权值以n为边界开成两维  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >  
  

```cpp
#pragma region
#pragma GCC optimize(3,"Ofast","inline")
#include <algorithm>
#include <iostream>
#include <cstring>
#include <string>
#include <vector>
#include <cstdio>
#include <stack>
#include <queue>
#include <cmath>
#include <map>
#include <set>
#define G 10.0
#define LNF 1e18
#define eps 1e-6
#define PI acos(-1.0)
#define ll long long
#define INF 0x7FFFFFFF
#define Regal exit(0)
#define Chivas int main()
#define pb(x) push_back(x)
#define SP system("pause")
#define Max(a,b) ((a)>(b)?(a):(b))
#define Min(a,b) ((a)<(b)?(a):(b))
#define IOS ios::sync_with_stdio(false)
#define mm(a, b) memset(a, b, sizeof(a))
#define each_cass(cass) for (cin>>cass; cass; cass--)
#define test(a) cout << "---------" << a << "---------" << '\n'

using namespace std;
#pragma endregion

//全局变量
#pragma region
const int maxn = 155000;

int nod[maxn];
int n, k;
int cnt = 0;
#pragma endregion


//主体------------------------------------------------------------

inline int find(int x){
    return x == nod[x]? x:nod[x]=find(nod[x]);
}

inline void merge(int x, int y){
    int fx = find(x);
    int fy = find(y);
    if(fx != fy){
        nod[fx] = fy;
    }
}

inline void init(){
    for(int i = 0; i < maxn; i ++){
        nod[i] = i;
    }
}

Chivas{
    IOS;
    cin >> n >> k;
    init();
    for(int i = 0; i < k; i ++){
        int id, x, y;
        cin >> id >> x >> y;
        if(x > n || y > n || x <= 0 || y <= 0){
            ++cnt;
            continue;
        }
        int fx = find(x);
        int fy = find(y);
        if(id == 1){
            if(find(x) == find(y + n) || find(x) == find(y + 2 * n))  ++cnt;
            else  merge(x, y), merge(x + n, y + n), merge(x + 2 * n, y + 2 * n);
        }else{
            if(x == y){
                cnt ++;
                continue;
            }
            if(find(x) == find(y) || find(x) == find(y + 2 * n))     ++cnt;
            else  merge(x, y + n), merge(x + n, y + 2 * n), merge(x + 2 * n, y);
        }
    }
    cout << cnt << endl;
    Regal;
}
```

<hr>

## POJ1703_FindThem,CatchThem

#### 🔗
http://poj.org/problem?id=1703

#### 💡
与食物链一样
并查集不仅用在正面对象
也可以对背后的对象建立并查集
所以关键是在分析都对什么建立并查集

本题让建立两个集合（两个不同的帮派）
故可以对每个帮派的反派也建立
来维护相反派之间的关系
因为有时候得到某种关系的时候，无法进行两者合并，只能对a的反派和b合并，b的反派和a合并


#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >  
  

```cpp
#pragma region
#pragma GCC optimize(3,"Ofast","inline")
#include <algorithm>
#include <iostream>
#include <cstring>
#include <string>
#include <vector>
#include <cstdio>
#include <stack>
#include <queue>
#include <cmath>
#include <map>
#include <set>
#define G 10.0
#define LNF 1e18
#define eps 1e-6
#define PI acos(-1.0)
#define ll long long
#define INF 0x7FFFFFFF
#define Regal exit(0)
#define Chivas int main()
#define pb(x) push_back(x)
#define SP system("pause")
#define Max(a,b) ((a)>(b)?(a):(b))
#define Min(a,b) ((a)<(b)?(a):(b))
#define IOS ios::sync_with_stdio(false)
#define mm(a, b) memset(a, b, sizeof(a))
#define each_cass(cass) for (cin>>cass; cass; cass--)
#define test(a) cout << "---------" << a << "---------" << '\n'
 
using namespace std;
#pragma endregion

//全局变量
#pragma region
const int maxn = 2e5 + 10;
int nod[maxn];
int n, m;
#pragma endregion

//主体----------------------------------------------------------------------------------

inline void Init(){
    for(int i = 0; i <= n * 2; i ++) nod[i] = i;
}

inline int find(int x){
    return x == nod[x] ? x : nod[x] = find(nod[x]);
}

inline void merge(int x, int y){
    int fx = find(x);
    int fy = find(y);
    if(fx != fy) nod[fx] = fy;
}

inline void solve(){

    scanf("%d%d", &n, &m);
    Init();
    
    for(int i = 0; i < m; i ++){
        char op;
        int a, b;
        getchar();
        scanf("%c%d%d", &op, &a, &b);
        if(op == 'A'){
            int fa = find(a);
            int fb = find(b);
            int fa_n = find(a + n);
            int fb_n = find(b + n);
            if(fa_n == fb_n || fa == fb)
                cout << "In the same gang." << endl;
            else if(fa_n == fb || fa == fb_n)
                cout << "In different gangs." << endl;
            else   
                cout << "Not sure yet." << endl;
        }else{
            merge(a + n, b);
            merge(b + n, a);
        }
    }
}

Chivas{
    IOS;
    int cass;
    scanf("%d", &cass);
    while(cass--){
        solve();
    }
    Regal;
}
```

<hr>

## POJ1988_BuildingBlock

#### 🔗
http://poj.org/problem?id=1988

#### 💡
一个平平无奇的带权并查集  
首先我们需要固定出三个信息：  
1.x属于以y为底的堆，nod[x] = y  
2.以x为底的堆有y个块，pile[x] = y  
3.x块底下有y个块，down[x] = y  
  
在每一次向下找堆底的块的时候，我们需要在逆序中回溯出这个堆中每个块底下有几个块  
在每一次 x 向 y 合并的时候，我们都应该更新一下 x 下面的块数量、y这个堆的总块数、x属于的堆编号  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >

```cpp
/*
           ________   _                                              ________                              _
          /  ______| | |                                            |   __   |                            | |
         /  /        | |                                            |  |__|  |                            | |
         |  |        | |___    _   _   _   ___  _   _____           |     ___|   ______   _____   ___  _  | |
         |  |        |  __ \  |_| | | | | |  _\| | | ____|          |  |\  \    |  __  | |  _  | |  _\| | | |
         |  |        | |  \ |  _  | | | | | | \  | | \___           |  | \  \   | |_/ _| | |_| | | | \  | | |
         \  \______  | |  | | | | \ |_| / | |_/  |  ___/ |          |  |  \  \  |    /_   \__  | | |_/  | | |
Author :  \________| |_|  |_| |_|  \___/  |___/|_| |_____| _________|__|   \__\ |______|     | | |___/|_| |_|
                                                                                         ____| |
                                                                                         \_____/
*/

//#include <unordered_map>
#include <algorithm>
#include <iostream>
#include <cstring>
#include <utility>
#include <string>
#include <vector>
#include <cstdio>
#include <stack>
#include <queue>
#include <cmath>
#include <map>
#include <set>

#define G 10.0
#define LNF 1e18
#define EPS 1e-6
#define PI acos(-1.0)
#define INF 0x7FFFFFFF

#define ll long long
#define ull unsigned long long

#define LOWBIT(x) ((x) & (-x))
#define LOWBD(a, x) lower_bound(a.begin(), a.end(), x) - a.begin()
#define UPPBD(a, x) upper_bound(a.begin(), a.end(), x) - a.begin()
#define TEST(a) cout << "---------" << a << "---------" << endl

#define CHIVAS_ int main()
#define _REGAL exit(0)
#define SP system("pause")
#define IOS ios::sync_with_stdio(false)

//#define map unordered_map

#define PB(x) push_back(x)
#define ALL(a) a.begin(),a.end()
#define MEM(a, b) memset(a, b, sizeof(a))
#define EACH_CASE(cass) for (cass = inputInt(); cass; cass--)

#define LS l, mid, rt << 1
#define RS mid + 1, r, rt << 1 | 1
#define GETMID (l + r) >> 1

using namespace std;


template<typename T> inline T MAX(T a, T b){return a > b? a : b;}
template<typename T> inline T MIN(T a, T b){return a > b? b : a;}
template<typename T> inline void SWAP(T &a, T &b){T tp = a; a = b; b = tp;}
template<typename T> inline T GCD(T a, T b){return b > 0? GCD(b, a % b) : a;}
template<typename T> inline void ADD_TO_VEC_int(T &n, vector<T> &vec){vec.clear(); cin >> n; for(int i = 0; i < n; i ++){T x; cin >> x, vec.PB(x);}}
template<typename T> inline pair<T, T> MaxInVector_ll(vector<T> vec){T MaxVal = -LNF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal <vec[i]) MaxVal = vec[i], MaxId = i; return {MaxVal, MaxId};}
template<typename T> inline pair<T, T> MinInVector_ll(vector<T> vec){T MinVal = LNF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i])MinVal = vec[i], MinId = i; return {MinVal, MinId};}
template<typename T> inline pair<T, T> MaxInVector_int(vector<T> vec){T MaxVal = -INF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal <vec[i]) MaxVal = vec[i], MaxId = i; return {MaxVal, MaxId};}
template<typename T> inline pair<T, T> MinInVector_int(vector<T> vec){T MinVal = INF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i])MinVal = vec[i], MinId = i; return {MinVal, MinId};}
template<typename T> inline pair<map<T, T>, vector<T> > DIV(T n){T nn = n;map<T, T> cnt;vector<T> div;for(ll i = 2; i * i <= nn; i ++){while(n % i == 0){if(!cnt[i]) div.push_back(i);cnt[i] ++;n /= i;}}if(n != 1){if(!cnt[n]) div.push_back(n);cnt[n] ++;n /= n;}return {cnt, div};}
template<typename T> vector<T>& operator-- (vector<T> &v){for (auto& i : v) --i; return v;}
template<typename T> vector<T>& operator++ (vector<T> &v){for (auto& i : v) ++i; return v;}
template<typename T> istream& operator>>(istream& is, vector<T> &v){for (auto& i : v) is >> i; return is;}
template<typename T> ostream& operator<<(ostream& os, vector<T> v){for (auto& i : v) os << i << ' '; return os;}
inline int inputInt(){int X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1);}
inline void outInt(int X){if(X<0) {putchar('-'); X=~(X-1);}int s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}
inline ll inputLL(){ll X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1); }
inline void outLL(ll X){if(X<0) {putchar('-'); X=~(X-1);}ll s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}

const int N = 3e4 + 10;
int pile[N], down[N], nod[N];
int Q;

inline void Init ( ) { for ( int i = 0; i < N; i ++ ) nod[i] = i, down[i] = 0, pile[i] = 1; }
inline int Find ( int x ) {
        int fx = nod[x];
        if ( fx != x ) {
                nod[x] = Find(nod[x]); // 正常的搜祖先
                down[x] += down[fx]; // 用父亲回溯出儿子的下块数
        }return nod[x];
}
inline void Merge ( int x, int y ) {
        int fx = Find(x);
        int fy = Find(y);
        if ( fx != fy ) {
                nod[fx] = fy; // fx 整体编号改变
                down[fx] = pile[fy]; // fx 下面的块数多了 fy 这个堆的块数
                pile[fy] += pile[fx]; // fy 这个堆的块数多了 fx 这个堆的块数
        }
}

CHIVAS_{Init();
        Q = inputInt();
        while ( Q -- ) {
                char op; scanf("%c", &op);
                if ( op == 'C' ) {
                        int id = inputInt();
                        int k = Find(id); // 不能少，要更新一下当前块的down值
                        outInt(down[id]); puts("");
                } else {
                        int a = inputInt(), b = inputInt();
                        Merge(a, b);
                }
        }
        _REGAL;
}
```



<hr>
