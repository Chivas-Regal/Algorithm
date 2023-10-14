---
title: 数位DP
---
###  
<hr>

## HDUOJ2089_不要62

#### 🔗
https://acm.dingbacode.com/showproblem.php?pid=2089

#### 💡
本题让记录没有 “4” 和 “62” 的数  
“4”是可以直接在每一位上进行判断，“62”则需要加一个记录前面是否为6的is_6变量，在这个基础上判断2  
求区间内满足条件的数的个数，可以用前缀和的思想求区间和  

#### ✅

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
#define INT __int128

#define LOWBIT(x) ((x) & (-x))
#define LOWBD(a, x) lower_bound(a.begin(), a.end(), x) - a.begin()
#define UPPBD(a, x) upper_bound(a.begin(), a.end(), x) - a.begin()
#define TEST(a) cout << "---------" << a << "---------" << endl

#define CHIVAS_ int main()
#define _REGAL exit(0)

#define SP system("pause")
#define IOS ios::sync_with_stdio(false)
//#define map unordered_map

#define _int(a) int a; cin >> a
#define  _ll(a) ll a; cin >> a
#define _char(a) char a; cin >> a
#define _string(a) string a; cin >> a
#define _vectorInt(a, n) vector<int>a(n); cin >> a
#define _vectorLL(a, b) vector<ll>a(n); cin >> a

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
template<typename T> inline pair<T, T> MaxInVector_ll(vector<T> vec){T MaxVal = -LNF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return make_pair(MaxVal, MaxId);}
template<typename T> inline pair<T, T> MinInVector_ll(vector<T> vec){T MinVal = LNF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return make_pair(MinVal, MinId);}
template<typename T> inline pair<T, T> MaxInVector_int(vector<T> vec){T MaxVal = -INF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return make_pair(MaxVal, MaxId);}
template<typename T> inline pair<T, T> MinInVector_int(vector<T> vec){T MinVal = INF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return make_pair(MinVal, MinId);}
template<typename T> inline pair<map<T, T>, vector<T> > DIV(T n){T nn = n;map<T, T> cnt;vector<T> div;for(ll i = 2; i * i <= nn; i ++){while(n % i == 0){if(!cnt[i]) div.push_back(i);cnt[i] ++;n /= i;}}if(n != 1){if(!cnt[n]) div.push_back(n);cnt[n] ++;n /= n;}return make_pair(cnt, div);}

inline int inputInt(){int X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1);}
inline void outInt(int X){if(X<0) {putchar('-'); X=~(X-1);}int s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}
inline ll inputLL(){ll X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1); }
inline void outLL(ll X){if(X<0) {putchar('-'); X=~(X-1);}ll s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}

const int N = 100;
int num[N], dp[N][2];

