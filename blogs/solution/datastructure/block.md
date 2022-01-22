---
title: 分块
---
###  
<hr>

## 洛谷P3396_哈希冲突

#### 🔗
https://www.luogu.com.cn/problem/P3396

#### 💡
(应该是国集的一道论文题来着吧)  
  
暴力中对每一个数对应的每一个模数都打一个表，<img src="https://latex.codecogs.com/svg.image?O(n^2)" title="O(n^2)" />肯定会超时  
考虑根号算法  
  
我们可以在发现在查询的时候如果模数x很小的时候一个个暴力肯定是不行的  
所以与朴素的线形分块不同  
这里将整个可以取得模数以<img src="https://latex.codecogs.com/svg.image?\sqrt{n}" title="\sqrt{n}" />为分界线分为两大块  

模数小于<img src="https://latex.codecogs.com/svg.image?\sqrt{n}" title="\sqrt{n}" />的可以直接打表出来sum[x][y]表示这n个数对x取模得y的序号的总和，  
修改的时候可以只对sum进行修改，<img src="https://latex.codecogs.com/svg.image?O(n\sqrt{n})" title="O(n\sqrt{n})" /> 
查询的时候，如果模数<img src="https://latex.codecogs.com/svg.image?x\leqslant&space;\sqrt{n}" title="x\leqslant \sqrt{n}" />，那么我们可以用已有的表直接输出<img src="https://latex.codecogs.com/svg.image?O(1)" title="O(1)" />。如果<img src="https://latex.codecogs.com/svg.image?x\geqslant&space;\sqrt{n}" title="x\geqslant \sqrt{n}" />，那么我们完全暴力（以大于根号n的步长向上跳）去累加，<img src="https://latex.codecogs.com/svg.image?O(\sqrt{n})" title="O(\sqrt{n})" />。  


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
#include <unordered_map>
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
#define TEST(a) cout << "---------" << a << "---------" << '\n'

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
#define EACH_CASE(cass) for (cin >> cass; cass; cass--)

#define LS l, mid, rt << 1
#define RS mid + 1, r, rt << 1 | 1
#define GETMID (l + r) >> 1

using namespace std;

template<typename T> inline void Read(T &x){T f = 1; x = 0;char s = getchar();while(s < '0' || s > '9'){if(s == '-') f = -1; s = getchar();}while('0'<=s&&s<='9'){x=(x<<3)+(x<<1)+(s^48);s=getchar();}x*=f;}
template<typename T> inline T MAX(T a, T b){return a > b? a : b;}
template<typename T> inline T MIN(T a, T b){return a > b? b : a;}
template<typename T> inline void SWAP(T &a, T &b){T tp = a; a = b; b = tp;}
template<typename T> inline T GCD(T a, T b){return b > 0? GCD(b, a % b) : a;}
template<typename T> inline void ADD_TO_VEC_int(T &n, vector<T> &vec){vec.clear(); cin >> n; for(int i = 0; i < n; i ++){T x; cin >> x, vec.PB(x);}}
template<typename T> inline pair<T, T> MaxInVector_ll(vector<T> vec){T MaxVal = -LNF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return {MaxVal, MaxId};}
template<typename T> inline pair<T, T> MinInVector_ll(vector<T> vec){T MinVal = LNF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return {MinVal, MinId};}
template<typename T> inline pair<T, T> MaxInVector_int(vector<T> vec){T MaxVal = -INF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return {MaxVal, MaxId};}
template<typename T> inline pair<T, T> MinInVector_int(vector<T> vec){T MinVal = INF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return {MinVal, MinId};}
template<typename T> inline pair<map<T, T>, vector<T> > DIV(T n){T nn = n;map<T, T> cnt;vector<T> div;for(ll i = 2; i * i <= nn; i ++){while(n % i == 0){if(!cnt[i]) div.push_back(i);cnt[i] ++;n /= i;}}if(n != 1){if(!cnt[n]) div.push_back(n);cnt[n] ++;n /= n;}return {cnt, div};}
template<typename T>             vector<T>& operator--            (vector<T> &v){for (auto& i : v) --i;            return  v;}
template<typename T>             vector<T>& operator++            (vector<T> &v){for (auto& i : v) ++i;            return  v;}
template<typename T>             istream& operator>>(istream& is,  vector<T> &v){for (auto& i : v) is >> i;        return is;}
template<typename T>             ostream& operator<<(ostream& os,  vector<T>  v){for (auto& i : v) os << i << ' '; return os;}

