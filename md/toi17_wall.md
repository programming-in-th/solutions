## สรุปปัญหา

จากโจทย์ข้อนี้ เราจะสามารถสรุปใจความสำคัญได้ว่า

- มีน้ำท่วมจากรอบๆแผนที่ โดยน้ำจะไหลไปเรื่อยๆ แต่จะ**ไม่สามารถไหลผ่านช่องที่มีบ้านได้**
- แต่ละบ้านจะสร้างกำแพงในแต่ละด้านของตัวเอง **ก็ต่อเมื่อด้านนั้นไม่ใช่บ้าน และน้ำท่วมถึง**
- รับข้อมูลนำเข้าเป็นแผนที่
- สิ่งที่โจทย์ต้องการคือ หาว่าในบ้านแต่ละชุดที่ติดกัน/มีด้านร่วมกัน
  บ้านชุดที่มีกำแพงมากที่สุด มีกี่กำแพง

**หมายเหตุ**: ให้ดูตัวอย่างของโจทย์ประกอบ ซึ่งอธิบายกรณีต่างๆไว้หมดแล้ว

## วิธีทำ

ความยากของโจทย์ข้อนี้: **ง่าย** (เจ้าภาพบอกเอง)

ข้อนี้ถือว่าไม่ได้ซับซ้อนมาก _และเป็นข้อง่ายเพียงหนึ่งเดียวของ TOI17_
เราสามารถที่จะลงมือเขียนได้เลย ตัวโจทย์ไม่ได้พลิกแพลงเยอะ โดยวิธีที่จะใช้คือ **Flood Fill**

### เกี่ยวกับ Flood Fill Algorithm

หากไม่รู้จัก Flood Fill Algorithm แนะนำให้ลองไปอ่านได้ตามเว็บเหล่านี้เลย