inline int DFS ( int n, bool is_6, bool is_max ) { // 62是连数，不能直接判断，所以要加个上一位的记录
        if ( !n ) return 1;
        if ( !is_max && ~dp[n][is_6] ) return dp[n][is_6];

        ll res = 0, m = is_max ? num[n] : 9;
        for ( int i = 0; i <= m; i ++ ) {
                if ( !(is_6 && i == 2) && i != 4 ) res += DFS ( n - 1, i == 6, is_max && i == m );
        } 
        if ( !is_max ) dp[n][is_6] = res;
        return res;
}
inline int Solve ( int x ) {
        int len;
        for ( len = 0; x; x /= 10 ) num[ ++ len ] = x % 10;
        return DFS(len, 0, 1); 
}
CHIVAS_{
        MEM(dp, -1);
        int l, r;
        while ( scanf("%d%d", &l, &r) == 2 , l || r ) {
                outInt( Solve(r) - Solve(l - 1) ); puts(""); // 前缀和统计区间内满足条件的数的个数
        }
        _REGAL;
}
```

<hr>

## HDUOJ3555_Bomb

#### 🔗
https://acm.dingbacode.com/showproblem.php?pid=3555

           
#### 💡
我们可以利用数位DP把不含 "49" 的统计出来  
然后 x - solve(x) + 1 即是正解  
本题为类模板题  

#### ✅

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
#define INT __int128

#define LOWBIT(x) ((x) & (-x))
#define LOWBD(a, x) lower_bound(a.begin(), a.end(), x) - a.begin()
#define UPPBD(a, x) upper_bound(a.begin(), a.end(), x) - a.begin()
#define TEST(a) cout << "---------" << a << "---------" << endl

#define CHIVAS_ int main()
#define _REGAL exit(0)

#define SP system("pause")
#define IOS ios::sync_with_stdio(false)
//#define map unordered_map

#define _int(a) int a; cin >> a
#define  _ll(a) ll a; cin >> a
#define _char(a) char a; cin >> a
#define _string(a) string a; cin >> a
#define _vectorInt(a, n) vector<int>a(n); cin >> a
#define _vectorLL(a, b) vector<ll>a(n); cin >> a

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
template<typename T> inline pair<T, T> MaxInVector_ll(vector<T> vec){T MaxVal = -LNF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return make_pair(MaxVal, MaxId);}
template<typename T> inline pair<T, T> MinInVector_ll(vector<T> vec){T MinVal = LNF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return make_pair(MinVal, MinId);}
template<typename T> inline pair<T, T> MaxInVector_int(vector<T> vec){T MaxVal = -INF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return make_pair(MaxVal, MaxId);}
template<typename T> inline pair<T, T> MinInVector_int(vector<T> vec){T MinVal = INF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return make_pair(MinVal, MinId);}
template<typename T> inline pair<map<T, T>, vector<T> > DIV(T n){T nn = n;map<T, T> cnt;vector<T> div;for(ll i = 2; i * i <= nn; i ++){while(n % i == 0){if(!cnt[i]) div.push_back(i);cnt[i] ++;n /= i;}}if(n != 1){if(!cnt[n]) div.push_back(n);cnt[n] ++;n /= n;}return make_pair(cnt, div);}

inline int inputInt(){int X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1);}
inline void outInt(int X){if(X<0) {putchar('-'); X=~(X-1);}int s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}
inline ll inputLL(){ll X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1); }
inline void outLL(ll X){if(X<0) {putchar('-'); X=~(X-1);}ll s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}

const int N = 100;
ll dp[N][2], b[N];

inline ll DFS ( int n, bool is_4, bool is_max ) {
        if ( !n ) return 1;
        if ( !is_max && dp[n][is_4] != -1 ) return dp[n][is_4];

        ll res = 0;
        int m = is_max? b[n] : 9; 
        for ( int i = 0; i <= m; i ++ ) {
                // 把构不成 49 的统计出来
                if ( !(is_4 && i == 9) ) res += DFS ( n - 1, i == 4, is_max && i == m );
        }
        if ( !is_max ) dp[n][is_4] = res;
        return res;
}

inline ll solve ( ll x ) {
        int len;
        for ( len = 0; x; x /= 10 ) b[ ++ len ] = x % 10;
        return DFS ( len, 0, true );
}

CHIVAS_{
        int cass;
        MEM (dp, -1);
        EACH_CASE ( cass ) {
                ll x = inputLL();
                outLL(x - solve(x) + 1); puts("");
        }
        _REGAL;
}
```

<hr>

## HDUOJ4507_恨7不成妻

#### 🔗
https://acm.dingbacode.com/showproblem.php?pid=4507

           
#### 💡
本题对数的要求：  
1.各位不能出现7：在枚举位的时候把7跳过去即可  
2.各位和不能是7的倍数：参数列表里面设置sum，表示各个位的和模7的数值，若为0代表是7的倍数  
3.数不能是7的倍数：参数列表设一个num，表示自身模7的数值，若为0代表是7的倍数  
  
在求平方和时，我们可以使用平方和公式，即 :  
<img src="https://latex.codecogs.com/svg.image?23^2=(20&plus;3)^2=20^2&plus;3^2&plus;2*20*3" title="23^2=(20+3)^2=20^2+3^2+2*20*3" />  
<img src="https://latex.codecogs.com/svg.image?123^2=(100&plus;23)^2=100^2&plus;23^2&plus;2*100*23&space;" title="123^2=(100+23)^2=100^2+23^2+2*100*23 " />  
可以通过记录填法数量、填法数总和并记录后面几位的平方和回溯出整体平方和


#### ✅

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
#define INT __int128

#define LOWBIT(x) ((x) & (-x))
#define LOWBD(a, x) lower_bound(a.begin(), a.end(), x) - a.begin()
#define UPPBD(a, x) upper_bound(a.begin(), a.end(), x) - a.begin()
#define TEST(a) cout << "---------" << a << "---------" << endl

