## ปัญหา

โจทย์ข้อนี้มีเหตุการณ์น้ำท่วมจากรอบๆแผนที่ โดยแต่ละบ้านจะสร้างกำแพงในแต่ละด้านของตัวเอง ก็ต่อเมื่อด้านนั้นมีน้ำท่วม

โจทย์ต้องการหาว่าในบ้านแต่ละชุดที่ติดกัน บ้านก้อนที่มีกำแพงมากที่สุด มีกี่กำแพง

โดยข้อนี้จะใช้วิธี Flood Fill

## ขั้นตอนที่ 0: รับ Input

ประกาศ static array สองตัว แทนแผนที่ของเมือง และพื้นที่ที่น้ำท่วมถึง

```cpp
char house[1002][1002];
bool isFlood[1002][1002]{false};
int R, C;

cin >> R >> C;

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
    cin >> house[i] + 1;
    house[i][C + 1] = '.';
}
```

**Tips**: อย่าใช้ std::string โดยไม่จำเป็น (จากคนที่ใช้ std::string ในข้อนี้ตอนแข่งแล้วติด T หายไป 8 คะแนน)

## ขั้นตอนที่ 1: หาว่าน้ำท่วมถึงจุดไหนบ้าง

วิธีการเขียน Flood Fill นั้นมีหลายแบบขึ้นอยู่กับความถนัด แต่ในตัวอย่างนี้จะใช้วิธี Recursive

**หมายเหตุ**: บางข้อที่มีการจำกัด memory มากๆ เช่น [toi11_candle](https://beta.programming.in.th/tasks/toi11_candle)
ไม่สามารถเขียน Flood Fill แบบ Recursive ได้

```cpp
void flood(int i, int j) {
    if (isFlood[i][j])
        return;
    isFlood[i][j] = true;

    if (house[i][j] == 'X')
        return;

    if (i > 0 && house[i - 1][j] == '.')
        flood(i - 1, j);
    if (i <= R && house[i + 1][j] == '.')
        flood(i + 1, j);
    if (j > 0 && house[i][j - 1] == '.')
        flood(i, j - 1);
    if (j <= C && house[i][j + 1] == '.')
        flood(i, j + 1);
}

// Flood Fill จากขอบแมพ
for (int i = 0; i < R; i++) {
    flood(i, 0);
    flood(i, C + 1);
}
for (int j = 0; j < C; j++) {
    flood(0, j);
    flood(R + 1, j);
}
```

## ขั้นตอนที่ 2: Flood Fill บ้านแต่ละชุดที่ติดกัน

```cpp
int getWalls(int i, int j) {
    if (house[i][j] != 'X')
        return 0;

    // ลบบ้านออก เพื่อที่จะได้ไม่คำนวณซ้ำ
    house[i][j] = '.';

    int res = 0;

    // หาว่าในแต่ละด้านของบ้านหลัง i,j มีกี่ด้านที่น้ำท่วมบ้าง และบวกผลลัพธ์ไปที่ int& co
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

int maximumWall = 0;

for (int i = 1; i <= R; i++) {
    for (int j = 1; j <= C; j++) {
        // รับจำนวนกำแพง สำหรับบ้านแต่ละชุดที่ติดกัน
        int currentWall = getWalls(i, j);
        maximumWall = max(maximumWall, currentWall);
    }
}

// พิมพ์คำตอบ
cout << maximumWall << "\n";
```

**Time Complexity**: $\mathcal{O}(RC)$

**Space Complexity**: $\mathcal{O}(RC)$

## Full Code

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

    if (house[i][j] == 'X')
        return;

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
