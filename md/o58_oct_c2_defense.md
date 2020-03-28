นิยามให้ $dp(x,y)$ เป็นต้นทุนน้อยที่สุดที่จะสร้างหอคอยโดยที่สนใจเพียงแค่ตารางเริ่มตั้งแต่ $x$ ขึ้นไป และ ได้สร้างหอคอยที่ตำแหน่ง $x$ และ $y$ ไปแล้ว

$dp(x,y) = min(dp(y,i) + cost_i)$ โดยที่ $y < i \leq x + K$ เพื่อที่จะให้ในทุกช่วง $K$ มีอย่างน้อยสองหอคอยเสมอ

หากเราคำนวณตาราง $dp(x,y)$ โดยตรงจะใช้ $\mathcal{O}(N^3)$ ซึ่งจะช้าเกินไปสำหรับ $N \leq 3000$

กำหนดให้ $best(x,y) = min(dp(x,i) + cost_i)$ โดยที่ $x < i \leq y$ ดังนั้นเราจะสามารถเปลี่ยนสมการ $dp$ เป็น $dp(x,y) = best(y,x+K)$ ได้ ซึ่งจะทำให้เราสามารถคำนวณ $dp$ และ $best$ ไปพร้อมๆกันได้ใน $\mathcal{O}(N^2)$

Time-complexity: $\mathcal{O}(N^2)$

```cpp
int solve() {
  for(int x = n; x >= 1; x--) {
    for(int y = x + 1; y <= n; y++) {
      dp[x][y] = best[y][min(n, x + K)];
    }
    best[x][x] = inf;
    for(int i = x + 1; i <= n; i++) {
      best[x][i] = min(best[x][i - 1], dp[x][i] + cost[i]);
    }
  }
  
  int ans = inf;
  for(int x = 1; x <= k - 1; x++) {
    for(int y = x + 1; y <= k; y++) {
      ans = min(ans, dp[x][y] + cost[x] + cost[y]);
    }
  }
  
  return ans;
}
```
