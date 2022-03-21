---
title: 反向思维
---

###  
<hr>

## CodeForces1638D_BigBrush

#### 🔗
<a href="https://codeforces.com/contest/1638/problem/D"><img src="https://img-blog.csdnimg.cn/a149526db0e84b03a28213d910f72991.png"></a>

#### 💡
考虑覆盖效果  
发现最后一个覆盖的元素一定是四个方格全有的  
倒数第二个覆盖的可以有一部分在这四个方格内，但它所染色的四个点不在这些方格内的点一定要同色才可以    
接下来同理  
  
那么可以使用倒着构造操作的方式  
每次看看是否有可以涂色的且未出发的点  
将它塞入操作内  
然后去看它所包含的四个点是否有可以入队的  
这样进行 BFS  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >
```cpp
const int N = 1e3 + 10;
int a[N][N];
int n, m;
struct node { int x, y, val; };
int dx[4] = {0, 0, -1, 1};
int dy[4] = {1, -1, 0, 0};
bool vis[N][N];
bool ismark[N][N];

inline int check ( int x, int y ) {
        if ( x <= 0 || y <= 0 || x >= n || y >= m || vis[x][y] ) return -1;
        vector<pair<int, int> > vec;
        if ( ismark[x][y] == 0 ) vec.push_back({x, y});
        if ( ismark[x + 1][y] == 0 ) vec.push_back({x + 1, y});
        if ( ismark[x][y + 1] == 0 ) vec.push_back({x, y + 1});
        if ( ismark[x + 1][y + 1] == 0 ) vec.push_back({x + 1, y + 1});
        if ( vec.size() == 0 ) return 1;
        int clr = a[vec[0].first][vec[0].second];
        for ( int i = 0; i < vec.size(); i ++ ) {
                if ( clr != a[vec[i].first][vec[i].second] ) return -1;
        }
        return clr;
}

inline void Solve () {
        scanf("%d%d", &n, &m);
        for ( int i = 1; i <= n; i ++ ) for ( int j = 1; j <= m; j ++ ) scanf("%d", &a[i][j]), vis[i][j] = 0, ismark[i][j] = 0;
        vector<node> res;

        queue<pair<int, int> > que;
        for ( int i = 1; i < n; i ++ ) {
                for ( int j = 1; j < m; j ++ ) {
                        if ( a[i][j] == a[i + 1][j] && a[i][j] == a[i][j + 1] && a[i][j] == a[i + 1][j + 1] ) {
                                que.push({i, j});
                        }
                }
        }
        while ( que.size() ) {
                pair<int, int> cur = que.front(); que.pop();
                int x = cur.first, y = cur.second;
                int chk = check(x, y);
                if ( chk == -1 ) continue;
                res.push_back({x, y, chk});
                vis[x][y] = 1;
                ismark[x][y] = ismark[x + 1][y] = ismark[x][y + 1] = ismark[x + 1][y + 1] = 1;
                for ( int i = 0; i < 4; i ++ ) {
                        int nx = x + dx[i];
                        int ny = y + dy[i];
                        que.push({nx, ny});
                }
        }
        for ( int i = 1; i <= n; i ++ ) for ( int j = 1; j <= m; j ++ ) {
                if ( !ismark[i][j] ) {
                        puts("-1");
                        return;
                }
        }
        printf("%d\n", (int)res.size());
        for ( int i = res.size() - 1; i >= 0; i -- ) printf("%d %d %d\n", res[i].x, res[i].y, res[i].val);
}

int main () {
        Solve();
}
```
<hr>

## CodeForces1644D_CrossColoring

#### 🔗
<a href="https://codeforces.com/contest/1644/problem/D"><img src="https://img-blog.csdnimg.cn/cd8c3039252f4d2389545fc8cf0388ac.png"></a>

