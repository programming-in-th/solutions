### Subtask 1 ( $W=0$ )

สังเกตุว่าการกระโดดของจิงโจ้่ในกรณีนี้จะเกิดขึ้นได้ก็ต่อเมื่อจิงโจ้เริ่มยืนติดกำแพง ซึ่งคือขอบทั้งสี่ขอบของตารางเท่านั้น จึงมีความเป็น
ไปได้แค่ 3 กรณีเท่านั้นคือ

1. จิงโจ้อยู่ที่มุมใดมุมหนึ่งใน 4 มุมของห้อง จิงโจ้จะสามารถกระโดดไปตำแหน่งใดๆในห้องโดยการกระโดด 1 ครั้งหากจุดหมายอยู่ใน
แถวหรือคอลลัมน๋์เดียวกับจุดเริ่มต้น และ 2 ครั้งไปตำแหน่งอื่นๆ ใดๆ

2. จิงโจ้อยู่ที่ขอบของห้องแต่ไม่อยู่ที่มุมของห้อง จิงโจ้่จะสามารถกระโดดไปจุดใดๆในแถวเดียวกันหากเป็นขอบซ้ายขวา และจุดใดๆใน
คอลลัมน์เดียวกันหากเป็นขอบบนล่าง โดยจะกระโดดเพียงแค่ครั้งเดียว นอกจากตำแหน่งดังกล่าวแล้ว จิงโจ้ไม่สามารถกระโดดไปตำแหน่ง
อื่นได้

3. ในกรณีอื่นๆ จิงโจ้ไม่สามารถกระโดดไปไหนได้เลย

Time Complexity: $\mathcal{O}(1)$

### Subtask 2 ( $N,M\leq 10^3$ และ $W\leq 10^4$ )

เนื่องจากขนาดของห้องในปัญหาย่อยนี้มีขนาดเล็กจึงสามารถมองห้องเป็นตาราง 2 มิติ ตาราง 2 มิติดังกล่าวจะมีขนาด $N\times M$ ช่อง ซึ่งรวมไม่เกิน $10^6$ ช่อง จิงโจ้ที่อยู่ที่จุดเริ่มต้นจะเคลื่อนที่ได้ก็ต่อเมื่อมันอยู่ติดกับกำแพง โดยอาจเคลื่อนที่ไปได้หลายจุด ปัญหานี้จึงสามารถแก้ได้ด้วยการทำ breadth-first search หรือ flood-filling บนตารางโดยเริ่มจากตำแหน่งที่จิงโจ้อยู่

สังเกตุว่าช่องที่จิงโจ้สามารถกระโดดไปทั้งหมดนั้น จะอยู่ในแถวหรือคอลลัมน์เดียวกับตำแหน่งของจิงโจ้เท่านั้น เพราะฉะนั้นจะมีตำแหน่งที่จิงโจ้กระโดดไปได้่ไม่เกิน N + M ตำแหน่งเท่านั้น ไม่ว่าจิงโจ้จะอยู่ที่ไหนก็ตาม
ในการทำ breadth-first search หรือ flood-filling โดยวนหาช่องที่ไปต่อในเวลา $\mathcal{O}(N+M)$ สำหรับทุกช่องในตาราง

Time Complexity: $\mathcal{O}(NM\cdot(N+M))$

### Subtask 3 ( $N,M\leq 10^5$ และ $W\leq 10^4$ )

ในปัญหาย่อยนี้จะไม่สามารถทำ Breadth-first Search แบบเดิมได้ แต่จะสังเกตุว่าจิงโจ้ไม่มีเหตุผลในการไปช่องที่ไม่ติดกำแพงที่ไม่ใช่จุดจบ เนื่องจากการไปช่องที่ไม่ใช่กำแพงที่ไม่ใช่จุดจบจะทำให้จิงโจ้ไม่สามารถกระโดดต่อไปได้ เนื่องจากขอบของห้องก็เป็นกำแพง และมีกำแพงอีก $W$ อัน จะมีช่องที่ต้องพิจารณาเพียงแค่ $2N+2M+4W-2$ ช่องเท่านั้น

