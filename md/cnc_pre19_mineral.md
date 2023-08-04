### Subtask 1 (4 คะแนน) $A[i]=B[i]$

เนื่องจาก $A[i]=B[i]$ ไม่ว่าเราจะเพิ่มหรือลดช่วงอย่างไรจะได้ว่า $\sum\limits_{i=L}^RA[i]=\sum\limits_{i=L}^RB[i]$ เสมอ ดังนั้น $\frac{\sum\limits_{i=L}^RA[i]}{\sum\limits_{i=L}^RB[i]}=1.000$

Time Complexity: $\mathcal{O}(1)$

### Subtask 2 (9 คะแนน) $N\leq 1\,000$

ในปัญหาย่อยนี้สามารถใช้วิธีการ Brute Force ในการไล่ตรวจสอบทุกช่วงที่เป็นไปได้จากนั้นนำมา sort แล้วหาลำดับที่ K เพื่อตอบได้

Time Complexity: $\mathcal{O}(N^2\log N^2)$

### Subtask 3 (7 คะแนน) $K=1$

ในปัญหาย่อยนี้สิ่งหลักที่ต้องสังเกตคือจะมีแร่ $i$ ที่คุ้มค่าที่สุด นั่นคือแร่ที่มี $\frac{A[i]}{B[i]}$ มากที่สุด และหากเพิ่มแร่ชนิดอื่นเข้าไปจะทำให้มูลค่าลดลงเสมอ สมมติว่าเราต้องการเพิ่มแร่ $j$ ให้ขุดพร้อมกับแร่ $i$ เนื่องจากแร่ $i$ คุ้มค่าที่สุด เราจะได้ว่า $\frac{A[i]}{B[i]}\geq\frac{A[j]}{B[j]}$ ซึ่งเมื่อย้ายข้างอสมการและบวก $A[i]B[i]$ ไปทั้งสองข้างจะได้ว่า $A[i]B[i]+A[i]B[j]\geq B[i]A[i]+B[i]A[j]$ เป็นจริง และได้ว่า $\frac{A[i]}{B[i]}\geq\frac{A[i]+A[j]}{B[i]+B[j]}$ เป็นจริงด้วย ดังนั้นเราสามารถไล่หาแร่ที่คุ้มค่าที่สุด และตอบมูลค่าของแร่นั้นได้เลย

Time Complexity: $\mathcal{O}(N)$

### Subtask 4, 5 (12 + 11 คะแนน) $K\leq1\,000$

จากข้อสังเกตในปัญหาย่อยที่ 3 จะได้ว่าค่าที่มากที่สุดลำดับที่ $K$ จะมีแร่อยู่ไม่เกิน $K$ ชนิด ดังนั้นเราสามารถไล่ Brute Force และ bound ให้ช่วงมีขนาดไม่เกิน $K$ ได้ แต่ต้องมีการ optimize memory เพื่อไม่ให้ใช้ memory เยอะจนเกินไป เนื่องจากปัญหาต้องการค่าที่มากที่สุด ลำดับที่ $K$ จึงสามารถใช้ `std::priority_queue` ในการ maintain จำนวนที่มากที่สุด $K$ อันดับแรกได้ และหากใน priority_queue มีสมาชิกมากกว่า $K$ ตัวก็สามารถ pop สมาชิกที่มีค่าน้อยที่สุดออกได้

Time Complexity: $\mathcal{O}(NK\log(NK))$

### Subtask 6 (20 คะแนน) $A[i]$ เท่ากันหมด, $B[i]\leq B[i+1]$

ในการแก้ปัญหานี้เราจะทำการ Binary Search บนคำตอบหรือก็คือมูลค่าที่มากที่สุดลำดับที่ K ให้มูลค่านั้นเป็น $x$ หากเราเลือก $L$ ใด ๆ จะได้ว่า $\frac{\sum\limits_{i=L}^R A[i]}{\sum\limits_{i=L}^R B[i]}$ จะเป็นลำดับลดลงตาม $R$ ที่เพิ่มขึ้น ดังนั้นเราสามารถ binary search อีกครั้งเพื่อหาว่าหากเลือกค่า L แล้ว สามารถเลือกค่า $R$ ได้มากที่สุดเท่าไหร่จึงจะไม่น้อยกว่า x จากนั้นบวกคำตอบของทุก ๆ ค่า $L$ ว่าจำนวนช่วงที่เป็นไปได้มากกว่าหรือเท่ากับ $K$ หรือไม่ หากมากกว่าก็สามารถให้ $x$ เป็นคำตอบได้หรือหากน้อยกว่าก็ไม่สามารถให้ $x$ เป็นคำตอบได้

Time Complexity: $\mathcal{O}(N\log_2 N)$

### Official Solution

