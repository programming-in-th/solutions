ในข้อนี้ เราต้องการใส่สีลงในตาราง โดยจะค่อยๆเติมสี่เหลี่ยมผืนผ้าที่มีความเข้มแตกต่างกัน แต่รับประกันแน่ๆว่าชิดขอบล่าง วิธีการที่เป็นพื้นฐานที่สุดของข้อนี้ คือค่อยๆเติมสี่เหลี่ยมผืนผ้าทีละชิ้น โดยใช้เวลารวม $\mathcal{O}(N+\sum\limits_{i=1}^{N}{h_i w_i})$ ซึ่งเห็นได้ชัดว่าไม่ทันเวลาแน่นอน

เราสามารถสร้าง event point เพื่อไล่จากซ้ายไปขวาได้ ดังนี้ เราสังเกตว่าการสร้างสี่เหลี่ยมผืนผ้าตั้งแต่พิกัด $x = s_i$ ถึง $x = s_i+w_i-1$ ความสูง $h_i$ นั้น ความเข้ม $o_i$ สามารถ encode จากค่า $(s_i, h_i, w_i, o_i)$ เป็นค่า $(x_i, h_i, o_i)$ สองค่าคือ $(s_i, h_i, o_i)$ และ $(s_i+w_i, h_i, -o_i)$ ต่อมาเราจะใช้สามสิ่งอันดับเหล่านี้แทน จุดต่างๆที่จะมีการเปลี่ยนแปลงสี

เราจะค่อยๆเลื่อนคอลัมน์ พิจารณาจาก $x = 0$ เพิ่มขึ้นไปเรื่อย หากมี event point บน $x$ ปัจจุบัน เราจะทำการ update คอลัมน์ปัจจุบันตามที่ event point กล่าวไว้ (เช่น เมื่อ $x = 3$ มี $(3, 5, 2)$ แสดงว่าคอลัมน์ปัจจุบันจะมีค่าสีเพิ่มขึ้น $+2$ สำหรับแถวที่สูงไม่เกิน $5$ ต่อมาคอลัมน์ $x = 4$ จะเหมือนกับคอลัมน์ $x = 3$ หากไม่มีการเปลี่ยนค่าใดๆเกิดขึ้น)

เราจะทำได้ในเวลา $\mathcal{O}(WH + N \log N + \sum\limits_{i=1}^{N}{h_i})$ โค้ดตัวอย่างดังต่อไปนี้

```cpp
int N, T;                                 // Defined in problem statement
vector<tuple<int, int, int>> eventPoints; // (x, h, o) as defined above
for (int i = 0; i < N; i++) {
  int s, h, w, o; // s, h, w, o as defined in problem statement
  eventPoints.push_back({s, h, o});
  eventPoints.push_back({s + w, h, -o});
}
sort(eventPoints.begin(), eventPoints.end());

int answer = 0;
int currentColumn[1048576]; // currentColumn[i] := val at height i on the
                            // current col
int eventPtr = 0;
for (int i = 1; i <= 4000000; i++) {
  if (eventPtr < eventPoints.size()) {
    int x, h, o;
    tie(x, h, o) = eventPoints[eventPtr];
    while (x == i) {
      // While there are some eventPoints which belong to current x, process
      // them.
      for (int j = 1; j <= h; j++)
        currentColumn[j] += o;
      eventPtr++;
      tie(x, h, o) = eventPoints[eventPtr];
    }
  }
  for (int j = 1; j <= 1000000; j++) {
    if (currentColumn[j] == T) { // Expected value
      answer++;
    }
  }
}
```

