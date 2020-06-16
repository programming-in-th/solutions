สำหรับข้อนี้ โจทย์ต้องการให้หุ่นยนต์ไปเก็บของแต่ละชิ้น

### Observation 1

เราสามารถมองได้ว่า เราต้องการให้ของทุกชิ้น ได้รับการเก็บโดยหุ่นยนต์ 1 ตัว นั่นคือ เราไม่สนใจว่าหุ่นยนต์ตัวไหนจะมาเก็บของชิ้นปัจจุบัน แต่เราต้องการให้หุ่นยนต์ **ที่อยู่ใกล้ที่สุด** เป็นหุ่นยนต์เก็บของชิ้นนี้ 

ตอนนี้โจทย์จะกลายเป็นว่า สำหรับของแต่ละชิ้น ให้หาหุ่นยนต์ที่ใกล้ที่สุด (ถ้ามี) แล้วนำระยะทางใกล้สุดของของแต่ละชิ้นมาบวกกัน แล้วนับด้วยว่ามีของอยู่กี่ชิ้นที่สามารถหาหุ่นยนต์ใกล้สุดได้ (กรณีที่หาไม่ได้เช่น มีกำแพงล้อมรอบ)

วิธีการที่เห็นได้ชัดเจนที่สุด คือการทำ *Breadth-first search* จากของแต่ละชิ้นไปหาหุ่นยนต์ที่ใกล้ที่สุด ซึ่งจะใช้เวลา $\mathcal{O}(TNM)$ หากมีของทั้งหมด $T$ ชิ้น

อีกวิธีที่มีความเป็นไปได้คือหาว่าหุ่นยนต์แต่ละตัวมีระยะทางไปแต่ละจุดเท่าใด แล้วค่อยๆตามมาดูทีหลังว่าของแต่ละชิ้นควรจะไปเชื่อมกับหุ่นยนต์ตัวไหน ใช้เวลา $\mathcal{O}(KNM + TK)$ หากมีหุ่นยนต์ $K$ ตัว

วิธีการข้างต้นดูเหมือนเป็นวิธีการที่แย่กว่าวิธีแรก แต่จะทำให้ปรับความเข้าใจสู่ข้อสังเกตต่อไปได้โดยง่าย

### Observation 2

สมมติให้ $d(i, j)$ แทนระยะห่าง ระหว่างหุ่นยนต์ตัวที่ $i$ กับ ของชิ้นที่ $j$ สิ่งที่เราต้องการหา คือ $\sum\limits_{j=1}^{T}{\min\limits_{i=1}^{K}{d(i,j)}}$

สังเกตว่า ไม่ว่า $i$ จะเป็นหุ่นยนต์ตัวไหนก็ตาม หากระยะทางสั้นสุด ก็ทำให้ได้คำตอบตามที่ต้องการ

ปัญหาดังกล่าว คือ ปัญหา *Multiple Source Shortest Path (MSSP)* ซึ่งมีวิธีการแก้ปัญหาได้เร็วกว่าการทดลองเริ่มต้นจากแต่ละจุดที่ต่างกัน โดยจะเปลี่ยนมาเริ่มจาก **ทุกจุดพร้อมกัน** แทน

การกระทำดังกล่าว อาจดูเหมือนเป็นไปไม่ได้ แต่เราสามารถดัดแปลงกระบวนการ *Breadth-first search* ได้ดังนี้

ตัวอย่างโค้ดการ *Breadth-first search* บนตาราง แบบปกติ

```cpp
int dist[2048][2048];    // dist[i][j] := distance between the current robot and
                         // point (row i, col j)
queue<pair<int, int>> q; // BFS Queue
int x, y;                // Assume the current robot is at row x, col y
int N, M;                // Assume there are N rows and M columns
for (int i = 1; i <= N; i++) {
  for (int j = 1; j <= M; j++) {
    dist[i][j] = 1e9; // High enough for infinity
  }
}
dist[x][y] = 0;  // Starts at zero
q.emplace(x, y); // Add current position to queue
while (!q.empty()) {
  tie(x, y) = q.front(); // Let x and y be the current coordinate in the queue
  q.pop();
  if (x > 1 && dist[x - 1][y] > dist[x][y] + 1) {
    dist[x - 1][y] = dist[x][y] + 1;
    q.emplace(x - 1, y);
  }
  if (x < N && dist[x + 1][y] > dist[x][y] + 1) {
    dist[x + 1][y] = dist[x][y] + 1;
    q.emplace(x + 1, y);
  }
  if (y > 1 && dist[x][y - 1] > dist[x][y] + 1) {
    dist[x][y - 1] = dist[x][y] + 1;
    q.emplace(x, y - 1);
  }
  if (y < M && dist[x][y + 1] > dist[x][y] + 1) {
    dist[x][y + 1] = dist[x][y] + 1;
    q.emplace(x, y + 1);
  }
}
```

การกระทำข้างต้นใช้เวลา $\mathcal{O}(NM)$ และเราจะต้องกระทำซ้ำทั้งหมด $K$ รอบ ตามจำนวนหุ่นยนต์ เพื่อหาระยะทางทั้งหมด ต่อมาจะเป็นขั้นตอนการเริ่มต้น **พร้อมกันทุกจุด** ดังนี้

```cpp
int dist[2048][2048]; // dist[i][j] := distance between the current robot and
                      // point (row i, col j)
char mp[2048][2048];
queue<pair<int, int>> q; // BFS Queue
int N, M;                // Assume there are N rows and M columns
for (int i = 1; i <= N; i++) {
  for (int j = 1; j <= M; j++) {
    if (mp[i][j] == 'X') {
      dist[i][j] = 0;  // Starts at zero
      q.emplace(i, j); // Add current position to queue
    } else {
      dist[i][j] = 1e9; // High enough for infinity
    }
  }
}
while (!q.empty()) {
  tie(x, y) = q.front(); // Let x and y be the current coordinate in the queue
  q.pop();
  if (x > 1 && dist[x - 1][y] > dist[x][y] + 1) {
    dist[x - 1][y] = dist[x][y] + 1;
    q.emplace(x - 1, y);
  }
  if (x < N && dist[x + 1][y] > dist[x][y] + 1) {
    dist[x + 1][y] = dist[x][y] + 1;
    q.emplace(x + 1, y);
  }
  if (y > 1 && dist[x][y - 1] > dist[x][y] + 1) {
    dist[x][y - 1] = dist[x][y] + 1;
    q.emplace(x, y - 1);
  }
  if (y < M && dist[x][y + 1] > dist[x][y] + 1) {
    dist[x][y + 1] = dist[x][y] + 1;
    q.emplace(x, y + 1);
  }
}
```

เท่านี้ `dist[i][j]` ก็จะเก็บระยะห่างจากจุดแถว $i$ คอลัมน์ $j$ ไปถึงหุ่นยนต์ที่ใกล้ที่สุด โดยใช้เวลาเพียง $\mathcal{O}(NM)$ เท่านั้น
