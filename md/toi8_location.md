โจทย์ข้อนี้สามารถแก้ได้ด้วยเทคนิค dynamic programming โดยนิยาม $dp(i,j)$ คือผมรวมของทุกตำแหน่ง $(x,y)$ ที่ $1<=x<=i$ และ $1<=y<=j$ จะได้ $dp(i,j) = dp(i-1,j) + dp(i,j-1) - dp(i-1,j-1) +$ ค่าช่อง $(i,j)$ โดย preprocess ค่าของทุกๆ $(i,j)$ เอาไว้ จากนั้นการหาค่ามากที่สุดในทุกๆสี่เหลี่ยมขนาด $K$ โดยสี่เหลี่ยมขนาด $K$ ใดที่มีมุมขวาล่างอยู่ที่ตำแหน่ง $(i,j)$ สามารถหาได้จากการนำ $dp(i,j)-dp(i-k,j)-dp(i,j-k)+dp(i-k,j-k)$ ก็จะสามารถหาคำตอบได้โดยมี Time complexity $O(NM)$

```cpp
#include <bits/stdc++.h>

using namespace std;

int m, n, k, sum[1010][1010], ans;

int main()
{
    scanf("%d %d %d",&m,&n,&k);
    for( int i = 1 ; i <= m ; i++ ){
        for( int j = 1 ; j <= n ; j++ ){
            scanf("%d",&sum[i][j]);
            sum[i][j] += sum[i-1][j] + sum[i][j-1] - sum[i-1][j-1];
        }
    }
    for( int i = k ; i <= m ; i++ ){
        for( int j = k ; j <= n ; j++ ){
            ans = max( ans, sum[i][j] - sum[i-k][j] - sum[i][j-k] + sum[i-k][j-k] );
        }
    }
    printf("%d",ans);
}
```
