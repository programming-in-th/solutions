พิจารณาผังเมืองเป็น Graph ประเภท Tree ที่ประกอบไปด้วยโหนด $N$ โหนดและเส้นเชื่อม $N-1$ เส้น

### Subtask 1 ( $N\leq 200$ )

สังเกตว่าในแต่ละคู่โหนด $(x,y)$ ที่ถูกเลือก จะสามารถแบ่งการเดินทางออกได้เป็นสามช่วงคือ

1. เดินจาก U ไปยัง x ผ่านถนนธรรมดา

2. เดินจาก x ไปยัง y ผ่านถนนพระราชทาน

3. เดินจาก y ไปยัง V ผ่านถนนธรรมดา

ซึ่งในช่วงที่หนึ่งและสามเป็น Path จากโหนดหนึ่งไปยังอีกโหนดหนึ่งบนเส้นของต้นไม้ (Tree) และช่วงที่สองเป็นการเดินผ่านเส้นเชื่อม (Edge) หนึ่งเส้น วิธีการต่อไปนี้ จะใช้ข้อสังเกตที่ว่า เส้นทางจาก $U$ ไป $x$ และ $y$ ไป $V$ มีเพียงหนึ่งเส้นทางเท่านั้นจากสมบัติของ Tree โดยเราจะตรวจสอบว่าวงเล็บของช่วงที่หนึ่งรวมกับช่วงที่สามสมดุลหรือไม่

ในการจัดเก็บค่าของวงเล็บ สามารถทำโดยการแปลงลำดับวงเล็บเป็นลำดับตัวเลข โดยให้ $+1$ แทน วงเล็บเปิด และ $-1$ แทน วงเล็บ
ปิด

เช่น (())() ได้เป็น $+1 +1 -1 -1 +1 -1$

โดยสังเกตว่าลำดับวงเล็บที่สมดุลนั้นจะมีคุณสมบัติคือ

1. จะต้องมีวงเล็บเปิดกับวงเล็บปิดที่สามารถเลือกมาคู่กันได้ กล่าวคือ ผลรวมของลำดับตัวเลขจะต้องท่ากับศูนย์

2. เมื่อพิจารณาลำดับวงเล็บจากซ้ายไปขวา ณ ตำแหน่งใด ๆ จะต้องมีจำนวนวงเล็บเปิดมากกว่าหรือเท่ากับวงเล็บปิดเสมอ กล่าวคือ ทุก Prefix Sum ของลำดับตัวเลขจะต้องมากกว่าเท่ากับศูนย์

จะได้ว่า ใน Subtask นี้จึงสามารถไล่ทุกคู่โหนด $(x,y)$ และทำ Graph Traversal (DFS/BFS) จากโหนด $U$ ไป $x$ แล้วเก็บผลรวมของลำดับวงเล็บไว้ เพื่อใช้คำนวณต่อเมื่อทำ Graph Traversal จากโหนด $y$ ไป $V$ โดยระหว่างการทำ Graph Tranversal ทั้งสองรอบ ให้ตรวจสอบว่า Prefix Sum ปัจจุบันที่ได้นั้น น้อยกว่าศูนย์หรือไม่ และตรวจสอบตอนท้ายว่าผลรวมทั้งหมดเป็นศูนย์หรือไม่ จะได้คำตอบเป็น จำนวณคู่โหนด $(x,y)$ ที่ได้ผลรวมทั้งหมดเป็นศูนย์ และไม่มี Prefix Sum ใดที่น้อยกว่าศูนย์เลย ตรงตามเงื่อนไขข้างต้น

Time Complexity: $\mathcal{O}(N^3)$

### Subtask 2 ( $N\leq 1000$ )

สังเกตว่าสามารถลดเวลาการทำงานของวิธีการใน Subtask ที่แล้วได้ โดยการเช็คว่ามีเส้นทาง $U\to x$ ที่สามารถ $y\to V$ กี่เส้น ซึ่งทำได้โดยการตรวจสอบว่าผลรวมของทั้งสองลำดับจะต้องเท่ากับศูนย์ และผลรวมของลำดับแรก ($U\to x$) จะต้องมากกว่าหรือเท่ากับ Prefix Sum ที่ต่ำที่สุดของลำดับสอง ($y\to V$ ) กล่าวคือ

ผลรวมของลำดับวงเล็บ $U\to x$ ต้องมากกว่าเท่ากับ $\min\{$Prefix Sum ของ $y\to V\}$

จึงสามารถทำได้โดยการทำ Graph Traversal $N$ ครั้งจากทุกโหนดไปยังทุกโหนดอื่น ๆ และเก็บผลรวมกับ Prefix Sum ที่มีค่าน้อยที่สุดของแต่ละคู่โหนด $(i,j)$ ไว้

