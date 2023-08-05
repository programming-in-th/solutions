สังเกตว่าเราจะสามารถทุบตัวตุ่นตัวที่ $j$ ต่อจาก $i$ ก็ต่อเมื่อ $|s_j-s_i|\leq t_j-t_i$ เมื่อ $t_i\leq t_j$

### Subtask 1 ( $|s_{i+1}-s_i|\leq t_{i+1}-t_i$ และ $t_i\leq t_{i+1}$ สำหรับ $1\leq i<N$ )

จากเงื่อนไขของ Subtask นี้ ทำให้สามารถทุบตัวตุ่นทุกตัวตามลำดับ $1,2,3,\dots,N$ ได้เสมอ ทำให้คำตอบคือค่า $N$

Time Complexity: $\mathcal{O}(1)$

### Subtask 2 ( $s_i< s_{i+1},t_i< t_{i+1}$ สำหรับ $1\leq i< N$ และ $N\leq 5000$ )

จากเงื่อนไขของ Subtask สังเกตได้ว่าลำดับของตัวตุ่นที่โดนทุบจะเป็นลำดับย่อยของตัวตุ่นตัวที่ $1,2,3,\dots,N$ เนื่องจากตัวตุ่นตัวที่ $i+1$ จะโผล่มาหลังตัวที่ $i$ ดังนั้นเราสามารถใช้ Dynamic Programming ในการแก้ Subtask นี้ได้

กำหนดให้ $dp(i)$ เป็นจำนวนตัวตุ่นโดนทุบมากที่สุดที่เป็นไปได้เมื่อพิจารณาจากตัวที่ $1$ ถึงตัวที่ $i$ และทุบตัวที่ $i$ ด้วย

Base Case:

$dp(i)=1 \quad |s_i-s_0|\leq t_i$ (สามารถทุบตัวตุ่นตัวนี้เป็นตัวแรกได้)

$dp(i)=0 \quad$ หากไม่เป็นไปตามเงื่อนไขด้านบน

Recurrence Formula:

$$dp(i)=\max\limits_{1\leq j<i}\{dp(j)\}+1$$

เมื่อ $dp(j)\neq 0$ (รองรับกรณีที่ตัวตุ่นไม่สามารถโดนทุบได้เป็นตัวแรก) และ $s_j-s_i\leq t_j-t_i$ ไม่จำเป็นต้องใช้ค่าสัมบูรณ์เนื่องจากเงื่อนไขของ Subtask

คำตอบข้อนี้เป็น $\max\limits_{1\leq i\leq N}\{dp(i)\}$

Time Complexity: $\mathcal{O}(N^2)$

### Subtask 3 ( $N\leq 5000$ )

สำหรับ Subtask นี้ ใช้วิธีแก้คล้ายกันกับ Subtask 2 โดยเพื่อทำให้วิธีการแก้ Subtask นี้ง่ายขึ้น เราจะสมมติตัวตุ่นตัวที่ $0$ ตำแหน่ง $s_0$ ณ เวลา $t=0$ แทนตำแหน่งเริ่มต้นของเรา จากนั้น เรียงตัวตุ่นตามเวลา $t_i$ จากน้อยไปมาก แล้วทำ Dynamic Programming ดังนี้

Base Case:

$dp(0)=1$ (เริ่มที่ตัวตุ่นสมมติจากที่นิยามไว้ข้างต้น)

Recurrence Formula:

$dp(i)=\max\limits_{1\leq j<i}\{dp(j)\}+1$

เมื่อ $dp(j)\neq 0$ (รองรับกรณีที่ตัวตุ่นไม่สามารถโดนทุบได้เป็นตัวแรก) และ $|s_j-s_i|\leq t_j-t_i$ ไม่จำเป็นต้องใช้ค่าสัมบูรณ์ของ $t_j-t_i$ เนื่องจากเราได้เรียง $t_i$ จากน้อยไปมากแล้ว แต่ยังคงต้องใช้ค่าสัมบูรณ์ของ $s_j-s_i$ เพราะ Subtask นี้ไม่ได้รับประกันว่า $s_j>s_i$

คำตอบข้อนี้เป็น $\max\limits_{1\leq i\leq N}\{dp(i)\}$

Time Complexity: $\mathcal{O}(N^2)$

### Subtask 4 ( ไม่มีเงื่อนไขเพิ่มเติม )

พิจารณาเงื่อนไข $|s_j-s_i|\leq t_j-t_i$ จะได้ว่า $t_i-t_j\leq s_j-s_i\leq t_j-t_i$ หรือ $t_i-t_j\leq s_j-s_i$ และ $s_j-s_i\leq t_j-t_i$

เราสามารถเขียนอสมการ 2 อสมการดังกล่าวในรูปต่อไปนี้: $t_i+s_i\leq t_j+s_j$ และ $t_i-s_i\leq t_j-s_j$ ตามลำดับ สังเกตว่าหากทั้งสองเงื่อนไขนี้เป็นจริง เราจะสามารถทุบตัวตุ่นตัวที่ $j$ หลังจาก $i$ ทันทีได้ นอกจากนี้ หาก $t_j<t_i$ อสมการดังกล่าวจะเป็นเท็จเสมอ (เนื่องจาก $|s_j-s_i
|\geq 0$)

ดังนั้น เราจะเรียงตัวตุ่นตามค่า $t_i+s_i$ จากน้อยไปมาก ทำให้สำหรับตัวตุ่นตัวที่ $i,j$ ใด ๆ ที่ $i<j,t_i+s_i\leq t_j+s_j$ เสมอ จากนั้น เราจะหาลำดับย่อยของตัวตุ่นที่ยาวที่สุด ที่มีเงื่อนไขเป็น $t_i-s_i\leq t_j-s_j$ สำหรับตัวตุ่นตัวที่ $i<j$ ในลำดับย่อยนี้ นอกจากนี้เราจะไม่สนใจตัวตุ่นที่ไม่สามารถโดนทุบเป็นตัวแรกได้เช่นเดิม

การหาลำดับย่อยดังกล่าว เป็นการหา Longest Non-decreasing Subsequence สามารถหาได้ภายในเวลา $\mathcal{O}(N\log N)$ ด้วยวิธี
Greedy Algorithm

Time Complexity: $\mathcal{O}(N\log N)$

### Solution Code:

```cpp
#include <bits/stdc++.h>

#define pii pair<int, int>
#define x first
#define y second

using namespace std;

const int N = 1e6 + 5;

int n, st;
int dp[N];
pii A[N];

int main() {
    scanf("%d %d", &n, &st);
    for (int i = 1; i <= n; i++) {
        scanf("%d %d", &A[i].x, &A[i].y);
        int a = A[i].y - A[i].x;
        int b = A[i].y + A[i].x; 
        A[i] = pii(a, b);
    }
    sort(A + 1, A + n + 1);
    vector<int> lis;
    for (int i = 1; i <= n; i++) {
        if (A[i].x < -st || A[i].y < st) continue;
        if (lis.empty() || lis.back() <= A[i].y) {
            lis.emplace_back(A[i].y);
            continue;
        }
        auto it = upper_bound(lis.begin(), lis.end(), A[i].y);
        if (it == lis.end()) continue;
        *it = A[i].y;
    }
    printf("%d\n", (int) lis.size());

    return 0;
}
```