เราจะทำการ Binary Search บนคำตอบของปัญหาข้อนี้ โดยเราจะพิจารณาว่า $x$ สามารถเป็นคำตอบของปัญหานี้ได้ก็ต่อเมื่อมีช่วง [L, R] ซึ่งเป็นค่าที่มากที่สุดลำดับที่ $K$ บางช่วงที่สอดคล้องกับสมการ

$$\frac{\sum\limits_{i=L}^RA[i]}{\sum\limits_{i=L}^RB[i]}\geq x$$

และเนื่องจาก $\sum\limits_{i=L}^RB[i]>0$ ดังนั้นเราสามารถย้ายข้างอสมการและได้ว่า $\sum\limits_{i=L}^RA[i]-x\sum\limits_{i=L}^R\geq 0$ ซึ่งเมื่อแทนค่าในอสมการเป็น $X[i]=A[i]-xB[i]$ จะสามารถเขียนอสมการดังกล่าวใหม่ให้อยู่ในรูป $\sum\limits_{i=L}^RX[i]\geq 0$ ได้ ดังนั้นเราลดรูปปัญหาให้อยู่ในรูปของการหาช่วง $[L,R]$ ที่เป็นช่วงมากสุดลำดับที่ $K$ ของ $X[i]$ และตรวจสอบว่า $\sum\limits_{i=L}^RX[i]\geq 0$ หรือไม่

ในการหาว่ามีช่วงดังกล่าวอยู่หรือไม่ เราจะทำการหาว่ามีช่วงที่มากกว่าหรือเท่ากับ $0$ อยู่ทั้งหมดกี่ช่วง จากนั้นเทียบกับค่า $K$ จะทำให้รู้ว่าช่วงที่มากที่สุดลำดับที่ $K$ มากกว่าหรือน้อยกว่า $0$ นั่นเอง โดยเมื่อเราแทนให้ $prefix\_X[i]$ แทน prefix sum ของ $X[i]$ แล้วช่วง $[L,R]$ จะมีค่ามากกว่าหรือเท่ากับ $0$ ก็ต่อเมื่อ $prefix\_X[R]-prefix\_X[L-1]\geq 0$ และ $R\geq L$ เท่านั้นซึ่งปัญหานี้ก็จะตรงกับปัญหา classic อย่างการ count inversion ที่ใช้ divide and conquer ในลักษณะเดียวกันกับ merge sort algorithm ในการแก้ปัญหาได้ใน $O(N\log N)$

Time Complexity: $\mathcal{O}(N\log_2 N)$

### Alternative Solution

อีกวิธีในการแก้ปัญหา count inversion สามารถใช้ Fenwick Tree หรือ Binary Indexed Tree มาช่วยในการแก้ไขปัญหานี้ได้ โดย Fenwick Tree เป็นเนื้อหาที่อยู่ในค่ายสสวท. ค่าย 1 แต่สามารถแก้ปัญหานี้ได้ใน Time Complexity $O(N\log N)$ ซึ่งเท่ากับการเขียน merge sort

Time Complexity: $O(N\log_2 N)$

```cpp
#include <bits/stdc++.h>
using namespace std;
long long A[100001], B[100001], X[100001];
long long cnt_inv = 0;

void merge(int l, int r, int mid){
  vector<long long> L, R;
  for(int i = l; i <= mid; i++) L.push_back(X[i]);
  for(int i = mid + 1; i <= r; i++) R.push_back(X[i]);
  
  int lidx = 0, ridx = 0, idx = l;
  while(lidx < L.size() && ridx < R.size()){
    if(L[lidx] > R[ridx]) X[idx++] = L[lidx++];
    else{
      cnt_inv += (L.size() - lidx) * 1ll;
      X[idx++] = R[ridx++];
    }
  }
  while(ridx < R.size()) X[idx++] = R[ridx++];
  while(lidx < L.size()) X[idx++] = L[lidx++];
}

void count_inversion(int l, int r){
  if(l == r) return;

  int mid = (l + r) >> 1;
  count_inversion(l, mid);
  count_inversion(mid + 1, r);
  merge(l, r, mid);
}

int main(int argc, char* argv[]){
  long long N, K;
  scanf("%lld %lld", &N, &K);
  for(int i = 1; i <= N; i++){
    scanf("%lld", &A[i]);
    A[i] *= (1000 * 1ll);
  }
  for(int i = 1; i <= N; i++) scanf("%lld", &B[i]);

  long long L = 1, R = 1e7;
  while(L < R){
    long long mid = (L + R + 1) >> 1;
    X[0] = 0;
    for(int i = 1; i <= N; i++) 
      X[i] = A[i] - (mid * B[i]) + X[i - 1];

    cnt_inv = 0;
    count_inversion(0, N);

    if(cnt_inv >= K) L = mid;
    else R = mid - 1;
  }
  double ans = double(L) / double(1000);
  printf("%.3lf", ans);
  return 0;
}
```