หลังจากนั้น นับจำนวณคู่โหนด $(x,y)$ ที่ $(U,x)$ และ $(y,V)$ ผลรวมกับ Prefix Sum ที่มีค่าน้อยที่สุดตรงตามเงื่อนไขข้างต้น

Time Complexity: $\mathcal{O}(N^2)$

### Subtask 3 ( ไม่มีเงื่อนไขเพิ่มเติม )

เราสามารถดัดแปลงลำดับ ให้ไม่ต้องเช็ค Prefix Sum ที่มีค่าน้อยที่สุดระหว่างทั้งสองลำดับวงเล็บได้ โดยการกลับค่าและกลับฝั่งของลำดับหลัง แล้วตรวจสอบเหมือนวิธีที่ใช้ในลำดับแรก ดังนี้

ทำการสลับวงเล็บจากวงเล็บเปิดเป็นวงเล็บปิด และวงเล็บปิดเป็นวงเล็บเปิด หลังจากนั้นก็กลับข้างลำดับ เช่น

$\dots ()(()))$ กลายเป็น $((())()\dots$

เนื่องจากลำดับวงเล็บที่สมดุลนั้น จะสมดุลทั้งเมื่ออ่านจากซ้ายไปขวา หรืออ่านจากขวาไปซ้ายแล้วสลับวงเล็บเปิด/วงเล็บปิด

จึงทำให้สามารถตรวจสอบแค่ ผลรวมของทั้งสองลำดับ และตรวจสอบ Prefix Sum ของลำดับแรก และลำดับสองที่ถูกแปลงแล้ว

ซึ่งสามารถทำได้โดยการทำ Graph Traversal

1. จากโหนด U ไปยังทุกโหนด และเก็บผลรวมกับเช็ค Prefix Sum

2. จากโหนด V ไปยังทุกโหนด โดยคำนวณในแบบที่ลำดับถูกดัดแปลงแล้ว และเก็บผลรวมกับเช็ค Prefix Sum บนลำดับที่ถูก
แปลง

หลังจากนั้น นับจำนวณคู่โหนด $(x,y)$ ที่ผ่านเงื่อนไขและมีผลรวมที่รวมกันได้พอดี (ถ้าเป็นลำดับแรกคำนวณแบบปกติ กับลำดับที่สองคำนวณแบบดัดแปลงแล้ว กล่าวคือ มีค่าผลรวมที่เท่ากันนั่นเอง)

ซึ่งสามารถทำได้โดยอาจจะใช้ Data Structure เช่น `std::map` ในการเก็บและนับ

Time Complexity: $\mathcal{O}(N\log N)$

หมายเหตุ: สามารถเก็บโดยใช้ อาเรย์ และให้ index ของช่องอาเรย์เป็นค่าผลรวม และให้ค่าในอาเรย์เก็บจำนวนโหนด ก็จะทำให้สามารถเพิ่มประสิทธิภาพของโปรแกรมให้ใช้ Time Complexity เหลือ $\mathcal{O}(N)$ ได้

### Solution Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 1;

int n, u, v;
vector<int> adj[N];
string s;
int val[N], sum[N][2], mn[N][2];
vector<int> st[N << 1];
long long ans;

void dfs(int u, int p, int t) {
  sum[u][t] = sum[p][t] + val[u];
  if (!t)
    mn[u][t] = min(mn[p][t], sum[u][t]);
  else
    mn[u][t] = min(0, mn[p][t] + val[u]);
  for (int v : adj[u])
    if (v != p) dfs(v, u, t);
}

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  cin >> n >> u >> v;
  for (int i = 0; i < n - 1; i++) {
    int a, b;
    cin >> a >> b;
    adj[a].push_back(b);
    adj[b].push_back(a);
  }
  cin >> s;
  for (int i = 1; i <= n; i++)
    if (s[i - 1] == '(')
      val[i] = 1;
    else
      val[i] = -1;
  dfs(u, 0, 0);
  dfs(v, 0, 1);
  for (int i = 1; i <= n; i++) st[sum[i][1] + N].push_back(abs(mn[i][1]));
  for (int i = 0; i < (N << 1); i++) sort(st[i].begin(), st[i].end());
  for (int i = 1; i <= n; i++)
    if (mn[i][0] >= 0) {
      auto it = upper_bound(st[-sum[i][0] + N].begin(),
                            st[-sum[i][0] + N].end(), sum[i][0]);
      ans += (int)(it - st[-sum[i][0] + N].begin());
    }
  cout << ans;
}
```