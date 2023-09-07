### Subtask 1, 2 (15 + 16 คะแนน) $N,M\leq 50$ และ $R[i],C[i]\leq 500$

เราสามารถลองทุกค่า $D$ ที่เป็นไปได้และหาค่า $D$ ที่น้อยที่สุดที่สามารถรดน้ำต้นมะม่วงได้ครบตามเงื่อนไข และสำหรับแต่ละค่า $D$ เราจะตรวจสอบ**ทุกคู่ของสปริงเกอร์และต้นมะม่วง**ว่าต้นมะม่วงอยู่ในช่วงที่สปริงเกอร์รดน้ำต้นไม้ถึงหรือไม่ ซึ่งจะใช้เวลา $\mathcal{O}(MN)$ สำหรับการตรวจสอบแต่ละค่า $D$

Time Complexity: $\mathcal{O}(NMD);$ $D=\max(R,C)$

### Subtask 3 (18 คะแนน) $N,M\leq 1000$ และ $R[i],C[i]\leq 500$

สมมติให้ $D_{ans}$ คือค่า $D$ ที่เป็นระยะสั้นที่สุดที่สามารถรดน้ำต้นมะม่วงได้ครบตามเงื่อนไข(คำตอบที่ดีที่สุด) เราจะสังเกตได้ว่าถ้าหากเราลองค่า $D$ ใดๆที่มีค่ามากกว่าค่า $D_{ans}$ เราจะสามารถรดน้ำต้นมะม่วงได้ครบเสมอ และถ้าหากเราลองค่า $D$ ใดๆที่น้อยกว่าค่า $D_{ans}$ เราจะไม่สามารถรดน้ำต้นมะม่วงได้ครบตามเงื่อนไขเสมอ จากข้อสังเกตนี้ทำให้เราสามารถทำ Binary Search บนค่า $D$ ได้ โดยการหาค่า $D$ ที่น้อยที่สุดที่จะยังสามารถรดน้ำต้นมะม่วงได้ครบตามเงื่อนไข ซึ่งจะใช้เวลา $\mathcal{O}(\log D)$ และเราสามารถตรวจสอบว่าสามารถรดน้ำต้นมะม่วงได้ครบหรือไม่โดยใช้วิธีเดียวกับ subtask ที่ 2

Time Complexity: $\mathcal{O}(NM\log D);$ $D=\max(R,C)$

### Official Solution

จากใน subtask ที่ 3 เรายังคง Binary Search หาค่า $D$ เช่นเดิม แต่เราจำเป็นที่จะต้องหาวิธีที่จะตรวจสอบว่า "ต้นมะม่วงได้รับน้ำจากสปริงเกอร์เพียงพอหรือไม่" ให้เร็วมากยิ่งขึ้น ซึ่งหนึ่งในวิธีที่เป็นไปได้คือการใช้ Quicksum หรือ Sweepline 2D เพื่อใช้ในการหาว่าสำหรับแต่ละตำแหน่ง $(r,c)$ ใดๆ จะมีสปริงเกอร์ทั้งหมดกี่อันที่รดน้ำถึง แล้วตรวจสอบว่าตำแหน่งของต้นไม้แต่ละต้นนั้นได้รับน้ำเพียงพอหรือไม่ ซึ่งแนวคิดนี้จะใช้เวลา $\mathcal{O}(RC\log D)$ อย่างไรก็ดีเราสามารถปรับปรุงแนวคิดให้ดียิ่งขึ้นได้จากการเปลี่ยนมุมมองจาก "สปริงเกอร์สามารถรดน้ำต้นไม้ได้ในช่วงระยะ $D$" เป็น "ในระยะ $D$ รอบๆต้นไม้จะมีสปริงเกอร์อยู่กี่อัน" ทำให้สามารถทำ Quicksum 2D เก็บจำนวนสปริงเกอร์ไว้ล่วงหน้าและเรียกถามจำนวนสปริงเกอร์ทั้งหมดในช่วง $(r_1,c_1)$ ถึง $(r_2,c_2)$ ใดๆ ในเวลา $\mathcal{O}(1)$ หลังจากนั้นสำหรับแต่ละ $D$ เราจะไล่ตรวจสอบต้นไม้แต่ละต้นว่าในระยะ $D$ รอบๆต้นไม้นั้นมีสปริงเกอร์อยู่มากกว่าเท่ากับ $w[i]$ หรือไม่

Time Complexity: $\mathcal{O}(RC+N\log D);$ $D=\max(R,C)$

### Solution Code

```cpp
#include <bits/stdc++.h>
using namespace std;
int qs[5005][5005];
vector<array<int, 3>> v;
int main(int argc, char* argv[]){
  int N, M, R, C;
  scanf("%d %d %d %d", &N, &M, &R, &C);
  for(int i = 1, r, c, w; i <= N; i++){
    scanf("%d %d %d", &r, &c, &w);
    v.push_back({r, c, w});
  }
  for(int i = 1, x, y; i <= M; i++){
    scanf("%d %d", &x, &y); qs[x][y]++;
  }
  for(int i = 1; i <= R; i++){
    for(int j = 1; j <= C; j++){
      qs[i][j] += qs[i - 1][j] + qs[i][j - 1] - qs[i - 1][j - 1];
    }
  }

  int l = 1, r = max(R, C);
  while(l < r){
    int mid = (l + r) >> 1;
    bool ok = true;
    for(auto [x, y, w] : v){
      int x1 = max(0, x - mid - 1), y1 = max(0, y - mid - 1);
      int x2 = min(R, x + mid), y2 = min(C, y + mid);
      if(qs[x2][y2] - qs[x2][y1] - qs[x1][y2] + qs[x1][y1] < w){
        ok = false;
        break;
      }
    }
    if(ok) r = mid;
    else l = mid + 1;
  }
  printf("%d\n", l);
  return 0;
}
```