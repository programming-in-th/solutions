โจทย์ข้อนี้สามารถแก้ด้วยด้วย เทคนิค dynamic programming โดยนิยาม $dp(i)$ คือ เงินที่ต้องจ่ายน้อยที่สุดเมื่อคิดสถานีที่ $1$ ถึงสถานีที่ $i$ และแต่ละสถานีที่เลือกห่างไม่เกิน $K$ สถานี 
#### subtask 1 ( 60 คะแนน )
สามารถทำ dynamic programming ที่เมื่อถึงสถานีที่ $i$ ใดๆจะได้ $dp(i)$ คือ $min(dp(j))$ ที่$i-k<=j$ และ $j>0$ บวกกับค่า สถานีที่ $i$ โดย $min(dp(j))$ สามารถหาได้จากการไล่หาในทุกๆ $j$ ที่สอดคล้อง 
Time complexity $O(NK)$
#### subtask 2 ( 40 คะแนน )
สามารถoptimizeการหาค่าที่น้อยที่สุดได้ด้วยการใช้เทคนิค **minimum sliding window** จะสามารถลด Time complexity ลงได้เหลือเพียง $O(N)$

```cpp
#include <bits/stdc++.h>
#define pii pair<int, int>
#define x first
#define y second

using namespace std;

const int N = 5e5 + 10;
int n, k, dp[N], cost[N];
deque<pii> dq;
int main()
{
    scanf("%d %d",&n,&k);
    for( int i = 1 ; i <= n ; i++ ) scanf("%d",&cost[i]);
    for( int i = 1 ; i <= n ; i++ ) {
        while( !dq.empty() && i - dq.front().y > k ) dq.pop_front();
        if( i != 1 ) dp[i] = dq.front().x + cost[i];
        else dp[i] = cost[i];
        while( !dq.empty() && dq.back().x > dp[i] ) dq.pop_back();
        dq.push_back( pii( dp[i], i ) );
    }
    printf("%d",dp[n]);
}
```