ปัญหาย่อยนี้จึงสามารถแก้ได้โดยใช้ Breadth-first Search บนช่องที่ต้องพิจารณาจำนวน $2N+2M+4W-2$ ช่องเท่านั้น โดย
การหาว่าช่องถัดไปที่ไปได้สามารถใช้การ Binary Search ช่วยทำให้หาได้ทัน

Time complexity: $\mathcal{O}((N+M+W)\log_2(N+M+W))$

### Subtask 4 ( ไม่มีเงื่อนไขเพิ่มเติม )

ปัญหานี้สามารถแก้ได้หลายวิธี เช่นการ Optimize วิธีการในปัญหาย่อยก่อนหน้าให้ใช้เวลาน้อยลงด้วยวิธีต่างๆ 

อย่างไรก็ตาม ปัญหานี้สามารถแก้ได้ง่ายขึ้นเมื่อพิจารณากราฟย้อนทิศทาง และการเดินทางบนกราฟย้อนทิศทางตั้งแต่จุดปลายสุดกลับมาถึงจุดเริ่มต้น

เมื่อพิจารณากราฟย้อนทิศทาง จะพบว่าบนกราฟย้อนทิศทาง แต่ละโหนดจะมีเส้นเขื่อมออกไปไม่เกิน 4 เส้นเชื่อม นั่นคือไม่เกิน 2 เส้นเชื่อมในแถวเดียวกัน และไม่เกิน 2 เส้นเชื่อมในคอลลัมน์เดียวกัน

การหาเส้นที่เชื่อมจากโหนด ๆ หนึ่งจึงสามารถทำโดยการ Binary Search บนจำนวนกำแพงทั้งหมด W ตำแหน่ง ในแถว และคอลลัมน์เดียวกัน และจะมั่นใจได้ว่าการ Breadth-first Search จะค้นหาเฉพาะช่องที่ติดกับกำแพง หรือช่องเริ่มต้นและช่องสุดท้ายเท่านั้น จึงมีจำนวนช่องที่ต้องค้นหาแค่ไม่เกิน $\mathcal{O}(N+M+W)$ ช่อง

Time complexity: $\mathcal{O}((N+M+W)\log(N+M+W))$

### Solution Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
#define x first
#define y second
#define all(x) x.begin(), x.end()

map<int, vector<int> > wallx, wally;
map<pair<int, int>, int> vis;
queue<pair<int, pair<int, int> > > q;

int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int n, m, w;
  cin >> n >> m >> w;
  int sx, sy, ex, ey;
  cin >> sx >> sy >> ex >> ey;
  for (int i = 0; i < w; i++) {
    int x, y;
    cin >> x >> y;
    wallx[x].push_back(y);
    wally[y].push_back(x);
  }

  q.push({0, {ex, ey}});
  int ans = -1;
  while (!q.empty()) {
    auto t = q.front();
    q.pop();
    if (vis[t.y]) continue;
    vis[t.y] = 1;

    if (t.y == make_pair(sx, sy)) {
      ans = t.x;
      break;
    }

    int idx = upper_bound(all(wally[t.y.y]), t.y.x) - wally[t.y.y].begin();
    if (idx == wally[t.y.y].size())
      q.push({t.x + 1, {n, t.y.y}});
    else
      q.push({t.x + 1, {wally[t.y.y][idx] - 1, t.y.y}});
    if (idx == 0)
      q.push({t.x + 1, {1, t.y.y}});
    else
      q.push({t.x + 1, {wally[t.y.y][idx - 1] + 1, t.y.y}});

    int idy = upper_bound(all(wallx[t.y.x]), t.y.y) - wallx[t.y.x].begin();
    if (idy == wallx[t.y.x].size())
      q.push({t.x + 1, {t.y.x, m}});
    else
      q.push({t.x + 1, {t.y.x, wallx[t.y.x][idy] - 1}});
    if (idy == 0)
      q.push({t.x + 1, {t.y.x, 1}});
    else
      q.push({t.x + 1, {t.y.x, wallx[t.y.x][idy - 1] + 1}});
  }
  cout << ans << endl;
  return 0;
}
```