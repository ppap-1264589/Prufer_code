## [Trang chủ](https://ppap-1264589.github.io/interesting-solution)

## Tài liệu thuật toán
[cp algorithms](https://cp-algorithms.com/graph/pruefer_code.html)


### Ý nghĩa
Prufer code giúp ta nén một cây n đỉnh và n-1 cạnh thành một dãy n-2 số

Ứng dụng quan trọng nhất của thuật toán là chứng minh cho công thức Cayley: Số cách tạo một cây khung n đỉnh là 
n^(n-2) cách

Code dưới đây là cách để nén một cây theo thuật toán Prufer. Đồng thời là cách để khôi phục lại dữ liệu cây từ dãy Prufer cho trước

### Code : [PRUFER.cpp](https://ideone.com/pVGryP)

``` c++
#include <bits/stdc++.h>
#define up(i,a,b) for (int i = (int)a; i <= (int)b; i++)
#define ep emplace_back
#define pii pair<int, int>
#define f first
#define s second
using namespace std;

const int maxn = 1e5 + 10;
vector<int> a[maxn];
int par[maxn];
int n;

void DFS(int u){
    for (int& v : a[u]){
        if (v == par[u]) continue;
        par[v] = u;
        DFS(v);
    }
}

vector<int> prufer() {
    vector<int> code;
    vector<int> degree(n+3, 0);
    DFS(n);

    int cur = -1;
    up(i,1,n){
        degree[i] = a[i].size();
        if (cur == -1 && degree[i] == 1){
            cur = i;
        }
    }

    int leaf = cur;
    up(i,1,n-2){
        int p = par[leaf];
        code.ep(p);

        if (--degree[p] == 1 && p < cur) leaf = p;
        else{
            ++cur;
            while (degree[cur] != 1) ++cur;
            leaf = cur;
        }
    }

    return code;
}

vector<pii> deprufer(const vector<int>& code){
    vector<pii> E;
    vector<int> degree(n+3, 1);
    for (auto x : code) ++degree[x];

    int cur = -1;
    up(i,1,n) if (degree[i] == 1){
        cur = i;
        break;
    }

    int leaf = cur;
    for (auto x : code){
        E.ep(leaf, x);
        if (--degree[x] == 1 && x < cur){
            leaf = x;
        }
        else{
            ++cur;
            while (degree[cur] != 1) ++cur;
            leaf = cur;
        }
    }
    E.ep(n, leaf);
    return E;
}

void decompess(){
	vector<int> p;
    cin >> n;
    up(i,1,n-2){
        int x;
        cin >> x;
        p.ep(x);
    }
    vector<pii> E = deprufer(p);
    for (auto x : E) cout << x.f << " " << x.s << "\n";
}

void compress(){
    cin >> n;
    up(i,1,n-1){
        int u,v;
        cin >> u >> v;
        a[u].ep(v);
        a[v].ep(u);
    }
    vector<int> p = prufer();
    for (auto x : p) cout << x << " ";
}

signed main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    #define Task "PRUFER"
    if (fopen(Task".inp", "r")){
        freopen(Task".inp", "r", stdin);
        freopen(Task".out", "w", stdout);
    }

    compress();
}
```
