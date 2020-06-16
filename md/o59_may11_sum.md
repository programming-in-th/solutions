ในข้อนี้หากเราพิจารณาค่า $m$ ใด ๆ โดยนิยามให้ $F(m)$ หมายถึงฟังก์ชันแทนความถี่ของ $m$ จะสังเกตว่า $F(m) \geq F(m+1)$ เสมอ เพราะถ้าคู่อันดับ $(i,j)$ ใด ๆ ถูกนับรวมใน $F(m+1)$ แล้วแปลว่า $S(i,j) \geq m+1$ แล้วคู่อันดับนั้นก็ย่อมถูกนับรวมใน $F(m)$ ด้วย เพราะ $S(i,j) \geq m+1 \geq m$ จากเหตุผลดังกล่าวจะได้ว่า ฟังก์ชัน $F(m)$ เป็นฟังก์ชัน monotone ดังนั้นการหาค่า $m$ ที่มากที่สุดที่เป็นคำตอบสามารถหาได้ด้วยการ binary search ค่า $m$ โดยตรง

ระหว่างการทำ binary search หากกำลังพิจารณาคำตอบในช่วง $[l,r]$ อยู่โดย `mid = (l+r)/2` จะมีกรณีที่ต้องพิจารณาเพื่อเปลี่ยนช่วงคำตอบ 2 กรณีดังนี้ 
* $F(mid) \geq k$ นั่นหมายความว่าคำตอบที่ต้องการนั้นมีค่ามากกว่าหรือเท่ากับ `mid` แน่นอน จึงเปลี่ยนช่วงไปพิจารณาช่วง $[mid,r]$ ต่อไป
* $F(mid) < k$ นั่นหมายความว่าคำตอบที่ต้องการนั้นน้อยกว่า `mid` แน่นอน จึงเปลี่ยนช่วงไปพิจารณาช่วง $[l,mid-1]$ ต่อไป

วิธีในการคำนวณ $F(m)$ สามารถทำได้ตรง ๆ ด้วยการไล่นับคู่อันดับ $(i,j)$ ที่ $S(i,j) \geq m$ ได้แต่วิธีนี้มี Time complexity $\mathcal{O}(N^2)$ ต่อการคำนวณ $F(m)$ 1 ครั้ง และเมื่อรวมจำนวนครั้งที่คิด $F(m)$ โดยจากการ binary search จำเป็นต้องคิด $F(m)$ ทั้งหมด $\log N$ ครั้ง ดังนั้นจึงได้ Time complexity $\mathcal{O}(N^2\log N)$ ซึ่งไม่ทันเวลา วิธีการ optimize นั้นเริ่มได้จากการ preprocess prefix sum ของทุก ๆ index $i$ ที่ $i \leq n$ โดยนิยามให้ $pref(i) =$ ผลรวมของ $A(1),A(2),...,A(i)$ จากการ preprocess prefix sum จะได้ว่า $S(i,j) = pref(j) - pref(i-1)$ ดังนั้นคู่อันดับ $(i,j)$ ที่เราต้องการนั้นจะมีคุณสมบัติคือ $m \leq pref(j) - pref(i-1)$ หรือก็คือ $pref(i-1) \leq pref(j) - m$ 

โดยวิธีในการคำนวณ $F(m)$ ใด ๆ นั้นเริ่มได้จากการพิจารณาตั้งแต่ $j = 1,2,3,...,n$ โดยสำหรับ $j$ ค่าหนึ่งจะนิยามให้ $G(j)$ หมายถึงฟังก์ชันแทนจำนวนคู่อันดับ $(i,j)$ ที่ $i \leq j$ และ $pref(i-1) \leq pref(j) - m$ เราสามารถคำนวณ $G(j)$ โดยเริ่มจากการกำหนด array $X$ โดยนิยามให้ $X(i)$ หมายถึงจำนวนของค่า $i$ และในตอนเริ่มต้น $X(i) = 0$ ในทุก ๆ $i$ จากนั้น update array $X$ ใน index $pref(1),pref(2),pref(3),pref(4),...,pref(j-1)$ ด้วยการเพิ่มค่าช่องนั้น ๆ ไป 1 การทำแบบนี้ทำให้เราสามารถคำนวณค่า $G(j)$ ได้สำหรับทุก ๆ $j$ ด้วยการหาผลรวม $...+X(-1)+X(0)+X(1)+...+X(m-pref(j))$ ดังนั้น หากพิจารณา $j = 1,2,3,...n$ แล้วนำคำตอบมารวมกัน จะได้เป็นความถี่ของ $m$ นี่เรากำลังค้นหาอยู่ 

เนื่องจากค่า $pref(i)$ นั้นอาจมีค่ามากหรือติดลบได้ เราจึงนำเทคนิค coordinate compression มาช่วย โดยเทคนิคนี้จะเก็บข้อมูลไว้ใน vector `coord` และใช้การ sort ข้อมูลทั้งหมด และนำค่าซ้ำออก เมื่อค่าใด ๆ ถูกเรียกใช้งานจะแทนค่าค่านั้นด้วย index ใน vector `coord` แทน ดังนั้นปัญหาค่าติดลบหรือมากเกินว่าจะนำไปเป็น index ของ array จะหมดไป
```cpp
for (int i = 1; i <= n; i++)
  coord.emplace_back(pref[i]);
//Coordinate compression
coord.emplace_back(0); // base case
sort(coord.begin(), coord.end());
coord.resize(unique(coord.begin(), coord.end()) - coord.begin());
```
การ update ค่านั้นหากนำค่าไปใส่ในช่องตรง ๆ ด้วย time complexity $\mathcal{O}(1)$ และ query ไล่ผลรวมตั้งแต่ $1$ ถึง $j-1$ ด้วย time complexity $\mathcal{O}(N)$ จะทำให้ time complexity ต่อการดำเนินการต่อ j ใด ๆ เป็น $\mathcal{O}(N)$ และ time complexity ในการคิด $F(m)$ 1 ครั้งเป็น $\mathcal{O}(N^2)$ และเมื่อคิดการคำนวณทุก ๆ ครั้งจะมี time complexity เป็น $\mathcal{O}(N^2\log N)$ เหมือนเดิม ด้วยการใช้โครงสร้างข้อมูล fenwick tree สามารถ optimize time complexity ทั้ง update และ query ต่อการดำเนินการ 1 ครั้งเป็น $\mathcal{O}(\log N)$ ในการคิด $F(m)$ 1 ครั้งจะลด time complexity เหลือเพียง $\mathcal{O}(N\log N)$ และเมื่อรวมเวลาการคำนวณทุกครั้งได้ time complexity เป็น $\mathcal{O}(N\log^2 N)$
```cpp
void update(int x, long k) {
  for (int i = x; i < N; i += i & -i)
    t[i] += k;
}

long long query(int x, long ret = 0) {
  for (int i = x; i; i -= i & -i)
    ret += t[i];
  return ret;
}

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