- [Wikipedia](https://en.wikipedia.org/wiki/Flood_fill)

- [GeeksforGeeks](https://www.geeksforgeeks.org/flood-fill-algorithm-implement-fill-paint/)

### ขั้นตอนที่ 0: รับ Input

ประกาศ static array สองตัว แทนแผนที่ของเมือง และพื้นที่ที่น้ำท่วมถึง

```cpp
char house[1002][1002];
bool isFlood[1002][1002]{false};

// Row and Column
int R, C;
```

การสร้างแผนที่สามารถทำได้หลายแบบ ขึ้นอยู่กับสไตล์การเขียนของแต่ละคน โดยในตัวอย่างนี้จะทำการเผื่อขอบไว้

```cpp
std::cin >> R >> C;

// Initialize ขอบแมพ
for (int i = 0; i < R; i++) {
    house[i][0] = '.';
    house[i][C + 1] = '.';
}
for (int j = 0; j < C; j++) {
    house[0][j] = '.';
    house[R + 1][j] = '.';
}

// รับ input
for (int i = 0; i < R; i++) {
    std::cin >> house[i] + 1;
    house[i][C + 1] = '.';
}
```

**ข้อควรระวัง**: อย่าใช้ `std::string` โดยไม่จำเป็น
(จากคนที่ใช้ `std::string` ในข้อนี้ตอนแข่งแล้วติด T หายไป 8 คะแนน)

### ขั้นตอนที่ 1: หาว่าน้ำท่วมถึงจุดไหนบ้าง

วิธีการเขียน Flood Fill นั้นมีหลายแบบขึ้นอยู่กับความถนัด
เช่น recursion, stack, queue, bfs, dfs เป็นต้น
แต่ในตัวอย่างนี้จะใช้วิธี recursion dfs เพราะเขียนง่ายดี

**ข้อควรระวัง**: บางข้อที่มีการจำกัด memory มากๆ เช่น [toi11_candle](https://beta.programming.in.th/tasks/toi11_candle)
ไม่สามารถเขียน Flood Fill แบบ recursion ได้ เพราะการเรียกฟังก์ชันแต่ละครั้ง
จะมีการใช้หน่วยความจำเพื่อเก็บ arguments หาก recursion มีความลึกมากๆ ก็จะทำให้หน่วยความจำเต็มได้

```cpp
void flood(int i, int j) {
    if (isFlood[i][j])
        return;
    isFlood[i][j] = true;

    if (i > 0 && house[i - 1][j] == '.')
        flood(i - 1, j);
    if (i <= R && house[i + 1][j] == '.')
        flood(i + 1, j);
    if (j > 0 && house[i][j - 1] == '.')
        flood(i, j - 1);
    if (j <= C && house[i][j + 1] == '.')
        flood(i, j + 1);
}
```

จากนั้น Flood Fill จากขอบแมพเพื่อแทนการไหลของน้ำ (recap โจทย์: น้ำจะไหลจากพื้นที่รอบๆ)

```cpp
for (int i = 0; i < R; i++) {
    flood(i, 0);
    flood(i, C + 1);
}
for (int j = 0; j < C; j++) {
    flood(0, j);
    flood(R + 1, j);
}
```

### ขั้นตอนที่ 2: สร้างกำแพงสำหรับบ้านแต่ละชุดที่ติดกัน

```cpp
int getWalls(int i, int j) {
    if (house[i][j] != 'X')
        return 0;

    // ลบบ้านออก เพื่อที่จะได้ไม่คำนวณซ้ำ
    house[i][j] = '.';

    int res = 0;

    // หาว่าในแต่ละด้านของบ้านหลัง (i, j) มีกี่ด้านที่น้ำท่วมบ้าง
    // (recap: จะสร้างกำแพงเฉพาะในด้านที่มีน้ำท่วม)
    // และบวกผลลัพธ์ไปที่ int& co
    void getSurroundingFloods(int &co, int i, int j) {
        if (isFlood[i - 1][j]) co++;
        if (isFlood[i + 1][j]) co++;
        if (isFlood[i][j - 1]) co++;
        if (isFlood[i][j + 1]) co++;
    }

    getSurroundingFloods(res, i, j);

    // Recursive Flood Fill บ้านที่ติดกัน
    res += getWalls(i - 1, j);
    res += getWalls(i + 1, j);
    res += getWalls(i, j - 1);
    res += getWalls(i, j + 1);

    return res;
}
```

```cpp
int maximumWall = 0;

for (int i = 1; i <= R; i++) {
    for (int j = 1; j <= C; j++) {
        // รับจำนวนกำแพง สำหรับบ้านแต่ละชุดที่ติดกัน
        int currentWall = getWalls(i, j);
        maximumWall = max(maximumWall, currentWall);
    }
}

// พิมพ์คำตอบ
std::cout << maximumWall << "\n";
```

**Time Complexity**: $\mathcal{O}(RC)$

เหตุผล: ฟังก์ชัน `flood` จะถูกเรียกบนแต่ละช่องไม่เกิน 5 ครั้ง (เรียกโดยตรงจากลูปหลัก และจากด้านรอบๆ) เช่นเดียวกับฟังก์ชัน `getWalls`  
ซึ่งแต่ละรอบของ recursion ใช้เวลาเป็น Constant Time หรือ $\mathcal{O}(1)$  
ดังนั้นเวลาโดยรวมจึงเป็น $\mathcal{O}(RC)$

**Space Complexity**: $\mathcal{O}(RC)$

## Full Code

[Link to Submission](https://beta.programming.in.th/submissions/9v4sWVI8q0r4JbgCNIrp)

```cpp
#include <bits/stdc++.h>
using namespace std;

char house[1002][1002];
bool isFlood[1002][1002]{false};

int R, C;

void flood(int i, int j) {
    if (isFlood[i][j])
        return;
    isFlood[i][j] = true;

    if (i > 0 && house[i - 1][j] == '.')
        flood(i - 1, j);
    if (i <= R && house[i + 1][j] == '.')
        flood(i + 1, j);
    if (j > 0 && house[i][j - 1] == '.')
        flood(i, j - 1);
    if (j <= C && house[i][j + 1] == '.')
        flood(i, j + 1);
}

inline void getSurroundingFloods(int &co, int i, int j) {
    if (isFlood[i - 1][j])
        co++;
    if (isFlood[i + 1][j])
        co++;
    if (isFlood[i][j - 1])
        co++;
    if (isFlood[i][j + 1])
        co++;
}

int getWalls(int i, int j) {
    if (house[i][j] != 'X')
        return 0;

    house[i][j] = '.';

    int res = 0;
    getSurroundingFloods(res, i, j);
    res += getWalls(i - 1, j);
    res += getWalls(i + 1, j);
    res += getWalls(i, j - 1);
    res += getWalls(i, j + 1);

    return res;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    cin >> R >> C;

    for (int i = 0; i < R; i++) {
        house[i][0] = '.';
        house[i][C + 1] = '.';
    }
    for (int j = 0; j < C; j++) {
        house[0][j] = '.';
        house[R + 1][j] = '.';
    }

    // * Input
    for (int i = 0; i < R; i++) {
        cin >> house[i] + 1;
        house[i][C + 1] = '.';
    }

    // * Flood
    for (int i = 0; i < R; i++) {
        flood(i, 0);
        flood(i, C + 1);
    }
    for (int j = 0; j < C; j++) {
        flood(0, j);
        flood(R + 1, j);
    }

    int maximumWall = 0;

    for (int i = 1; i <= R; i++) {
        for (int j = 1; j <= C; j++) {
            int currentWall = getWalls(i, j);
            maximumWall = max(maximumWall, currentWall);
        }
    }

    cout << maximumWall << "\n";
}
```
