เนื่องจากกราฟที่โจทย์ให้มาเป็น *connected graph* ที่มี $N$ โหนด และ *edge* $N-1$ เส้น กราฟที่โจทย์ให้มาคือ[**กราฟต้นไม้(Tree)**](https://en.wikipedia.org/wiki/Tree_%28graph_theory%29#:~:text=In%20graph%20theory,%20a%20tree,a%20connected%20acyclic%20undirected%20graph.&text=A%20polytree%20%28or%20directed%20tree,undirected%20graph%20is%20a%20tree.)  
ผู้เขียนจะขอสมมุติให้ *tree* ที่โจทย์ให้มาเป็น *rooted tree* และ มี *root* เป็น โหนดหมายเลข 1   

## Subtask 4 $(N\leq10^5, K\leq5)$

หลังจากนี้จะขอเรียก *คำสั่งของเหตุการณ์ที่ 1* ว่า **update(v, k)** และ *คำสั่งของเหตุการณ์ที่ 2* ว่า **query(v)**  
  
สำหรับคำสั่ง **update(v, k)** หากเราทำตรง ๆ โดยการเพิ่มค่าจำนวนชนิดไวรัสให้กับทุกโหนดที่อยู่ห่างจากโหนด *v* ไม่เกิน *k* จริงๆ เราจะสามารถทำคำสั่ง **query** ได้ใน $\mathcal{O}(1)$ แต่ทว่าในวิธีดังกล่าว **update(v, k)** จะมี *complexity* เป็น $\mathcal{O}(N)$ เนื่องจาก *worst case* ของมันคือ [**Star graph**](https://en.wikipedia.org/wiki/Star_(graph_theory))  

ในบางครั้ง การยอมเพิ่ม *complexity* ของคำสั่ง **query** เพื่อแลกกับการลด *complexity* ของคำสั่ง **update** อาจจะเป็นทางเลือกที่ดีกว่า  

**นิยาม** $reach[u][i]$ = จำนวนชนิดไวรัสที่เกิดขึ้นที่ *descendant* ของ $u$ ที่สามารถเดินทางขึ้นมาถึง**โหนด** $u$ ได้ในขณะที่ยังสามารถเดินทางได้อีก $i$ ก้าว  
**นิยาม** $reach\_parent[u][i]$ = จำนวนชนิดไวรัสที่เกิดขึ้นที่ *descendant* ของ $u$ ที่สามารถเดินทางขึ้นมาถึง**โหนด parent ของ** $u$ ได้ในขณะที่ยังสามารถเดินทางได้อีก $i$ ก้าว  

ในคำสั่ง **update(v, k)**  เราจะดำเนินการดังนี้
* เนื่องจากมีไวรัสชนิดใหม่เกิดขึ้นที่โหนด $v$ ดังนั้นเราต้องมีการเพิ่มค่าให้กับ *reach* ของ $v$ และเหล่า *ancestor* ของ $v$ ที่เราสามารถเดินทางขึ้นไปถึงได้ ซึ่งมีไม่เกิน $k$ โหนด
	* $reach[v][k]$
	* $reach[$*parent* อันดับที่ 1 ของ $v][k-1]$
	* $reach[$*parent* อันดับที่ 2 ของ $v][k-2]$
	*  ... 
	* $reach[$*parent* อันดับที่ $k$ ของ $v][0]$
* และทำในทำนองคล้าย ๆ กันกับ *reach_parent* เพียงแต่การเดินทางจากโหนด $v$ ไปยัง *parent* ของ $v$ จะต้องเสียก้าวเดินอีก 1 ก้าว
	* $reach\_parent[v][k-1]$
	* $reach\_parent[$*parent* อันดับที่ 1 ของ $v][k-2]$
	* $reach\_parent[$*parent* อันดับที่ 2 ของ $v][k-3]$
	*  ... 
	* $reach\_parent[$*parent* อันดับที่ $k-1$ ของ $v][0]$

ในคำสั่ง **query(v)** เราจะพิจารณา**โหนด $v$ และเหล่า *ancestor* ของโหนด** $v$ สมมุติว่าตอนนี้เรากำลังพิจารณาโหนด $u$
* เนื่องจากเราต้องการหาไวรัสที่สามารถเดินมายังโหนด $v$ ได้ แสดงว่าไวรัสนั้นจะต้อง**สามารถเดินทางเดินทางมายัง $u$ ได้** และ **มีจำนวนก้าวเหลือพอที่จะเดินต่อมายัง $v$**
* และเนื่องจาก $k\leq5$ ดังนั้นจำนวนชนิดของไวรัสที่สามารถเดินจาก $u$ มายัง $v$ ได้คือ $\sum_{i=dist(u,v)}^{5}reach[u][i]$  
* สังเกตุได้ว่า **โหนดที่เราต้องพิจารณาจะไม่เกิน *ancestor* อันดับที่ 5 ของ $v$** เนื่องจากไวรัสไม่สามาถเดินได้เกิน 5 ก้าว$(k\leq5)$ ดังนั้นไวรัสจาก *ancestor* อันดับที่ 6 เป็นต้นไปจะไม่มีทางเดินมาถึง $v$ แน่นอน
  
เราจะค่อย ๆ พิจารณาจากโหนด $v$ ขึ้นมาเรื่อยๆ และเพิ่มค่า $\sum_{i=dist(u,v)}^{5}reach[u][i]$  ให้กับคำตอบ **แต่การเพิ่มค่าดังกล่าวจะทำให้เกิดคำตอบซ้ำขึ้น เช่น ไวรัสที่สามารถเดินมาได้ทั้ง $u$ และ parent ของ $u$ เป็นต้น** ดังนั้นเราจึงต้องลบค่า  $\sum_{i=dist(u,v)+1}^{5}reach\_parent[u][i]$ ออกด้วยหาก $u$ ไม่ใช่โหนดสุดท้ายที่เราจะพิจารณา  

ด้านล่างนี้เป็นตัวอย่างโค้ดของคำสั่ง **query(v)**
```cpp
int ans = 0;
for (int i = 0; i < 6 && v != -1; ++i) {
  // i is the same as dist(v, u)
  for (int j = i; j < 6; j++) {
    ans += reach[v][j];
  }
  // check if it is the final ancestor
  if (i < 5 && parent[v] != -1) {
    for (int j = i + 1; j < 6; j++) {
      ans -= reach_parent[v][j];
    }
  }
  // continue on the parent of v
  v = parent[v];
}
printf("%d\n", ans);
```  
สังเกตุได้ว่า **query(v)** มี *complexity* เพิ่มขึ้นเป็น $\mathcal{O}(K^2)$ ในขณะที่คำสั่ง **update(v, k)** มี *complexity* ลดลงเหลือเพียง $\mathcal{O}(K)$ เท่านั้นเอง  

Time Complexity : $\mathcal{O}(N+Q\cdot K^2)$  
Space Complexity : $\mathcal{O}(N\cdot K)$
  
**อนึ่ง เราสามารถ *optimize* ให้ Time Complexity เหลือ $\mathcal{O}(N+Q\cdot K)$ ได้**
## Subtask 5 $(N\leq10^5,K\leq10^5)$

เราไม่สามารถทำเหมือนใน **Subtask 4** ด้วยข้อจำกัด 2 อย่างด้วยกัน
* **จำนวนโหนดที่ต้องพิจารณา** จำนวน *ancestor* ที่เราต้องพิจารณามีมากถึง $\mathcal{O}(K)$ ซึ่งช้าเกินไปใน subtask นี้
* **การเก็บ reach และ reach_parent** ขนาดของตาราง *reach* และ *reach_parent* คือ $\mathcal{O}(N\cdot K)$ ซึ่งใหญ่เกินไปใน subtask นี้
  
เราสามารถจัดการกับข้อจำกัดแต่ละข้อได้ ดังนี้

### จำนวนโหนดที่ต้องพิจารณา

บ่อยครั้งมาก ที่หากจำนวน *ancestor* ที่เราต้องพิจารณามีจำนวนมากเกินไป เราสามารถใช้เทคนิค [**Heavy Light Decomposition**](https://en.wikipedia.org/wiki/Heavy_path_decomposition) หรือ [**Centroid Decomposition**](https://tanujkhattar.wordpress.com/2016/01/10/centroid-decomposition-of-a-tree/) มาช่วยให้แก้ปัญหาได้อย่างมีประสิทธิภาพได้  

และบ่อยครั้งเช่นกัน ที่หากใช้ **Heavy Light Decomposition** แล้วยุ่งยากเกินไป การเลือกเปลี่ยนมาพิจารณา **Centroid Decomposition** จะนำไปสู่ solution ที่เรียบง่ายกว่า  

ด้วย *Centroid Decomposition* เราสามารถ *decompose* ต้นไม้ของโจทย์ข้อนี้ให้เหลือโหนดที่ต้องพิจารณาเหลือไม่เกิน $\log N$ โหนด 

### การเก็บ reach และ reach_parent

แทนที่เราจะเก็บเป็นตารางที่เก็บทุกค่าที่เป็นไปได้ของ $k$ เอาไว้ **ให้เราเก็บเฉพาะค่า $k$ ของไวรัสที่มาถึงโหนดปัจจุบันได้แทน**  

และ เมื่อย้อนกลับไปพิจารณาสมการ $\sum_{i=dist(u,v)}^{5}reach[u][i]$ และ $\sum_{i=dist(u,v)+1}^{5}reach\_parent[u][i]$ จะพบว่า ในคำสั่ง **query** ต้องการถามหาว่า ในค่าทั้งหมดที่เก็บไว้ในโหนดปัจจุบัน **มีกี่จำนวนที่มากกว่าเท่ากับ $dist(u,v)$ หรือ $dist(u,v)+1$** นั่นเอง  
เราสามารถทำแบบ off-line โดยการรับค่าทั้งหมดและดำเนินการเพิ่มค่าที่แต่ละโหนดจะต้องมีในคำสั่ง **update** ก่อน หลังจากนั้นให้ทำการ *sort* ค่าเหล่านั้นในทุก ๆ โหนด พอเรากลับไปกลับไปพิจารณาข้อมูลคำสั่งทั้งหมดอีกครั้ง เราสามารถใช้ [**Fenwick tree**](https://en.wikipedia.org/wiki/Fenwick_tree) หรือ [**Segment tree**](https://en.wikipedia.org/wiki/Segment_tree) ในการเพิ่มค่าให้คำสั่ง **update** และ หาคำตอบสำหรับคำสั่ง **query** ในแต่ละโหนดได้ใน $\mathcal{O}(\log N)$  

ทั้งนี้ เราสามารถทำแบบ on-line ได้ด้วย *Data Structure* อย่างเช่น [**Red-black tree**](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree) หรือ [**Treap**](https://en.wikipedia.org/wiki/Treap)  ได้ แต่ด้วยข้อจำกัดของ *Time Limit* ทำให้ *Red-black tree* ของ *STL (Standard Template Library)* หรือ *Treap* ไม่สามารถผ่านได้ เพราะมี *constant* สูงเกินไป 
 
### สรุป

เราสามารถพิจารณา *ancestor* แค่ไม่เกิน $\log N$ โหนด และในแต่ละโหนดเราสามารถหาค่าที่ต้องการได้ใน $\log N$ ดังนั้นจะได้ว่า  

Time Complexity : $\mathcal{O}(N+Q\log^2 N)$  
Space Complexity : $\mathcal{O}(N+Q\log N)$  

Code :
```cpp
#include <bits/stdc++.h>
#define pb push_back
#define eb emplace_back
#define all(a) (a).begin(), (a).end()
#define SZ(a) (int)(a).size()
#define make_unique(a)                                                         \
  sort(all((a))), (a).resize(unique(all((a))) - (a).begin())

using namespace std;

typedef vector<int> vi;

const int N = 100005, lgn = 17;

struct Centroid {
  vector<bool> dead;
  vi sz;

  int dfs(int u, int p, vector<vi> &g) {
    sz[u] = 1;
    for (int e : g[u]) {
      if (e == p || dead[e])
        continue;
      sz[u] += dfs(e, u, g);
    }
    return sz[u];
  }
  int find_root(int u, int p, int n, vector<vi> &g) {
    for (int e : g[u]) {
      if (e == p || dead[e])
        continue;
      if (sz[e] > (n >> 1))
        return find_root(e, u, n, g);
    }
    return u;
  }
  void gen_dist(int u, int p, int lv, vector<vi> &g, vector<vi> &dist) {
    if (u != p)
      dist[u][lv] = dist[p][lv] + 1;
    for (int e : g[u]) {
      if (e == p || dead[e])
        continue;
      gen_dist(e, u, lv, g, dist);
    }
  }
  int play(int u, int p, vi &level, vi &parent, vector<vi> &g,
           vector<vi> &dist) {
    dfs(u, u, g);
    int x = find_root(u, u, sz[u], g);
    dead[x] = true;
    if (p != -1)
      level[x] = level[p] + 1;
    gen_dist(x, x, level[x], g, dist);
    parent[x] = p;
    for (int e : g[x]) {
      if (dead[e])
        continue;
      play(e, x, level, parent, g, dist);
    }
    return x;
  }
  void init(vi &level, vi &parent, vector<vi> &g, vector<vi> &dist) {
    int n = SZ(g);
    dead.resize(n);
    parent.resize(n);
    sz.resize(n);
    level.resize(n);
    dist.resize(n, vector<int>(lgn));
    int root = play(1, -1, level, parent, g, dist);
  }
} T;
vi level, parent;
vector<vi> dist, g;
vector<vi> fw[2], reach, reach_parent;
int order[N][3];
void add(vi &f, int idx) { // fenwick tree update
  int n = SZ(f), i;
  for (i = idx; i < n; i += (i & -i))
    ++f[i];
}
int get(vi &f, int idx) { // fenwick query
  int n = SZ(f), i, sum = 0;
  for (i = idx; i > 0; i -= (i & -i))
    sum += f[i];
  return sum;
}
void update(vi &v, vi &f, int k) {
  int idx = upper_bound(all(v), k) - v.begin();
  add(f, idx);
}
int query(vi &v, vi &f, int k) {
  int idx = upper_bound(all(v), k) - v.begin();
  return get(f, idx);
}
int main() {
  int n, q;
  scanf("%d %d", &n, &q);
  g.resize(n + 1);
  reach.resize(n + 1);
  reach_parent.resize(n + 1);
  fw[0].resize(n + 1);
  fw[1].resize(n + 1);
  for (int i = 1; i < n; ++i) {
    int a, b;
    scanf("%d %d", &a, &b);
    g[a].push_back(b);
    g[b].push_back(a);
  }
  T.init(level, parent, g, dist);
  for (int i = 1; i <= q; ++i) {
    int cur_dist, v, k, st;
    scanf("%d %d", &order[i][0], &order[i][1]);
    if (order[i][0] == 1) {
      scanf("%d", &order[i][2]);
      st = v = order[i][1];
      k = order[i][2];
      cur_dist = 0;
      while (1) {
        reach[v].push_back(-(k - cur_dist));
        if (parent[v] == -1)
          break;
        cur_dist = dist[st][level[parent[v]]];
        reach_parent[v].push_back(-(k - cur_dist));
        v = parent[v];
      }
    }
  }
  // offline
  for (int i = 1; i <= n; ++i) {
    make_unique(reach[i]);
    make_unique(reach_parent[i]);
    fw[0][i].resize(SZ(reach[i]) + 1);
    fw[1][i].resize(SZ(reach_parent[i]) + 1);
  }
  // sweepline
  for (int i = 1; i <= n; ++i) {
    int cur_dist, v, k, st;
    if (order[i][0] == 1) {
      // update
      st = v = order[i][1];
      k = order[i][2];
      cur_dist = 0;
      while (1) {
        update(reach[v], fw[0][v], -(k - cur_dist));
        if (parent[v] == -1)
          break;
        cur_dist = dist[st][level[parent[v]]];
        update(reach_parent[v], fw[1][v], -(k - cur_dist));
        v = parent[v];
      }
    } else {
      st = v = order[i][1];
      int ans = 0;
      cur_dist = 0;
      while (1) {
        ans += query(reach[v], fw[0][v], -cur_dist);
        if (parent[v] == -1)
          break;
        cur_dist = dist[st][level[parent[v]]];
        ans -= query(reach_parent[v], fw[1][v], -cur_dist);
        v = parent[v];
      }
      printf("%d\n", ans);
    }
  }
  return 0;
}
```
