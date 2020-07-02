
## Subtask 1 $(K = 0)$

เราสามารถหาคำตอบโดยใช้ *Hamiltonian Distance* ได้เลย  

## Subtask 2 $(N \leq 200, K \leq 100, Q \leq 300)$

เนื่องจากจำนวนคำถามของ Subtask นี้ไม่เยอะมาก เราสามารถหา *Shortest Path* โดยใช้ *Breadth First Search* ในแต่ละคำถามได้  

Time Complexity : $\mathcal{O}(QN^2)$
Space Complexity : $\mathcal{O}(N^2)$

## Subtask 3 $(N \leq 200, K \leq 100, Q \leq 20,000)$

*"กรณีใดที่เราสามารถมั่นใจได้ว่าเราสามารถหา Shortest Path โดยใช้ Hamiltonian Distance ได้เลย?"*  

สมมุติว่าเรากำลังพิจารณาจุด A กับ จุด B อยู่  

ลองพิจารณาสี่เหลี่ยมผืนผ้าที่เล็กที่สุดที่ครอบทั้งจุด A และจุด B เอาไว้ **หากไม่มีพุ่มไม้อยู่ในสี่เหลี่ยมพื้นผ้านี้ เราสามารถใช้ Hamiltonian Distance หาคำตอบได้เลย**  

*"แต่ถ้ามีพุ่มไม้อยู่ในสี่เหลี่ยมผืนผ้านั้นล่ะ?"*  

แสดงว่า **จะต้องมี Shortest Path อย่างน้อยหนึ่งเส้นที่สัมผัสกับอย่างน้อยพุ่มไม้หนึ่งพุ่มอย่างแน่นอน**  

ถ้าเราลองสร้างโหนดสมมุติ (Auxiliary Node) รอบแปดทิศของพุ่มไม้ทุกพุ่ม **จะได้ว่า Shortest Path อย่างน้อยหนึ่งเส้นจะผ่านหนึ่งในโหนดเหล่านี้**  

ลำดับเหตุการณ์ของเส้น *Shortest Path* ดังกล่าว จะมีลักษณะดังนี้ (สมมุติให้จุด A เป็นจุดเริ่ม และจุด B เป็นจุดจบ)
* จากจุด A เดินทางไปยังโหนดสมมุติ โหนดใดโหนดหนึ่ง 
* เดินจากโหนดสมมุติ ไปมาระหว่างโหนดสมมุติด้วยกันเอง (หรือไม่เดินทางเลยก็ได้)
* เดินทางจากโหนดสมมุติมายังจุด B

### จาก A ไปยังโหนดสมมุติ และ จากโหนดสมมุติมายัง B

แน่นอนว่าการจะคำนวณ *Shortest Path* จากจุด A ไปยังโหนดสมมุติ หรือจากโหนดสมมุติมายัง B นั้น ให้เราจับคู่เฉพาะคู่โหนดที่สี่เหลี่ยมผืนผ้าไม่มีพุ่มไม้และโหนดสมมุติอื่น ๆ อยู่เลยก็เพียงพอ เพราะจะมี *Shortest Path* อย่างน้อยหนึ่งเส้นที่ผ่านโหนดเหล่านี้อย่างแน่นอน แล้วคำนวณ *Shortest Path* ด้วย *Hamiltonian Distance* 

### จากโหนดสมมุติไปยังโหนดสมมุติด้วยกัน 

สังเกตุได้ไม่ยากว่า *Shortest Path* ของคู่โหนดใด ๆ ในสวนตารางนี้จะไม่เกิน $2n+4k$ อย่างแน่นอน ซึ่งมีค่าน้อยมาก ๆ ใน subtask นี้ เราสามารถหา *Shortest Path* ด้วย *Dijkstra's Algorithm* **โดยไม่ต้องพึ่ง priority queue ได้**  

*"แต่เราจะเชื่อม edge ให้แต่ละคู่ของโหนดอย่างไรล่ะ?"*  

เราจะเชื่อม edge ให้กับเฉพาะกับคู่โหนดที่เรารู้ระยะทางแน่นอน หรือก็คือ คู่โหนดที่สี่เหลี่ยมผืนผ้าไม่มีพุ่มไม้หรือโหนดอื่นนอกจากคู่โหนดนี้ น้ำหนักของ edge จะเป็น *hamiltonian distance* ของทั้งคู่  
```cpp
for (int i = 1; i <= aux_cnt; ++i) {
  for (int j = i + 1; j <= aux_cnt; ++j) {
    if (rect(aux[i], aux[j]) == 2) {
      // if there is nothing else in the rectangle
      // add an edge between (i,j)
      // the weight is the hamiltonian distance
      add_edge(i, j, hamilton(aux[i], aux[j]));
    }
  }
}
```
สังเกตุได้ไม่ยากเช่นกันว่า การเชื่อม edge ด้วยวิธีนี้จะมีจำนวน edge รวมไม่เกิน $3{k \choose 2}$ เส้น  

ด้านล่างนี้เป็นโค้ดตัวอย่างของการหา *Shortest Path* จาก A ไปยัง B  