ต่อมา เราสังเกตว่าไม่จำเป็นจะต้องไล่ $x$ ให้ครบทุกค่าจาก $1$ ถึง $4,000,000$ โดยเราสามารถไล่เฉพาะค่าที่มีอยู่ใน `eventPoints` ได้ สมมติคอลัมน์ $x = X_1$ มีค่าที่ตรงตามเงื่อนไข (เท่ากับ $T$) อยู่ทั้งหมด $V$ ค่า เราก็สามารถมั่นใจได้ว่า ระหว่าง $x = X_1$ ไปจนถึง $x = X_2$ นั้น ค่าที่เท่ากับ $T$ ก็จะมีอยู่ $V$ ค่าเช่นเดิม ($X_1$ แทนคอลัมน์ที่มี event point ใดๆ และ $X_2$ แทนคอลัมน์ที่มี event point ที่อยู่ถัดจาก $X_1$) เราจะสามารถดัดแปลงได้ดังนี้

```cpp
int N, T;                                 // Defined in problem statement
vector<tuple<int, int, int>> eventPoints; // (x, h, o) as defined above
vector<int>
    possibleXs; // Maintain a set of columns with at least one event point
for (int i = 0; i < N; i++) {
  int s, h, w, o; // s, h, w, o as defined in problem statement
  eventPoints.push_back({s, h, o});
  eventPoints.push_back({s + w, h, -o});
  possibleXs.push_back(s);
  possibleXs.push_back(s + w);
}
sort(eventPoints.begin(), eventPoints.end());
sort(possibleXs.begin(), possibleXs.end());
possibleXs.resize(unique(possibleXs.begin(), possibleXs.end()) -
                  possibleXs.begin());
// Sort and resolve distinct column positions

int answer = 0;
int currentColumn[1048576]; // currentColumn[i] := val at height i on the
                            // current col
int eventPtr = 0;
int lastPos = 0;
for (int i : possibleXs) {
  for (int j = 1; j <= 1000000; j++) {
    if (currentColumn[j] == T) { // Expected value
      answer += i - lastPos;
    }
  }
  lastPos = i;
  if (eventPtr < eventPoints.size()) {
    int x, h, o;
    tie(x, h, o) = eventPoints[eventPtr];
    while (x == i) {
      // While there are some eventPoints which belong to current x, process
      // them.
      for (int j = 1; j <= h; j++)
        currentColumn[j] += o;
      eventPtr++;
      tie(x, h, o) = eventPoints[eventPtr];
    }
  }
}
```

จะลดเวลาเหลือเพียง $\mathcal{O}(NH + N \log N + \sum\limits_{i=1}^{N}{h_i})$

ต่อมาคือขั้นตอนสำคัญของข้อนี้ คือการพยายามทำการ update ค่า `currentColumn` ในเวลาที่รวดเร็วกว่าความสูงของ event point นั้น วิธีอย่างเป็นทางการของการ optimize ส่วนนี้คือการใช้โครงสร้างข้อมูลประเภทต้นไม้ค้นหาทวิภาค (*Binary Search Tree*) ที่แต่ละปมเก็บผลรวมของสีเอาไว้ แต่วิธีดังกล่าวเป็นวิธีที่ยากที่จะเขียนใช้จริง วิธีต่อมาที่จะนำเสนอในที่นี้คือการใช้โครงสร้างข้อมูลที่เกินขอบเขตเนื้อหาระดับชาติ แต่เขียนง่ายกว่าวิธีดังกล่าว นั่นคือการใช้ *Fenwick Tree* หรือเรียกอีกอย่างว่า *Binary Search Tree* (จากงานวิจัย *A new data strcture for culumulative frequency tables* โดย *Peter M. Fenwick*) ซึ่งเป็นโครงสร้างข้อมูลยอดนิยมในการแก้โจทย์ปัญหาหลังระดับชาติ

โครงสร้างข้อมูลดังกล่าวจะรองรับการดำเนินการดังนี้ `add(idx, amt)` แทนการเติมค่า **ตั้งแต่** ช่อง `idx` ไปจนถึงช่องสูงสุด (ในที่นี้จะใช้ตัวแปร `H` ซึ่งมีค่า $1,000,000$ ตามโจทย์) โดยเติมไป `amt` หรือเทียบเท่าการดำเนินการตามโค้ดตังต่อไปนี้

