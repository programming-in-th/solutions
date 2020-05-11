ในข้อนี้หากเราพิจารณาค่า $m$ ใดๆ โดยมีฟังก์ชัน $F(m)$ คือฟังก์ชันแทน ความถี่ของ $m$ จะสังเกตุว่าที่ $m$ และ $m+1$ คู่อันดับ $S(i,j)$ ใดๆที่มีค่ามากกว่า $m+1$ นั้นย่อมมากกว่า $m$ ด้วยดังนั้น $F(m+1)$ นั้นไม่มากไปกว่า $F(m)$ และถ้ามีคู่อันดับใดๆที่ $S(i,j) = m$ แล้วจะทำให้ค่าของ $F(m)$ มีค่ามากกว่า $F(m+1)$ ดังนั้น $F(m+1) \leq F(m)$ เสมอ จากเหตุผลดังกล่าวจะได้ว่า ฟังก์ชัน $F(m)$ เป็นฟังก์ชัน *monotone* ดังนั้นการหาค่า $m$ ที่มากที่สุดที่เป็นคำตอบสามารถหาได้ด้วยการ *binary search* ค่า $m$ โดยตรง
```cpp 
while (l < r) {
  long long mid = (l + r + 1) / 2;
  if (F(mid) >= k)
    l = mid;
  else
    r = mid - 1;
}
```
* $F(mid) \geq k$ นั่นหมายความว่าคำตอบที่ต้องการนั้นมีค่ามากกว่าหรือเท่ากับ mid แน่นอน จึงเลื่อน *lower bound* คำตอบไปยัง `mid`
* $F(mid) < k$ นั่นหมายความว่าคำตอบที่ต้องการนั้นน้อยกว่า `mid` แน่นอน จึงเลื่อน *upper bound* ไปยัง `mid-1` 

**วิธีในการคำนวน $F(m)$** สามารถทำได้ตรงๆด้วยการไล่ในทุกๆคู่อันดับ $(i,j)$ ได้แต่วิธีนี้มี Time-complexity $\mathcal{O}(N^2\log N)$ ซึ่งวิธีนี้ไม่ทันเวลา ซึ่งวิธีการ optimize นั้นทำได้โดยการ preprocess **prefix sum** ของทุกๆ index $i$ ที่ $i \leq n$ โดย $pref(i) = pref(i-1) + a(i)$ จากการ preprocess prefix sum จะได้ว่า $S(i,j) = pref(j) - pref(i-1)$
```cpp
pref[i] = a[i] + pref[i - 1];
```
ดังนั้นเราจะได้ว่าคู่อันดับ $(i,j)$ ที่เราต้องการนั้นจะมีสมบัติคือ $m \leq pref(j) - pref(i-1)$ หรือก็คือ $pref(i-1) \leq pref(j) - m$ 
ให้ $G(j)$ คือฟังก์ชันแทนจำนวน คู่อันดับ $(i,j)$ ที่ $i \leq j$ และ $m \leq S(i,j)$ เราสามารถนับคำตอบ $G(j)$ ด้วยการ update ค่า $pref(1),pref(2),pref(3),pref(4),...,pref(j-1)$ และนับจำนวนของค่า $pref(i)$ ที่ $pref(i) \leq pref(j) - m$ เนื่องจากค่า $pref(i)$ นั้นอาจมีค่ามากหรือติดลบได้ เราจึงนำเทคนิค *coordinate compression* มาช่วย
```cpp
for (int i = 1; i <= n; i++)
  coord.emplace_back(pref[i]);
coord.emplace_back(0);
sort(coord.begin(), coord.end());
coord.resize(unique(coord.begin(), coord.end()) - coord.begin());
```
การ update ค่านั้นหากนำค่าไปใส่ในช่องตรงๆ ด้วย time complexity $\mathcal{O}(1)$ และ query ไล่ผลรวมตั้งแต่ $1$ ถึง $j-1$ ด้วย time complexity $\mathcal{O}(N)$ จะทำให้ time complexity รวมเป็น $\mathcal{O}(N^2\log N)$ เหมือนเดิม ด้วยการใช้โครงสร้างข้อมูล *fenwick tree* สามารถ optimize time complexity ทั้ง update และ query เป็น $\mathcal{O}(\log N)$ 
```cpp
int F(int mid) {
  long long now = 0;
  memset(t, 0, sizeof t);
  update(get(0), 1);
  for (int i = 1; i <= n; i++) {
    int pos = lower_bound(coord.begin(), coord.end(), pref[i] - mid) -
              coord.begin() + 1;
    if (coord[pos - 1] > pref[i] - mid)
      --pos;
    if (pos)
      now += query(pos);
    update(get(pref[i]), 1);
  }
  return now;
}
```
จากการใช้โครงสร้างข้อมูล fenwick tree เข้ามาช่วยสามารถลด time complexity รวมลงได้เหลือ $\mathcal{O}(N\log^2 N)$ 
