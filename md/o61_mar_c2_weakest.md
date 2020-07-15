โจทย์ข้อนี้สามารถสรุปใจความได้ดังนี้

กำหนด weighted, rooted tree ที่ประกอบไปด้วย $N$ node มาให้ ให้ตอบ $Q$ คำถามโดยแต่ละคำถามจะกำหนด node ให้ 2 node คือ $u$ และ $v$ แล้วให้หาน้ำหนักของเส้นที่มีค่าน้อยที่สุดบนเส้นทางจาก $u$ ไป $v$

เราจะได้รับคำถามแบบ Online Query นั่นคือ จะต้องตอบคำถามที่ได้รับมาให้เสร็จก่อนจึงจะพิจารณาคำถามถัดไปได้

เราสามารถแก้โจทย์ข้อนี้ได้โดยการดัดแปลงเทคนิค [**Binary Lifting**](https://www.geeksforgeeks.org/lca-in-a-tree-using-binary-lifting-technique/) ซึ่งเดิมมีไว้สำหรับหา [Lowest Common Ancestor](https://en.wikipedia.org/wiki/Lowest_common_ancestor) (node ร่วมกันที่ใกล้ที่สุดของ 2 node ที่กำหนดให้) สามารถทำได้ดังนี้

ก่อนอื่น จะต้องทำการ precompute ค่า $par[u][i]$ โดยมีนิยามเป็น ancestor ของ node $u$ ลำดับที่ $2^i$ (เว้นแต่ว่า ancestor ลำดับที่ $2^i$ ไม่มีอยู่จริง กำหนดให้ $par[u][i]$ เป็น root ของต้นไม้)
- $par[u][0]$ จะเท่ากับ parent โดยตรงของ $u$
- สำหรับ $i>0$ จะได้ $par[u][i] = par[par[u][i-1][i-1]$ นั่นคือ หากต้องการหา ancestor ลำดับที่ $2^i$ สามารถหาได้โดยกระโดดจาก $u$ ไป ancestor ลำดับที่ $2^{i-1}$ ก่อน แล้วจากตรงนั้นให้กระโดดหา ancestor ลำดับที่ $2^{i-1}$ ของตัวนั้น

โดยเราจำเป็นต้องคำนวณแค่ $i=0,1,2,\dots,17$ เท่านั้น เพราะ $\log_2 (100\,000) \leq 17$

นอกจากนี้จะ precompute ค่า $mnv[u][i]$ ด้วย โดยนิยามให้เท่ากับเส้นที่มีค่าน้อยที่สุดบนเส้นทางจาก node $u$ ไปยัง $par[u][i]$
- $mnv[u][0]$ จะเท่ากับน้ำหนักของเส้นเชื่อมไปยัง parent โดยตรงของ $u$
- $mnv[u][i] = \min\{mnv[u][i-1], mnv[par[u][i-1]][i-1]\}$

และ precompute ค่า $dep[u]$ โดยนิยามให้เท่ากับจำนวนเส้นเชื่อมระหว่าง root ของต้นไม้มาถึง node $u$

เมื่อทำเช่นนี้แล้ว การตอบคำถามแต่ละคำถามสามารถทำได้ด้วยขั้นตอนดังนี้

- ถ้า $u = v$ ตอบ $0$ ได้เลย (ตามโจทย์กำหนด)
- ถ้า $u$ และ $v$ อยู่ระดับความลึกต่างกัน (ดูจาก $dep$) ให้**ยก (lift)** ตัวที่อยู่ลึกกว่าขึ้นมาจนอยู่ในระดับความลึกเดียวกับอีกตัวหนึ่ง โดยคำว่ายกในที่นี้หมายถึง ให้หา node ที่เป็น ancestor ที่อยู่ในระดับความลึกที่ต้องการแล้วสนใจ node นั้นแทน node $u$ หรือ $v$ เริ่มต้น
    - การยกในที่นี้สามารถทำด้วยวิธีคล้ายกับ binary exponentiation คือ หากต้องการยกให้สูงขึ้นมา $21$ ชั้น สามารถทำได้โดยยกขึ้นมา $2^4$ ชั้นก่อน ตามด้วย $2^2$ ขั้น แล้ว $2^0$ ชั้น (ใช้ $par$ ที่เรา precompute ไว้ในการหา ancestor ลำดับดังกล่าว)
- เมื่อได้ $u$ และ $v$ อยู่ในระดับเดียวกันแล้ว ถ้า $u$ และ $v$ ยังไม่ใช่ node เดียวกัน ให้พยายามยกขึ้นมาจนเป็น node เดียวกันที่ใกล้ที่สุด (lowest common ancestor) ด้วยวิธีคล้าย ๆ binary search
    - ลองยก $2^{17}$ ชั้น ก่อน ถ้ายกมาแล้วเท่ากัน แปลว่าเราอาจจะเผลอยกมากเกิน lowest common ancestor ดังนั้นเราจะไม่ยก แต่ถ้าไม่เท่ากัน แสดงว่าทำได้ ให้ยกขึ้นมาเลย แล้วพิจารณา $2^{16}, 2^{15}, \dots, 2^0$ ต่อ
    - เมื่อทำจนจบ เราจะได้ $u$ และ $v$ เป็น node ที่มี parent เป็น lowest common ancestor เหมือนกัน ดังนั้นเพียงแค่ยกเพิ่มขึ้นมาอีก 1 ขั้นก็จะได้ $u$ และ $v$ เป็น node เดียวกัน

ระหว่างการยก $u$ และ/หรือ $v$ ขึ้นมา เราจะเก็บน้ำหนักของ edge ที่เบาที่สุดที่เดินทางผ่านไว้เสมอ (จาก $mnv$ ที่ precompute ไว้) เพื่อนำมาตอบคำถาม

สามารถทำความเข้าใจขั้นตอนวิธีดังกล่าวได้จากโค้ดด้านล่าง
 
## Implementation

ในภาษา C++:

```cpp
#include <bits/stdc++.h>
using namespace std;

const int INF = 1e9;
const int N = 100010;
const int LN = 18;

int par[N][LN], mnv[N][LN], dep[N];

int main() {
  int n;
  scanf("%d", &n);
  for (int i = 0; i < LN; ++i)
    mnv[0][i] = INF;
  for (int u = 1; u <= n - 1; ++u) {
    int p, w;
    scanf("%d%d", &p, &w);
    par[u][0] = p;
    dep[u] = dep[p] + 1;
    mnv[u][0] = w;
  }
  for (int i = 1; i < LN; ++i) {
    for (int u = 0; u < n; ++u) {
      par[u][i] = par[par[u][i - 1]][i - 1];
      mnv[u][i] = min(mnv[u][i - 1], mnv[par[u][i - 1]][i - 1]);
    }
  }
  int q, k, m, u, v;
  scanf("%d%d%d%d%d", &q, &k, &m, &u, &v);
  while (q--) {

    int u0 = u;
    int v0 = v;
    int ans = INF;

    if (u != v) {
      if (dep[u] > dep[v])
        swap(u, v);
      for (int i = LN - 1; i >= 0; --i) {
        int nv = par[v][i];
        if (dep[nv] >= dep[u]) {
          ans = min(ans, mnv[v][i]);
          v = nv;
        }
      }
      if (u != v) {
        for (int i = LN - 1; i >= 0; --i) {
          int nu = par[u][i];
          int nv = par[v][i];
          if (nu != nv) {
            ans = min(ans, mnv[u][i]);
            ans = min(ans, mnv[v][i]);
            u = nu;
            v = nv;
          }
        }
        ans = min(ans, mnv[u][0]);
        ans = min(ans, mnv[v][0]);
        u = par[u][0];
        v = par[v][0];
      }
    } else {
      ans = 0;
    }
    printf("%d\n", ans);

    int a3 = (v0 * 1LL * k + ans) % m % n;
    u = v0;
    v = a3;
  }

  return 0;
}
```