#define CHIVAS_ int main()
#define _REGAL exit(0)

#define SP system("pause")
#define IOS ios::sync_with_stdio(false)
//#define map unordered_map

#define _int(a) int a; cin >> a
#define  _ll(a) ll a; cin >> a
#define _char(a) char a; cin >> a
#define _string(a) string a; cin >> a
#define _vectorInt(a, n) vector<int>a(n); cin >> a
#define _vectorLL(a, b) vector<ll>a(n); cin >> a

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
template<typename T> inline pair<T, T> MaxInVector_ll(vector<T> vec){T MaxVal = -LNF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return make_pair(MaxVal, MaxId);}
template<typename T> inline pair<T, T> MinInVector_ll(vector<T> vec){T MinVal = LNF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return make_pair(MinVal, MinId);}
template<typename T> inline pair<T, T> MaxInVector_int(vector<T> vec){T MaxVal = -INF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return make_pair(MaxVal, MaxId);}
template<typename T> inline pair<T, T> MinInVector_int(vector<T> vec){T MinVal = INF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return make_pair(MinVal, MinId);}
template<typename T> inline pair<map<T, T>, vector<T> > DIV(T n){T nn = n;map<T, T> cnt;vector<T> div;for(ll i = 2; i * i <= nn; i ++){while(n % i == 0){if(!cnt[i]) div.push_back(i);cnt[i] ++;n /= i;}}if(n != 1){if(!cnt[n]) div.push_back(n);cnt[n] ++;n /= n;}return make_pair(cnt, div);}

inline int inputInt(){int X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1);}
inline void outInt(int X){if(X<0) {putchar('-'); X=~(X-1);}int s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}
inline ll inputLL(){ll X=0; bool flag=1; char ch=getchar();while(ch<'0'||ch>'9') {if(ch=='-') flag=0; ch=getchar();}while(ch>='0'&&ch<='9') {X=(X<<1)+(X<<3)+ch-'0'; ch=getchar();}if(flag) return X;return ~(X-1); }
inline void outLL(ll X){if(X<0) {putchar('-'); X=~(X-1);}ll s[20],top=0;while(X) {s[++top]=X%10; X/=10;}if(!top) s[++top]=0;while(top) putchar(s[top--]+'0');}

const ll mod = 1e9 + 7;
const ll N = 30;
struct node { 
        ll num, sum, SUM; // 下一位的填法数量，填法的数的总和，填法的数的平凡和
}dp[N][10][10]; // [位数][各位数和%7][本身%7]
ll b[N], len;   // 一个数被拆分为一个位数组
ll pow10[N];    // 预处理10^i

inline node DFS ( int n, int sum, int num, bool is_max ) { // sum 和 num 都是模7后的值
        if ( !n ) {
                node tmp = {num && sum, 0, 0}; // 本身与数位和有一个是7的倍数的话，开始就一个都给不了
                return tmp;
        } if ( !is_max && ~dp[n][num][sum].num ) return dp[n][num][sum];

        int m = is_max ? b[n] : 9;
        node res = {0, 0, 0};
        for ( int i = 0; i <= m; i ++ ) {
                if ( i == 7 ) continue;
                node tmp = DFS ( n - 1, (sum + i) % 7, (num * 10 + i) % 7, is_max && i == m );
                ll CurPos = pow10[n - 1] * i % mod; // 获得当前位的数确切代表什么（比如231的200部分

                // 记录下属个数
                res.num = (res.num + tmp.num) % mod; 
                // 本位与下属个数相乘，一一对应
                res.sum = (res.sum + (tmp.sum + CurPos * tmp.num % mod) % mod) % mod;
                // 用平方和公式 
                res.SUM = (res.SUM + ((tmp.SUM + 2 * CurPos * tmp.sum % mod) % mod + tmp.num * CurPos % mod * CurPos % mod) % mod) % mod;
        }
        if ( !is_max ) dp[n][num][sum] = res;
        return res;
}

inline ll solve ( ll x ) {
        for ( len = 0; x; x /= 10 ) b[ ++ len ] = x % 10;
        return DFS(len, 0, 0, 1).SUM; 
}

