หมายเหตุทุก Subtask จำเป็นต้องรับ input ด้วย $\mathcal{O}(N^2)$

### Subtask 1 ($K=0$)

เนื่องจากเราไม่สามารถลบส่วนใด ๆ ของเพลงได้เลย ฉะนั้นคำตอบจึงเป็น 

$$\sum\limits_{i=1}^{M-1}p[s_i][s_{i+1}]$$

Time Complexity: $\mathcal{O}(M)$

### Subtask 2 ($K=1$)

เราสามารถ loop เพื่อหาตำแหน่งของเพลงที่ลบแล้วทำให้เกิดค่าความเหนื่อยน้อยที่สุดได้ ฉะนั้นคำตอบจึงเป็น

$$\sum\limits_{i=1}^{M-1}p[s_i][s_{i+1}]-\max\limits_{1\leq i\leq M}\{p[s_{i-1}][s_i]+p[s_i][s_{i+1}]-p[s_{i-1}][s_{i+1}]\}$$

โดยกำหนดให้ $p[s_0][s_i]=p[s_i][s_{M+1}]=0$ สำหรับ $1\leq i\leq M$ อย่างไรก็ตาม คำตอบที่ถูกต้องอาจไม่จำเป็นต้องลบส่วนใด
ของเพลงเลยก็ได้

Time Complexity: $\mathcal{O}(M)$

### Subtask 3 ($N,M,K\leq 20$)

เราสามารถลองทุกวิธีในการลบลำดับของเพลง โดยมีเงื่อนไขว่าต้องลบไม่เกิน $K$ ตำแหน่งเท่านั้น

Time Complexity: $\mathcal{O}(2^M)$

### Subtask 4 (ไม่มีเงื่อนไขเพิ่มเติม)

ปัญหานี้สามารถแก้ได้ด้วย Dynamic Programming โดยเราจะสมมุติ $s_0=0$ และ $s_{M+1}=M+1$ เป็นลำดับของเพลงที่ต้องเล่นก่อนและหลังดีด $s_i$ ใด ๆ ตามลำดับ โดยที่ $p[s_0][s_i]=p[s_i][s_{M+1}]=0$ สำหรับ $1\leq i\leq M$

กำหนดให้ $dp(i,j)$ คือค่าความเหนื่อยที่น้อยที่สุดหลังจากเล่นลำดับของเพลงตั้งแต่ $s_0,s_1,s_2,\dots,s_i$ โดยที่ลบลำดับของเพลงไป $j$ ตำแหน่ง

Base Case:

$$dp(0,0)=0$$

Recurrence Formula:

$$dp(i,j)=\min\limits_{1\leq k\leq \min(i,j+1)}\{dp(i-k,j-k+1)+p[s_i−k][s_i]\}$$

คำตอบคือ $\min\limits_{0\leq i\leq K}dp(M+1,i)$

Time Complexity: $\mathcal{O}(NMK)$

### Solution Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 305;

int s[N];
long long p[N][N], dp[N][N];

int main()
{
    int n, m, k;
    long long ans = 1e18;

    scanf(" %d %d %d",&n,&m,&k);
    for(int i=1 ; i<=n ; i++) for(int j=1 ; j<=n ; j++) scanf(" %lld",&p[i][j]);
    for(int i=1 ; i<=m ; i++) scanf(" %d",&s[i]);

    for(int i=0 ; i<=m+1 ; i++) for(int j=0 ; j<=k ; j++) dp[i][j] = 1e18;
    dp[0][0] = 0;
    for(int i=0 ; i<=m ; i++) for(int j=0 ; j<=k ; j++) for(int l=0 ; j+l<=k && i+l+1<=m+1 ; l++) dp[i+l+1][j+l] = min(dp[i+l+1][j+l], dp[i][j] + p[s[i]][s[i+l+1]]);
    for(int i=0 ; i<=k ; i++) ans = min(ans, dp[m+1][i]);

    printf("%lld\n",ans);
    return 0;
}
```