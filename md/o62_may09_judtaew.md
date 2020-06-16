ในข้อนี้ โจทย์ต้องการนับจำนวนวิธีการจัดแถวที่เป็นไปได้ เห็นได้ชัดเจนเลยว่าเรามีวิธี $\mathcal{O}(N^4)$ หรือ $\mathcal{O}(N^3)$ ตรงๆ โดยใช้ _Dynamic Programming_ โดยเริ่มจากการนิยาม `dp[i][j][k]` แทนว่าหากให้แถวแรกจบที่คนที่ $i$ แถวที่สองจบที่คนที่ $j$ และแถวที่สามจบที่คนที่ $k$ จะมีวิธีที่เป็นไปได้ทั้งหมดกี่วิธี

หากคิดแบบ bottom-up เราจะทราบว่าหากพิจารณา state `[i][j][k]` แล้ว เราสามารถนำ state ปัจจุบันไปอัพเดท state ถัดไปได้ ดังโค้ดตัวอย่างต่อไปนี้

```cpp
dp[0][0][0] = 1; // Initial state
for (int i = 0; i < n; i++) {
  for (int j = 0; j < n; j++) {
    for (int k = 0; k < n; k++) {
      // Consider (i,j,k) to be the head of current config.
      // The next person to be configure is max(i,j,k)+1.
      int nxt = max(i, j, k) + 1;
      // Check all possible configs after adding nxt to the lines
      // (i.e. determine if nxt can be after i, j, k or not)
      if (i == 0 || abs(h[nxt] - h[i]) <= lim)
        dp[nxt][j][k] += dp[i][j][k];
      if (j == 0 || abs(h[nxt] - h[j]) <= lim)
        dp[i][nxt][k] += dp[i][j][k];
      if (k == 0 || abs(h[nxt] - h[k]) <= lim)
        dp[i][j][nxt] += dp[i][j][k];
    }
  }
}
int ans = 0;
for (int i = 0; i < n; i++) {
  for (int j = 0; j < n; j++) {
    ans += dp[n][i][j];
    ans += dp[i][n][j];
    ans += dp[i][j][n];
  }
}
```

ซึ่งจะทำได้ใน $\mathcal{O}(N^3)$ แต่ยังไม่ผ่าน subtask 4 เพราะ memory มากเกินไป

ทั้งนี้ เราสามารถอาศัยหลักการที่ว่า state ที่เก็บมันซ้ำกันมาก จนสามารถลดลงได้เหลือเพียงเก็บเฉพาะ $i, j, k$ ที่ $i \geq j \geq k$ (เรารู้ว่าไม่ว่าสลับแถวอย่างไรก็ได้ค่าเท่ากัน) แล้วสุดท้ายเราก็สามารถหาคำตอบได้จากการเรียง $i, j, k$ แล้วคูณด้วย $6$ อยู่ดี จะใช้ memory ลดลงจาก $4N^3$ ไบต์เหลือเพียง $\frac{4N(N-1)(N-2)}{6}$ ไบต์เท่านั้น โดยเราสามารถประกาศอาเรย์หลายมิติ ที่มิติหลังแปรไปตามมิติหน้าได้ ด้วยเทคนิคดังนี้

```cpp
int **dp[512]; // dp[i] is a pointer that is pointing to a pointer
for (int i = 1; i <= n; i++) {
  dp[i] = new int *[i]; // dp[i] should be a 2D array of i rows
  for (int j = 0; j < i; j++) {
    dp[i][j] = new int[j]; // and row j should have j columns
    for (int k = 0; k < j; k++) {
      dp[i][j][k] = 0; // set zero
    }
  }
}
```

แล้วดำเนินการเฉพาะกรณี $i > j > k$ แล้วแยกกรณีพิเศษคือมีเพียงแถวเดียว $(i,0,0)$ ออกมา กรณีทั่วไปนำคำตอบคูณ $6$ เพื่อเรียงสับเปลี่ยนแถว กรณีพิเศษนำคำตอบคูณ $3$ เพื่อเรียงสับเปลี่ยนเพียงแถวเดียว เท่านี้จะสามารถผ่าน subtask 4 ได้

ต่อมาคือขั้นตอนสำคัญที่จะนำไปสู่การ optimize จาก $\mathcal{O}(N^3)$ เป็น $\widetilde{\mathcal{O}}(N^2)$ คือการสังเกตว่าการเก็บ state ที่เราคิดว่าน่าจะดีที่สุดแล้ว (เฉพาะ $i > j > k$) มันยังมีวิธีที่ดีกว่านี้อีก มันมีวิธีที่ resolve กรณีซ้ำซากได้ดีกว่านี้อีก