```cpp
vector<int> dijk[801]; // dijkstra without heap
int SP[808];           // shortest path from A to auxiliary nodes
void subtask3(PII A, PII B, int a, int b) {
  for (int i = 1; i <= aux_cnt; ++i)
    SP[i] = 1 << 29; // inf
  // a = 1 if A is an auxiliary node
  // b = 1 if B is an auxiliary node

  if (a) { // if A, ifself, is an auxiliary node
    // let's start on that node
    SP[point[A]] = 0;
    dijk[0].pb(point[A]);
  } else {
    // let's try walking from A
    // to auxiliary nodes
    // that we can use hamilton distance
    for (int i = 1; i <= aux_cnt; ++i) {
      if (rect(A, aux[i]) == 1) {
        // there is only one auxiliary node in the rectangle
        SP[i] = hamilton(A, aux[i]);
        dijk[SP[i]].pb(i);
      }
    }
  }
  // dijkstra
  for (int i = 0; i <= 800; ++i) {
    // 800 is the maximum of 2*n+4*k
    // 'i' is the distance
    for (int e : dijk[i]) {
      if (SP[e] != i)
        continue;
      for (PII f : g[e]) {
        if (SP[f.x] > i + f.y) {
          SP[f.x] = i + f.y;
          dijk[SP[f.x]].pb(f.x);
        }
      }
    }
    dijk[i].clear();
  }
  int ans = 1 << 29; // infinity
  // walking from auxiliary nodes to B
  // similar to A
  if (b)
    ans = SP[point[B]]; // we already have the answer
  else {
    // try walking from auxiliary nodes
    // that we can use hamilton distance
    FOR(i, 1, aux_cnt) {
      if (rect(B, aux[i]) == 1) {
        // there is only one auxiliary node in the rectangle
        ans = min(ans, hamilton(B, aux[i]) + SP[i]);
      }
    }
  }
  // answering
  if (ans == (1 << 29))
    printf("-1");
  else
    printf("%d\n", ans);
}
```  

ทั้งนี้ การตรวจสอบดูว่าในสี่เหลี่ยมผืนผ้าของคู่โหนดใด ๆ มีโหนดสมมุติหรือพุ่มไม้อยู่ทั้งหมดเท่าไร เราสามารถทำตาราง *quicksum* สองมิติเอาไว้ในเวลา $\mathcal{O}(n^2)$ แล้วนำมาหาผลรวมภายในสี่เหลี่ยมผืนผ้าได้ในเวลา $\mathcal{O}(1)$  

Time Complexity : $\mathcal{O}(n^2+(8k)^2+Q\cdot 3{k \choose 2}))$  
Space Complexity : $\mathcal{O}(n^2+3{k \choose 2})$  

## Subtask 4 $(N \leq 10^5, K \leq 30, Q \leq 20,000)$

ใน subtask นี้ การเก็บตาราง *quicksum* สองมิติตรง ๆ จะไม่สามารถทำได้ แต่เนื่องจากจำนวนโหนดสมมุติที่สร้างขึ้นรอบแปดทิศของพุ่มไม้จะมีไม่เกิน $8k$ โหนด ดังนั้นเราสามารถทำ **coordinate compression** ได้ ทำให้ตาราง *quicksum* สองมิติของเราเหลือขนาดเพียง $3k \cdot 3k$ เท่านั้น  

ดังที่กล่าวไปเมื่อก่อนหน้านี้ว่า *Shortest Path* ของคู่โหนดใดๆ จะมีค่าไม่เกิน $2n+4k$ ก้าว ซึ่งคราวนี้มีค่าเยอะมาก ๆ จนเราสามารถใช้ *Dijkstra's Algorithm* โดยไม่พึ่งพา *priority queue* ได้แล้ว เพราะหากทำเช่นเดิมจะไม่ทันเวลา  

**แต่** ใน subtask นี้ค่า $k$ มีเพียงไม่เกิน 30 เท่านั้น ทำให้เราสามารถใช้ *Dijkstra's Algorithm* โดยใช้ *priority queue* ช่วยแล้วยังทันเวลาได้ใน $\mathcal{O}(3{k \choose 2} \log 3{k \choose 2})$ นั่นเอง  

```cpp
priority_queue<PII> heap;
void subtask4(PII A, PII B, int a, int b) {
  for (int i = 1; i <= aux_cnt; ++i)
    SP[i] = 1 << 29; // inf
  if (a) {
    SP[point[A]] = 0;
    heap.push(PII(0, point[A]));
  } else {
    for (int i = 1; i <= aux_cnt; ++i) {
      if (rect(A, aux[i]) == 1) {
        SP[i] = hamilton(A, aux[i]);
        heap.push(PII(-SP[i], i));
      }
    }
  }
  while (!heap.empty()) {
    int u = heap.top().y;
    int w = -heap.top().x;
    heap.pop();
    if (SP[u] != w)
      continue;
    for (PII e : g[u]) {
      if (SP[e.x] > e.y + w) {
        SP[e.x] = e.y + w;
        heap.push(PII(-SP[e.x], e.x));
      }
    }
  }
  int ans = 1 << 29;
  if (b)
    ans = SP[point[B]];
  else {
    for (int i = 1; i <= aux_cnt; ++i) {
      if (rect(B, aux[i]) == 1) {
        ans = min(ans, SP[i] + hamilton(aux[i], B));
      }
    }
  }
  if (ans == (1 << 29))
    printf("-1\n");
  else
    printf("%d\n", ans);
}
```

Time Complexity : $\mathcal{O}((3k)^2+(8k)^2+Q\cdot 3{k \choose 2} \log 3{k \choose 2})$  
Space Complexity : $\mathcal{O}((3k)^2+3{k \choose 2})$  