const int maxn = 150010;
int n, m, len;//序列个数，查询次数，模数上界
int sum[500][500];//模数计和表
int val[maxn];
char cmd;
int x, y;

inline void GET_SUM(){//打表
        for(int i = 1; i <= n; i ++){
                for(int p = 1; p <= len; p ++){//模数
                        sum[p][i % p] += val[i];
                }
        }
}

inline void Update(int x, int y){//对每一个模数表进行修改
        for(int i = 1; i <= len; i ++) sum[i][x % i] -= val[x] - y; 
        val[x] = y;
}

inline void Query(int x, int y){
        if(x <= len){
                cout << sum[x][y] << endl;
        }else{
                int res = 0;
                for(int i = y; i <= n; i += x) res += val[i];//以大于根号n的步长向上跳
                cout << res << endl;
        }
}

CHIVAS_{
        cin >> n >> m; len = sqrt(n);
        for(int i = 1; i <= n; i ++) cin >> val[i];
        GET_SUM();
        while(m --){
                cin >> cmd >> x >> y;
                if(cmd == 'A') Query(x, y);
                else           Update(x, y);
        }
        _REGAL;
}
```

<hr>

## 洛谷P3870_开关

#### 🔗
https://www.luogu.com.cn/problem/P3870

#### 💡
分块的入门题  
就是把块长记录下来，设为len  
存一个块的lazy懒惰标记，存一个块的总和sum  
  
查询：  
对散的元素一个个计算他们进行完lazy后的状态，然后对块的sum进行累加  
更新：  
对散的元素一个个更新并同时更新所处块的sum(按该元素进行完lazy后的状态对sum更新)，对块的sum更新是len - sum[i]，同时lazy多记录1  

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
#include <unordered_map>
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
#define TEST(a) cout << "---------" << a << "---------" << '\n'

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
#define EACH_CASE(cass) for (cin >> cass; cass; cass--)

#define LS l, mid, rt << 1
#define RS mid + 1, r, rt << 1 | 1
#define GETMID (l + r) >> 1

using namespace std;

template<typename T> inline void Read(T &x){T f = 1; x = 0;char s = getchar();while(s < '0' || s > '9'){if(s == '-') f = -1; s = getchar();}while('0'<=s&&s<='9'){x=(x<<3)+(x<<1)+(s^48);s=getchar();}x*=f;}
template<typename T> inline T MAX(T a, T b){return a > b? a : b;}
template<typename T> inline T MIN(T a, T b){return a > b? b : a;}
template<typename T> inline void SWAP(T &a, T &b){T tp = a; a = b; b = tp;}
template<typename T> inline T GCD(T a, T b){return b > 0? GCD(b, a % b) : a;}
template<typename T> inline void ADD_TO_VEC_int(T &n, vector<T> &vec){vec.clear(); cin >> n; for(int i = 0; i < n; i ++){T x; cin >> x, vec.PB(x);}}
template<typename T> inline pair<T, T> MaxInVector_ll(vector<T> vec){T MaxVal = -LNF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return {MaxVal, MaxId};}
template<typename T> inline pair<T, T> MinInVector_ll(vector<T> vec){T MinVal = LNF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return {MinVal, MinId};}
template<typename T> inline pair<T, T> MaxInVector_int(vector<T> vec){T MaxVal = -INF, MaxId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MaxVal < vec[i]) MaxVal = vec[i], MaxId = i; return {MaxVal, MaxId};}
template<typename T> inline pair<T, T> MinInVector_int(vector<T> vec){T MinVal = INF, MinId = 0;for(int i = 0; i < (int)vec.size(); i ++) if(MinVal > vec[i]) MinVal = vec[i], MinId = i; return {MinVal, MinId};}
template<typename T> inline pair<map<T, T>, vector<T> > DIV(T n){T nn = n;map<T, T> cnt;vector<T> div;for(ll i = 2; i * i <= nn; i ++){while(n % i == 0){if(!cnt[i]) div.push_back(i);cnt[i] ++;n /= i;}}if(n != 1){if(!cnt[n]) div.push_back(n);cnt[n] ++;n /= n;}return {cnt, div};}
template<typename T>             vector<T>& operator--            (vector<T> &v){for (auto& i : v) --i;            return  v;}
template<typename T>             vector<T>& operator++            (vector<T> &v){for (auto& i : v) ++i;            return  v;}
template<typename T>             istream& operator>>(istream& is,  vector<T> &v){for (auto& i : v) is >> i;        return is;}
template<typename T>             ostream& operator<<(ostream& os,  vector<T>  v){for (auto& i : v) os << i << ' '; return os;}

const int maxn = 2e5 + 10;
int n, m, len;//总长、查询次数、块长
int c, a, b;
int sum[350];//块的总和
int lazy[350];//懒惰标记，记录这个块更新了多少次
int w[maxn];//灯的状态

inline int get(int i){//获取第i个元素所处块的编号
        return i / len;
}

inline int Query(int l, int r){//查询区间和
        int res = 0;
        if(get(l) == get(r)){//所处同一个块，全是散数，直接暴力
                for(int i = l; i <= r; i ++) res += (lazy[get(i)] & 1) ? w[i] ^ 1 : w[i];//如果变了奇数次，就说明有变值
        }else{
                int i = l, j = r;
                while(get(i) == get(l)) res += (lazy[get(i)] & 1) ? w[i] ^ 1 : w[i], i ++;//将左侧散数给算了，方式和上面一样
                while(get(j) == get(r)) res += (lazy[get(j)] & 1) ? w[j] ^ 1 : w[j], j --;//将右侧散数给算了，方式和上面一样
                for(int k = get(i); k <= get(j); k ++) res += sum[k];//计算中间完整的块
        }return res;
}

inline void Update(int l, int r){//更新区间
        if(get(l) == get(r)){//所处同一个块，全是散数，直接暴力
                for(int i = l; i <= r; i ++) sum[get(i)] += (Query(i, i) ^ 1) - Query(i, i), w[i] ^= 1;//该块的sum + （变换后的值 - 变换前的值），同时一个个更新
        }else{
                int i = l, j = r;
                while(get(i) == get(l)) sum[get(i)] += (Query(i, i) ^ 1) - Query(i, i), w[i] ^= 1, i ++;//将左侧散数给算了，该块的sum + （变换后的值 - 变换前的值），同时一个个更新
                while(get(j) == get(r)) sum[get(j)] += (Query(j, j) ^ 1) - Query(j, j), w[j] ^= 1, j --;//将右侧散数给算了，该块的sum + （变换后的值 - 变换前的值），同时一个个更新
                for(int k = get(i); k <= get(j); k ++) sum[k] = len - sum[k], lazy[k] ++;//计算中间完整的块，并记录变了多少次
        }
}



CHIVAS_{
        cin >> n >> m; len = sqrt(n);
        while(m --){
                cin >> c >> a >> b;

                if(c) cout << Query(a, b) << endl;
                else  Update(a, b);
        }
        _REGAL;
}


```