```cpp
int value[1048576];
void add(int idx, int amt) {
  for (int i = idx; i <= H; i++)
    value[i] += amt;
}
```

ต่อมาจะรองรับการดำเนินการ `sum(idx)` แทนการหาผลรวม **ตั้งแต่** ช่อง `idx` ลงไปถึงช่องต่ำสุด (ในที่นี้คือช่องหนึ่ง) เทียบเท่าการดำเนินการตามโค้ดต่อไปนี้

```cpp
int value[1048576];
int sum(int idx) {
  int ret = 0;
  for (int i = idx; i >= 1; i--) {
    ret += value[i];
  }
  return ret;
}
```

สังเกตว่าการดำเนินการทั้งสองทำงานในเวลา $\mathcal{O}(H)$ แต่หากใช้โครงสร้างข้อมูล *Fenwick Tree* จะลดเวลาเหลือเพียงอย่างละ $\mathcal{O}(\log H)$ ทั้งนี้ จะไม่แสดงวิธีการสร้าง *Fenwick Tree* ในข้อนี้ แต่จะหยิบมาใช้งานเลย

จากโค้ดข้างต้นเราจะเปลี่ยนจาก

```cpp
while (x == i) {
  // While there are some eventPoints which belong to current x, process them.
  for (int j = 1; j <= h; j++)
    currentColumn[j] += o;
  eventPtr++;
  tie(x, h, o) = eventPoints[eventPtr];
}
```

เป็น

```cpp
while (x == i) {
  // While there are some eventPoints which belong to current x, process them.
  add(1, o);      // processed by the Fenwick Tree
  add(j + 1, -o); // processed by the Fenwick Tree
  eventPtr++;
  tie(x, h, o) = eventPoints[eventPtr];
}
```

ต่อมา จะยังมีขั้นตอนที่ต้องหาว่ามีค่าที่เท่ากับ $T$ อยู่กี่ค่าใน *Fenwick Tree* แต่เนื่องจากโจทย์จะเติมจากช่องล่างสุดขึ้นไปข้างบนเท่านั้น เราสามารถสังเกตได้ไม่ยากว่า ในคอลัมน์ใดๆ ค่าที่อยู่แถวล่างจะมีค่า **ไม่น้อยไปกว่า** ค่าที่อยู่แถวบนกว่า เราจึงสามารถใช้ *Binary Search* เพื่อหาว่า **แถวที่ต่ำที่สุดที่มีค่าน้อยกว่าหรือเท่ากับ** $T$ คือแถวไหน (จะแทนด้วย $R_{low}$) และ **แถวที่สูงที่สุดที่มีค่ามากกว่าหรือเท่ากับ** $T$ คือแถวไหน (จะแทนด้วย $R_{high}$) จะได้ว่าคำตอบของคอลัมน์ปัจจุบันคือ $R_{high} - R_{low} + 1$

โค้ดตัวอย่างแก้ไขจาก

```cpp
for (int j = 1; j <= 1000000; j++) {
  if (currentColumn[j] == T) { // Expected value
    answer += i - lastPos;
  }
}
```

เป็นดังนี้

```cpp
int rlow = binarySearchLessEqual(T);     // Implement Binary Search
int rhigh = binarySearchMoreEqual(T);    // Implement Binary Search
if (sum(rlow) == T && sum(rhigh) == T) { // Recheck in case there is no T found
  answer += (rhigh - rlow + 1) * (i - lastPos);
}
```

เท่านี้การทำงานทั้งหมดจะใช้เวลา $\mathcal{O}(N \log^2 H + N \log N + \sum\limits_{i=1}^{N}{\log h_i}) = \mathcal{O}(N \log^2 H + N \log N)$

Bonus: มันจะมีวิธีที่ดีกว่านี้มั้ย? ลองคิดอัลกอริทึมที่ทำได้ในเวลา $\mathcal{O}(N \log H + N \log N)$ ดู