#### 💡
首先每一个颜色是否被覆盖是与顺序有关的  
它被覆盖也是被后面的情况覆盖  
因为每次涂色是涂一行 $+$ 一列  
我们先以行为考虑  
它是否能被保留下来的条件是  
- 后面涂色列没有涂完所有的行  
- 后面没有涂过这一行
对于一次涂色，如果行和列有一个保留，那么就可以让答案 $+1$   
因为是考虑的都是后面的  
所以我们反向操作，是否涂过可以是用 `vis[]` 标记数组，对应的是否涂完了可以用 `set` 进行看 `size` 是否满了  
每次判断完存一下标记和 `set`   
最后就是求个数，这一看就是一个球盒模型的球不同盒不同可空模型  
所以是 $k^{cnt}$  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >
```cpp
const int N = 2e5 + 10;
const int mod = 998244353;

inline ll ksm ( ll a, ll b ) { ll res = 1; while ( b ) { if ( b & 1 ) res = res * a % mod; a = a * a % mod; b >>= 1; } return res; }

int row[N], col[N];
int vis1[N], vis2[N];
bool flag[N];

inline void Solve () {
        int n, m; cin >> n >> m;
        ll k, q; cin >> k >> q;
        for ( int i = 0; i < q; i ++ ) {
                cin >> row[i];
                cin >> col[i];
                flag[i] = false;
        }
        for  (int i = 0; i <= n; i ++) vis1[i] = 0;
        for  (int i = 0; i <= m; i ++) vis2[i] = 0;
        set<int> st1, st2;
        for ( int i = q - 1; i >= 0; i -- ) {
                if ( st1.size() == n && st2.size() == m ) break;
                if ( st1.size() != n && !vis2[col[i]] ) flag[i] = true;
                if ( st2.size() != m && !vis1[row[i]] ) flag[i] = true;
                vis1[row[i]] = vis2[col[i]] = 1;
                st1.insert(row[i]);
                st2.insert(col[i]);
        }
        ll num = 0;
        for ( int i = 0; i < q; i ++ ) num += flag[i];
        cout << ksm(k, num) << endl;
}
```
<hr>

## CodeForces1654C_AliceAndTheCake