สมมติเรามี state $(i,j,k)$ อยู่ (เมื่อ $i > j > k$) แล้วต้องการเติมตัวใหม่ล่าสุดเข้าไป แน่นอนว่าจะต้องเติม $\max(i,j,k)$ ไปอย่างแน่นอน นั่นคือ เติม $i+1$ ไปแน่ๆ แต่จะเติมในแถวใด หากเติมในแถว $i$ state ใหม่จะเป็น $(i+1,j,k)$ หากเติมแถว $j$ state ใหม่จะเป็น $(i+1, i, k)$ และหากเติมแถว $k$ state ใหม่จะเป็น $(i+1, i, j)$ ดังภาพ

![img](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o62_may09_judtaew/judtaew_states.png)

เราสังเกตว่า states ที่มีสีเขียว คือ state ที่ต่อเติมจากแถวปัจจุบันแถวเดียวโดยตรง และ state สีฟ้า คือ state จบ (ไม่สามารถ update state อื่นๆได้อีกแล้ว)

หากเราลองมองข้าม state เหล่านี้ (สีเขียวและฟ้า) จะทราบได้ว่า state ที่เหลือ (สีเหลือง) อยู่ในรูปแบบ $(x+1, x, y)$ ทั้งนั้น (สำหรับ $0 \leq x < N$ และ $0 \leq y \leq N$) เราจึงเปลี่ยนวิธีการเก็บ state เป็น `opt[i][j] := dp[i+1][i][j]` แทน แต่ยังคงทำโดยใช้เวลา $\mathcal{O}(N^3)$ อยู่ ดังนี้

```cpp
int opt[2505][2505];
opt[1][0] = 1;
int ans = 0;
for (int j = 1; j < n; j++) {
  for (int k = 0; k < j; k++) {
    int i = j + 1;
    int tmp = opt[j][k]; // := dp[i+1][j][k]
    for (int ex = i; ex <= n; ex++) {
      if (ex == n) {
        ans += tmp;
      } else {
        if (abs(h[ex + 1] - h[j]) <= lim)
          opt[ex][k] += tmp;
        if (k == 0 || abs(h[ex + 1] - h[k]) <= lim)
          opt[ex][j] += tmp;
      }
      if (abs(h[ex + 1] - h[ex]) > lim)
        break;
    }
  }
}
ans *= 6;
if (singlestack)
  ans += 3; // Check if [1,2,3, ... N][null][null] config is possible
```

ต่อมา สังเกตว่าเราจะพยายามส่ง `ex` ต่อไปเรื่อยๆจาก $i$ ไปจนกว่า จะถึง $N$ หรือ `abs(h[ex+1] - h[ex]) > lim` สมมติเราสามารถหาช่วงดังกล่าวได้ เรียกช่วงนั้นว่า $[i, R]$ เราจะได้ว่า สำหรับแต่ละ $ex$ ใน $[i, R]$ หากนำ $ex$ ต่อกับ `k` ได้ก็ให้ update `opt[ex][j]` และหากต่อกับ `j` ได้ก็ให้ update `opt[ex][k]` แต่เนื่องจากเงื่อนไขการต่อกันนั้น ขึ้นอยู่กับความสูง เมื่อมาถึงจุดนี้จะได้แนวคิดในความรู้สึกของการอัพเดตตารางเป็นช่วงสองมิติ ซึ่งเหมือนจะไม่ช่วยอะไร เราจึงต้องหาวิธีการปรับค่าที่ดีกว่านี้

เงื่อนไขของการปรับค่า คือเราต้องการให้ ทุกๆ $ex$ ในช่วง $[i, R]$ และมี **ความสูงของ `ex+1` ต่างจาก `j`** ไม่เกิน `lim` ไปเปลี่ยนค่าช่อง `k` สังเกตว่าเราพิจารณาความสูงเป็นหลัก และหากความสูงเท่ากันแล้วค่าของ opt ก็จะเท่ากันด้วย กล่าวคือ หาก `h[x] == h[y]` แล้ว `opt[x][j] == opt[y][j]` ด้วย เมื่อ $x, y \in [i, R]$

เราจึงต้องการใช้ data structure บางอย่างที่สามารถตอบคำถามได้ว่า ณ ช่องที่มีความสูง $h$ นั้น จะมี `opt[h][j]` เป็นเท่าใด และสามารถอัพเดทค่า `opt[h][j]` เป็นช่วงบนค่า `h` ได้