<hr>

## 洛谷P4109_定价

#### 🔗
https://www.luogu.com.cn/problem/P4109

#### 💡
同样的长度，后面的0越多，则荒谬度越小  
所以我们可以设定一个固定的len值为十的正整数次方，观测数据量，这里用10000更合适  
每10000为区间的数的荒谬度一样，同时题目又让求最小的，于是我们可以用块的最左侧数代表整个区间  
  
写一个VAL函数维护荒谬度，其余就是分块跑一遍就行了  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >

```cpp
#include <iostream>

using namespace std;

int len = 10000;

inline int get ( int id ) { return id / len; }

inline int VAL ( int num ) {
        while ( num % 10 == 0 ) num /= 10;
        return 2 * to_string(num).size() - (num % 10 == 5);
}

inline int Query ( int l, int r ) {
        int res = l, MinVal = VAL(l);
        if ( get(l) == get(r) ) {
                for ( int i = l; i <= r; i ++ ) if ( MinVal > VAL(i) ) MinVal = VAL(i), res = i;
        } else {
                int i = l, j = r;
                while ( get(i) == get(l) ) { if ( MinVal > VAL(i) ) MinVal = VAL(i), res = i; i ++; }
                for ( int k = get(i); k < get(j); k ++ ) if ( MinVal > VAL(k * len) ) MinVal = VAL(k * len), res = k * len;
                while ( get(j) == get(r) ) { if ( MinVal > VAL(j) ) MinVal = VAL(j), res = j; j --; }
        }
        return res;
}

int main () {

        int cass;
        for ( cin >> cass; cass; cass -- ) {
                int l, r; cin >> l >> r;
                cout << Query ( l, r ) << endl;
        }

}
```

<hr>