#### 🔗
<a href="https://codeforces.com/contest/1654/problem/C">![20220321220720](https://raw.githubusercontent.com/Tequila-Avage/PicGoBeds/master/20220321220720.png)</a>

#### 💡
一个一个组会很麻烦，因为有的是需要和自己同大小的组，有的是需要和不同大小的组  
所以我们考虑瓜分，从大到小  
如果当前数不存在就继续瓜分，存在的话就直接用了并且返回    
如果瓜分不出来（也就是 $1$ ）还没有的话就不行  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >
```cpp
map<ll, int> num;
bool flag = true;
inline void DFS ( ll x ) {
        if ( !flag ) {
                return;
        }
        if ( num[x] ) {
                num[x] --;
                return;
        }
        if ( x == 1 ) {
                flag = false;
                return;
        }
        if ( x % 2 ) {
                DFS(x / 2);
                DFS(x / 2 + 1);
        } else {
                DFS(x / 2);
                DFS(x / 2);
        }
}
 
inline void Solve () {
        int n; cin >> n;
        num.clear(); ll sum = 0;
        for ( int i = 0; i < n; i ++ ) {
                ll x; cin >> x;
                num[x] ++;
                sum += x;
        }
        flag = true;
        DFS(sum);
        if ( !flag ) cout << "NO\n";
        else cout << "YES\n";
}
```
<hr>


## CCPC2021网络赛_JumpingMonkey

#### 🔗
<a href="https://acm.hdu.edu.cn/showproblem.php?pid=7136"><img src="https://i.loli.net/2021/10/11/3XzVMLBKTsqUZah.png"></a>

#### 💡
由于每一个很大的点都可以挡住一定范围的点对互相连通  
所以每一个点所能到达的范围，其实是一个被拆开之后的连通块   
那么拆的方式也就是从最大的点向最小的点递进  
每一次可以拆掉每个连通块内最大的点，同一次被拆掉的点都是同级的  
如：第一次是整棵树最大的点x，第二次是拆掉x后剩下的连通块的最大的点...  
他们的级数就是他们能跳到的点数  
  
这样去拆很难把时间复杂度降低下来  
我们可以试着反向模拟  
从最小的点开始枚举  
每一次它将连接"与它相连且已经枚举过了的连通块"，并将它作为这个连通块的根节点（也就是连接它和这个连通块的根节点）    
这样构建出的一棵树，其中每个点的深度就是他们能跳到的点树  
  
在构造树的过程中我们可以使用并查集  
可以发现在最后一次遍历新树之前，所有连通块除了根节点之外毫无作用  
我们就可以用并查集记录这个连通块的根节点，然后每次连接枚举的点x和与x相连的且已经枚举过的儿子节点to的根节点nod[to]  
  
最后跑一次记录一下深度即可  

#### <img src="https://img-blog.csdnimg.cn/20210713144601841.png" >

```cpp
const int N = 2e5 + 10;
struct Edge {
        int nxt, to;
} edge[2][N];
int head[2][N], cnt[2]; // edge[0][]老树， edge[1][]新树
inline void add_Edge ( int op, int from, int to ) { // 连边
        edge[op][++ cnt[op]] = { head[op][from], to };
        head[op][from] = cnt[op];
}

namespace union_Find { // 并查集
        int nod[N];
        inline void Init ( int n ) { for ( int i = 1; i <= n; i ++ ) nod[i] = i; }
        inline int Find ( int x ) { return x == nod[x] ? x : nod[x] = Find(nod[x]); }
        inline void Merge ( int x, int y ) { int fx = Find(x), fy = Find(y); nod[fx] = fy; }
}

#define pii pair<int, int>
#define x first
#define y second
pii a[N]; // x: val, y: id， 输入的a
int n, depth[N]; // 输入的n，深度 

inline void dfs ( int x, int fath ) { // 求深度的dfs
        depth[x] = depth[fath] + 1;
        for ( int i = head[1][x]; ~i; i = edge[1][i].nxt ) {
                int to = edge[1][i].to;
                if ( to == fath ) continue;
                dfs ( to, x );
        }
}

inline void Solve () {
        memset ( head[0], -1, sizeof head[0] );
        memset ( head[1], -1, sizeof head[1] );
        cnt[0] = cnt[1] = 0;
        scanf("%d", &n);
        for ( int i = 0, x, y; i < n - 1; i ++ ) 
                scanf("%d%d", &x, &y),
                add_Edge ( 0, x, y ),
                add_Edge ( 0, y, x );
        for ( int i = 1; i <= n; i ++ )
                scanf("%d", &a[i].x),
                a[i].y = i;
        sort ( a + 1, a + n + 1, [&](pair<int, int> a, pair<int, int> b){ // 按val升序排序
                return a.first < b.first;
        });

        union_Find::Init( n );
        map<int, bool> vis;
        for ( int i = 1; i <= n; i ++ ) { // 枚举
                vis[a[i].y] = true; // 枚举过了
                for ( int j = head[0][a[i].y]; ~j; j = edge[0][j].nxt ) { // 跑一遍这个编号的儿子
                        if ( !vis[edge[0][j].to] ) continue;              // 如果还没有枚举过，就不连接
                        int fj = union_Find::Find(edge[0][j].to);         // 它儿子所在连通块的根节点
                        if ( union_Find::nod[fj] != a[i].y )              // 如果它儿子没有和它连接过 
                                add_Edge ( 1, a[i].y, fj ),
                                add_Edge ( 1, fj, a[i].y ),
                                union_Find::nod[fj] = a[i].y;              // 同时儿子的连通快根节点认父为连通块根节点
                }
        }
        depth[a[n].y] = 0; dfs ( a[n].y, a[n].y );  // 建立深度
        for ( int i = 1; i <= n; i ++ ) printf("%d\n", depth[i]);
}

int main () {
        int cass; scanf("%d", &cass); while ( cass -- ) {
                Solve();
        }
}
```

<hr>