เพื่อการนั้น เราจึงหยิบ _Fenwick Tree_ มาใช้ และทำการจัดการ query อีกที เพื่อให้สามารถหาค่า `tmp` ได้ตรง และยังสามารถ update ได้อีก เพื่อความง่าย เราจะทำการ transpose (สลับแถวกับคอลัมน์) `opt[i][j]` กลายเป็น `F[i][h[j]] := opt[j][i] := dp[j+1][j][i]` (เมื่อเราต้องการหาค่า `opt[i][j]` ก็ไปถามค่า `F[j][h[i]]` ในตาราง `F`)

ต่อมา เราจะมองเป็น _Fenwick Tree_ $N$ ต้น แต่ละต้น (ในต้นที่ $i$ จะเก็บ `F[i][j]` ว่าถ้า `x` สูง `j` แล้ว `opt[x][i]` มีค่าเท่าใด) โดยเราสามารถสั่งบวกเป็นช่วง แล้วถามเป็นช่อง (range sum update, point query) ได้

เมื่อต้องการเพิ่มค่าในตารางดังภาพ

![judtaew_fenwick](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o62_may09_judtaew/judtaew_fenwick.png)

เราจะแยกเป็น คำสั่ง 2 ค่า ดังภาพด้านล่าง

![judtaew_config](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o62_may09_judtaew/judtaew_config.png)

โดยในคำสั่งด้านบนเราสามารถสั่งเพิ่มค่าใน _Fenwick Tree_ ได้เลย (เพิ่มช่องที่ 3 และ ลดช่องที่ 7 ออก) ส่วนคำสั่งด้านล่างนั้น สามารถเก็บเอาไว้ก่อนแล้วค่อยมาทำได้ (ใช้ Priority Queue เพื่อ maintain คำสั่งดังกล่าว)

สังเกตว่าเราไม่จำเป็นต้องเก็บความสูงทั้ง $10^9$ ค่า แต่สามารถทำ Coordinate Compression ได้ จะทำให้ในแต่ละต้นมี $N$ ค่า และมีทั้งหมด $N$ ต้น สามารถทำการดำเนินการทั้งหมดได้ใน $\mathcal{O}(N^2 \log N)$

โค้ดตัวอย่างดังนี้

```cpp
fenwickTree dp[2505];
bool relayable[2505];
class cleaner : public tuple<int, int, int, int, int> {
public:
  cleaner(int a, int b, int c, int d, int e)
      : tuple<int, int, int, int, int>(a, b, c, d, e) {}
  friend bool operator<(cleaner x, cleaner y) { return get<4>(x) > get<4>(y); }
};
priority_queue<cleaner> pq;
for (int i = 2; i <= n; i++) {
  relayable[i] = abs(h[i] - h[i - 1]) <= k;
}
int last = n;
for (int i = n; i > 0; i--) {
  R[i] = last;
  if (!relayable[i])
    last = i - 1;
}
int ans = 0;
for (int j = 1; j < n; j++) {
  const int i = j + 1;
  if (singlestack) {
    dp[0].update(h[i], h[i], 1); // Initial data
    pq.emplace(0, h[i], h[i], MOD - 1, i + 1);
    if (!relayable[i])
      singlestack = false;
  }
  while (!pq.empty() && get<4>(pq.top()) <= i) {
    // The priority queue maintains the 'future minus' list (refer to above
    // fig.)
    dp[get<0>(pq.top())].update(get<1>(pq.top()), get<2>(pq.top()),
                                get<3>(pq.top()));
    pq.pop();
  }
  vector<int> buf;
  // Get data of the current row
  for (int l = 0; l < j; l++)
    buf.push_back(dp[l].query(h[i]));
  for (int l = 0; l < j; l++) {
    int tmp = buf[l];
    if (R[i] == n) {
      ans += tmp;
    }
    dp[l].update(h[j] - k, h[j] + k, tmp); // Range update on heights
    int bound =
        min(n, R[i] + 1); // If there must be 'future minus', maintain them
    pq.emplace(l, h[j] - k, h[j] + k, MOD - tmp, bound + 1);
    if (l == 0) {
      dp[j].update(-1e9, 1e9, tmp);
      pq.emplace(j, -1e9, 1e9, MOD - tmp, bound + 1);
    } else {
      dp[j].update(h[l] - k, h[l] + k, tmp);
      pq.emplace(j, h[l] - k, h[l] + k, MOD - tmp, bound + 1);
    }
  }
}
ans *= 6;
if (singlestack)
  ans += 3; // Check if [1,2,3, ... N][null][null] config is possible
```

โดยจะอาศัยฟังก์ชัน `update` และ `query` ซึ่งทำงานบน _Fenwick Tree_ ในเวลา $\mathcal{O}(\log N)$
