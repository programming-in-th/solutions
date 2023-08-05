### Subtask 1 ( $N\leq 20$ )

เราสามารถลองทุกวิธีในการเลือกแบตเตอรี่ $N$ ก้อนและดูว่าในแต่ละกรณีที่ทำให้ม้าไปถึงจุดสิ้นสุด ใช้เวลาชาร์จเท่าไร และเก็บกรณีที่
ใช้เวลาชาร์จน้อยที่สุด

Time Complexity: $\mathcal{O}(2^N)$

### Subtask 2 ( N ≤ 1000, X ≤ 500, Y = 0 )

ใน subtask นี้ เราไม่จำเป็นต้องคำนึงถึงค่า $Y$ เนื่องจากจุดเริ่มต้นเรามีค่า $Y=0$

สังเกตว่าปัญหานี้คล้ายกับปัญหา Knapsack แต่แทนที่เราจะทำให้ผลรวม $x_i$ ไม่เกิน $X$ และผลรวม $c_i$ มากที่สุด เราต้องทำให้ผลรวม
$x_i$

เกิน $X$ และผลรวม $c_i$ น้อยที่สุด
คำนึงด้วยว่าหากม้าไปเกิน X สามารถเก็บค่าไว้ที่ช่อง X ได้เลย เพราะถือว่าถึงจุดหมายเหมือนกัน

[https://en.wikipedia.org/wiki/Knapsack_problem](https://en.wikipedia.org/wiki/Knapsack_problem)

Time Complexity: $\mathcal{O}(NX)$

### Subtask 3 ( ไม่มีเงื่อนไขเพิ่มเติม )

สังเกตว่าเราต้องพิจารณาทั้งสองแกนในการคำนึงว่าเราถึงจุดหมายแล้วหรือไม่ เราจึงต้องเพิ่มมิติในการเก็บค่าของตาราง knapsack จาก
ตารางขนาด $N\times X$ เป็นขนาด $N\times X\times Y$

$$dp(i,x,y)$$
$$=dp(i-1,x,y) \quad x<x_i,y<y_i$$
$$=\min(dp(i-1,x,y),\min\limits_{1\leq j\leq x_i}\{dp(i-1,x-j,y-y_i)+c_i\}) \quad x=X$$
$$=\min(dp(i-1,x,y),\min\limits_{1\leq j\leq y_i}\{dp(i-1,x-x_i,y-j)+c_i\}) \quad y=Y$$
$$=\min(dp(i-1,x,y),\min\limits_{1\leq j\leq x_i}\{\min\limits_{1\leq k\leq y_i}\{dp(i-1,x-j,y-k)+c_i\}\}) \quad x=X,y=Y$$
$$=\min(dp(i-1,x,y),dp(i-1,x-x_i,y-y_i)+c_i) \quad \text{other}$$

เนื่องจากตารางขนาด $N\times X\times Y$ มีขนาดเกิน Memory Limit เราสามารถลดการใช้ Memory ด้วยการเก็บแค่ค่า $N$ ปัจจุบันและ
ค่า $N$ ก่อนหน้าทำให้ขนาดตารางเหลือ $2\times X\times Y$

Time Complexity: $\mathcal{O}(NXY)$

### Solution Code:

```cpp
#include<bits/stdc++.h>
using namespace std;
#define MAXL 1000000000000000000ll
int n, x, y;
array<int, 3> arr[1010];
long long dp[2][550][550];
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> n >> x >> y;
    for(int i = 0; i < n; i++){
        cin >> arr[i][0] >> arr[i][1] >> arr[i][2];
    }
    for(int i = 0; i <= x; i++){
        for(int j = 0; j <= y; j++){
            dp[0][i][j] = MAXL;
        }
    }
    dp[0][0][0] = 0; 
    for(int i = 0; i < n; i++){
        for(int j = 0; j <= x; j++){
            for(int k = 0; k <= y; k++){
                dp[(i+1)%2][j][k] = dp[i%2][j][k];
            }
        }
        for(int j = 0; j <= x; j++){
            for(int k = 0; k <= y; k++){
                dp[(i+1)%2][min(j+arr[i][0], x)][min(k+arr[i][1], y)] = min(dp[(i+1)%2][min(j+arr[i][0], x)][min(k+arr[i][1], y)], dp[i%2][j][k] + arr[i][2]); 
            }
        }
    }
    long long ans = dp[n%2][x][y];
    cout << ((ans == MAXL)?-1:ans) << '\n';
    return 0;
}
```