CHIVAS_{
        pow10[0] = 1;
        for ( ll i = 1; i < N; i ++ ) pow10[i] = (pow10[i - 1] * 10) % mod;
        MEM(dp, -1);

        int cass;
        EACH_CASE ( cass ) {
                ll l = inputLL(), r = inputLL();
                outLL((solve(r) - solve(l - 1) + mod) % mod); puts("");
        }
        _REGAL;
}

```

<hr>

## ICPC2020上海站C_SumOfLog

#### 🔗
<a href="https://codeforces.com/gym/102900/problem/C"><img src="https://img-blog.csdnimg.cn/22c08ad3440c4ad7a629af2975f96976.png"></a>

#### 💡
关注一下 <img src="https://latex.codecogs.com/svg.image?\inline&space;[i\&j=0]" title="\inline [i\&j=0]" />，这样的话每一位都不同才可以做出贡献，那么<img src="https://latex.codecogs.com/svg.image?\inline&space;log(i+j)" title="\inline [i\&j=0]" />就是<img src="https://latex.codecogs.com/svg.image?\inline&space;i" title="\inline [i\&j=0]" />和<img src="https://latex.codecogs.com/svg.image?\inline&space;j" title="\inline [i\&j=0]" />的最高位  
第一眼想到排列组合乱搞，但是架不住有 <img src="https://latex.codecogs.com/svg.image?\inline&space;XY" title="\inline [i\&j=0]" /> 的限制让选数不能随便选  
那么既然是上界，可以采用数位dp去跑  
  
限制为两个上界，正常一个位数
所以我们设置 <img src="https://latex.codecogs.com/svg.image?\inline&space;dp[i][j][k]" title="\inline [i\&j=0]" /> 表示枚举到第 <img src="https://latex.codecogs.com/svg.image?\inline&space;i" title="" /> 位，第一个数是否到达上界，第二个数是否到达上界  
  
我们在 <img src="https://latex.codecogs.com/svg.image?\inline&space;dfs" title="" />参数上也保持这样的状态，并因为有两个数，我们在枚举第<img src="https://latex.codecogs.com/svg.image?\inline&space;i" title="" />位的时候应有两重 <img src="https://latex.codecogs.com/svg.image?\inline&space;01" title="" />，并根据是否为最高数来给定枚举的最大值，保证两个不同为<img src="https://latex.codecogs.com/svg.image?\inline&space;1" title="" />即可

#### ✅

```cpp
const int mod = 1e9 + 7;

int a[40], b[40]; // 二进制表示
ll dp[40][2][2];

inline ll DFS ( int id, bool is_high_x, bool is_high_y ) {
        if ( !(~id) ) return 1;
        if ( ~dp[id][is_high_x][is_high_y] ) return dp[id][is_high_x][is_high_y];

        int topx = is_high_x ? a[id] : 1; // 根据最高位设置枚举的最大值
        int topy = is_high_y ? b[id] : 1;

        ll res = 0;
        for ( int i = 0; i <= topx; i ++ ) {
                for ( int j = 0; j <= topy; j ++ ) {
                        if ( i && j ) continue;
                        res += DFS(id - 1, is_high_x && i == topx, is_high_y && j == topy); // 向小位走，传递“是否最高位”
                        res %= mod;
                }
        }
        return dp[id][is_high_x][is_high_y] = res;
}

inline void Solve () {
        ll x, y; cin >> x >> y;
        ll lenx = 0, leny = 0;
        memset(dp, -1, sizeof dp);
        memset(a, 0, sizeof a);
        memset(b, 0, sizeof b);
        while ( x ) a[lenx ++] = x % 2, x /= 2;
        while ( y ) b[leny ++] = y % 2, y /= 2;

        ll res = 0;
        for ( int i = 0; i < max(lenx, leny); i ++ ) {
                int topx = i >= lenx - 1 ? a[i] : 1;
                int topy = i >= leny - 1 ? b[i] : 1;
                for ( int j = 0; j <= topx; j ++ ) {
                        for ( int k = j == 0; k <= topy; k ++ ) { // 最高位都是0的话没有枚举的必要
                                if ( j && k ) continue;
                                res += DFS(i - 1, i >= lenx - 1 && j == topx, i >= leny - 1 && k == topy) * (i + 1) % mod;
                                res %= mod;
                        }
                }
        }
        cout << res << endl;
}

int main () {
        ios::sync_with_stdio(false);
        ll cass; cin >> cass; while ( cass -- ) {
                Solve();
        }
}
```

<hr>
