#### กลุ่มชุดสอบที่ไม่มีการตัดไส้อั่ว (12 คะแนน)

**นิยาม** $dp(l, r)$ หมายถึง ความอร่อยมากที่สุดที่สามารถกินได้ในช่วง $l$ ถึง $r$ โดยไม่มีการตัดไส้อั่ว

$$
dp(l,r) = \begin{cases}
D_{l} & \text{; } l = r \\
|D_{l} - D_{r}| + \max(dp(l + 1, r) + D_l, dp(l, r - 1) + D_r) & \text{; } l < r
\end{cases}
$$

Time Complexity : $O(N^{2})$

#### กลุ่มชุดสอบที่ $N\le500$ (65 คะแนน)

ในชุดทดสอบกลุ่มนี้สามารถใช้ Matrix Chain Multiplication ในการลองไล่ตัดทุกวิธีได้

**นิยาม** $opt(l, r)$ หมายถึง ความอร่อยมากที่สุดที่สามารถกินได้ในช่วง $l$ ถึง $r$ โดยมีการตัดกี่ครั้งก็ได้

$$
opt(l, r) = \max\limits_{l \le k < r}\begin{cases}
dp(l, r) \\
opt(l, k) + opt(k + 1, r)
\end{cases}
$$

Time Complexity : $O(N^{3})$

#### ไม่มีเงื่อนไขเพิ่มเติม (100 คะแนน)

ในปัญหาย่อยนี้ต้อง optimize การหา $opt(l, r)$ ลงให้ใช้เวลาไม่เกิน $O(N^{2})$

ตั้งนิยามใหม่ให้กับ $opt$ โดย  
**นิยาม** $opt(r)$ หมายถึง ความอร่อยมากที่สุดที่สามารถกินได้ในช่วง $l$ ถึง $r$ โดยมีการตัดกี่ครั้งก็ได้

$$
opt(r) = \max\limits_{1 \le l < r}\begin{cases}
dp(1, r) \\
opt(l) + dp(l + 1, r)
\end{cases}
$$

Time Complexity : $O(N^{2})$

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MxN = 5050;
int n, dp[MxN][MxN], opt[MxN], D[MxN];
int main() {
	cin.tie(nullptr)->ios::sync_with_stdio(false);
	cin >> n;
	for(int i=1; i<=n; ++i){
		cin >> D[i];
	}
	for(int i=1; i<=n; ++i){
		dp[i][i] = D[i];
	}
	for(int sz=2; sz<=n; ++sz){
		for(int l=1; l+sz-1<=n; ++l){
			int r = l + sz - 1;
			dp[l][r] = abs(D[l] - D[r]) + max(dp[l + 1][r] + D[l], dp[l][r - 1] + D[r]);
		}
	}
	for(int r=1; r<=n; ++r){
		opt[r] = dp[1][r];
		for(int l=1; l<r; ++l){
			opt[r] = max(opt[r], opt[l] + dp[l + 1][r]);
		}
	}
	cout << opt[n];
	return 0;
}

```
