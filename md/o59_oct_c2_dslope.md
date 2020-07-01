โจทย์ต้องการให้เราหา *ลำดับลด (Decreasing Subsequence)* ที่มีความยาว $k$ จากลำดับความยาว $n$ ที่กำหนดมาให้

## Dynamic Programming

**นิยาม** $dp_{i,j,l}$ = จำนวน *ลำดับลด ( Decreasing Subsequence )* ที่มีความยาว $j$ ตัวและลงท้ายด้วย $l$ ของลำดับ $i$ ตัวแรก  

สมมุติว่าปัจจุบัน เรากำลังพิจารณาลำดับ $i+1$ ตัวแรก การ *transition* จาก $dp$ ของลำดับ $i$ ตัวแรก สามารถทำได้โดยการเพิ่มจำนวนวิธีที่เรานำ $a_{i+1}$ มาต่อท้ายลำดับลดก่อนหน้านี้ทุกลำดับที่ลงท้ายด้วยตัวเลขที่มีค่ามากกว่า $a_{i+1}$ เข้าไปใน $dp$ ของลำดับ $i$ ตัวแรก หรือก็คือ  

$dp_{i+1,j,l} = 
\begin{cases} 
	1 & \text{if } j = 1 \text{ and } l = a_{i+1}\\
	dp_{i,j,l} + \sum_{m=a_{i+1}+1}^{n} dp_{i,j-1,m} & \text{otherwise}
\end{cases}$  

คำตอบของปัญหาที่เราต้องการ คือ ลำดับลดความยาว $k$ หรือก็คือ $\sum_{m=1}^{n} dp_{n,m,k}$ นั่นเอง เราสามารถคำนวณคำตอบของปัญหาด้วยวิธีนี้ได้ในเวลา $\mathcal{O}(n^3k)$  

## Fenwick Tree

สังเกตุว่าวิธีที่กล่าวมามีปัญหาคือเราต้องพิจารณาทุก $l$ ที่ $l > a_{i+1}$ ซึ่งใช้เวลาได้มากถึง $\mathcal{O}(n)$ 

หากเราเปลี่ยนตาราง $dp$ ให้เป็นโครงสร้างของต้น *Fenwick Tree* ได้ **เราจะสามารถหาลำดับลดทุกลำดับที่ลงท้ายด้วยตัวเลขที่มีค่ามากกว่า $a_{i+1}$ ได้ในเวลา $\mathcal{O}(\log n)$** โดยที่เราไม่ต้องพิจารณาทุก $l$  

ปัญหาต่อมาคือ เราต้องนำจำนวนวิธีของลำดับทั้งหมดใน $dp_{i}$ มายัง $dp_{i+1}$ ด้วย แต่เราสามารถแก้ปัญหานี้ได้ด้วยการใช้ **ตาราง $dp$ เดิม** ไปเลย  

Time Complexity : $\mathcal{O}(nk\log n)$  
Space Complexity : $\mathcal{O}(nk)$  

Code :

```cpp
#include <bits/stdc++.h>
using namespace std;
const int mod = 999999999;
const int N = 80003, K = 42;
int mul(int a, int b) { return a * 1ll * b % mod; }
int add(int a, int b) {
  int c = a + b;
  if (c >= mod)
    return c - mod;
  return c;
}
int dp[K][N], n;
void update(int k, int l, int val) { // fenwick(dp) update
  for (int i = l; i <= n; i += (i & -i))
    dp[k][i] = add(dp[k][i], val);
}
int query(int k, int l) { // fenwick(dp) query
  int sum = 0;
  for (int i = l; i > 0; i -= (i & -i))
    sum = add(sum, dp[k][i]);
  return sum;
}
int a[N];
int main() {
  int k;
  scanf("%d %d", &n, &k);
  for (int i = 1; i <= n; ++i)
    scanf("%d", &a[i]);
  // transition
  for (int i = 1; i <= n; ++i) {
    for (int j = k; j >= 2; --j) {
      update(j, a[i], add(query(j - 1, n), mod - query(j - 1, a[i])));
    }
    update(1, a[i], 1);
  }
  printf("%d\n", query(k, n));
  return 0;
}
```
