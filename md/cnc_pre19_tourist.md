### Subtask 1 (9 คะแนน) $N,M,K,Q\leq 1\,000,L[i]\leq 1,R[i]\leq K$

ในปัญหาย่อยนี้ สังเกตว่า $L[i]=1$ และ $R[i]=K$ ทำให้เราสามารถพิจารณาเส้นเชื่อมเป็นเพียงแค่ผ่านได้หรือผ่านไม่ได้เท่านั้น กล่าวคือเมื่อมีคำสั่งรูปแบบที่ 1 บนถนนใด ๆ ก็ตาม เราจะพิจารณาว่าถนนเส้นดังกล่าวไม่สามารถผ่านได้เลยสำหรับนักท่องเที่ยวทุก ๆ กลุ่ม

เนื่องจาก $N,M,Q$ มีค่าน้อย จึงทำให้เราสามารถตรวจสอบการเดินทางหากันของเมืองสองเมืองได้โดยการ breadth-first search โดยตรง หากทั้งสองเมืองสามารถเดินทางหากันได้ให้เราตอบจำนวนกลุ่มนักท่องเที่ยวทั้งหมด และหากไม่ได้ให้ตอบ 0 ทั้งนี้จำนวนกลุ่มนักท่องเที่ยวทั้งหมดสามารถคำนวณได้หลายวิธี ในเบื้องต้นอาจใช้ `std::set` ในการนับจำนวนหมายเลขที่แตกต่างกันทั้งหมดก็ได้เช่นกัน

Time Complexity: $\mathcal{O}((N+M)\times Q)$

### Subtask 2 (19 คะแนน) $N,M,K,Q\leq 25\,000,L[i]=1,R[i]=K$

**ข้อสังเกต #1** การที่เส้นเชื่อมค่อย ๆ ถูกปิดไม่ให้ผ่านก็เหมือนกับการเปิดเส้นเชื่อมให้ค่อย ๆ ผ่านได้บนการคำนวณแบบย้อนกลับบนคำสั่งทั้งหมด

การคำนวณในรูปแบบนี้สามารถช่วยให้แก้โจทย์ได้ง่ายขึ้น เพราะเราสามารถนำโครงสร้างข้อมูล Disjoint Set มาใช่ในการ Union สถานีเข้าด้วยกันได้ และการตรวจสอบการเดินทางหากันสามารถดูจาก component ของสถานีที่เชื่อมกันได้โดยตรง

ดังนั้น การแก้ปัญหาย่อยนี้สามารถทำได้โดยการเชื่อมทางรถไฟทั้งหมดที่ไม่ถูกปิดตลอด $Q$ คำสั่ง และประมวลผลคำสั่งจากหลังมาหน้าด้วยการทยอยเชื่อมทางรถไฟและตอบคำถามด้วยการใช้ Disjoint Set Union ที่ได้กล่าวไว้ข้างต้น

อย่างไรก็ตาม คำสั่งปิดทางรถไฟอาจเกิดขึ้นหลายครั้งบนทางรถไฟเดียวกันได้ เราจึงต้องสร้างอาเรย์เพื่อเก็บค่าไว้ตรวจสอบสถานะการเปิดของทางรถไฟด้วย (ตัวอย่างเช่น ทางรถไฟหมายเลข $1$ ถูกปิด $5$ ครั้งตลอดทั้ง $Q$ คำสั่ง เราต้องเก็บ $closed[1]=5$ และทุกครั้งที่ผ่านคำสั่งปิดทางรถไฟหมายเลข $1$ จากการประมวลผลย้อนกลับ เราจะค่อย ๆ ลดค่า $closed[1]$ ลงไปทีละหนึ่ง แล้วค่อยทำการเชื่อมทางรถไฟเมื่อ
$closed[1]=0$)

Time Complexity: $\mathcal{O}((M+Q)\log N)$

### Subtask 3 (11 คะแนน) $N,M,K,Q\leq25\,000,R[i]=K,S[i]=1,$ และคำสั่งรูปแบบที่ 1 ทั้งหมดมาก่อนคำสั่งรูปแบบที่ 2

สังเกตว่าเงื่อนไข $R[i]=K$ ทำให้เราทราบว่ากลุ่มนักท่องเที่ยวที่มีคนมากกว่าหรือเท่ากับ $L[i]$ จะไม่สามารถเดินทางผ่านทางรถไฟดังกล่าวได้เลย เนื่องจากว่าคำสั่งรูปแบบที่ 1 ทั้งหมดมาก่อนคำสั่งรูปแบบที่ 2 ทำให้เราสามารถประมวลผลคำสั่งบนทางรถไฟทั้งหมดก่อนหาคำตอบได้ ซึ่งเราจะสามารถ label น้ำหนักบนทางรถไฟที่ $i$ เป็น $w[i]=\min(L[i]$ ของคำสั่งทั้งหมดบนทางรถไฟ $i)$ และ maximize ค่า $\min(w[i]$ บน path$)$ ของทุก ๆ path จาก $S[i]$ ไป $E[i]$ นอกจากนี้ เนื่องจาก $S[i]=1$ เราสามารถมองปัญหาอยู่ในรูปของ single source shortest path ได้ โดยใช้ Dijkstra's Algorithm โดยคำนวณ $distance[i]=\max(\min(distance[x],w(x,i)$ สำหรับทุก ๆ สถานี $x$ ที่ติดกับ $i))$ เมื่อ $w(x,i)$ คือน้ำหนักทางรถไฟจาก $x$ ไป $i$

Time Complexity: $\mathcal{O}(M\log N+Q)$

### Subtask 5 (17 คะแนน) $N,M,K,Q\leq 25000,A[i]\leq 100$

ความน่าสนใจของปัญหาย่อยนี้คือเรามีค่า $A[i]$ ที่แตกต่างกันไม่เกิน $100$ ค่า ซึ่งเป็นจำนวนที่เพียงพอต่อการคำนวณ brute force บนกลุ่มของนักท่องเที่ยวที่เป็นไปได้ทั้งหมด ในการแก้ปัญหาย่อยนี้จึงสามารถทำได้ด้วยการพิจารณานักท่องเที่ยวแต่ละกลุ่มเพื่อดูว่าคำสั่งการห้ามผ่านทางรถไฟครั้งใดมีผลต่อนักท่องเที่ยวกลุ่มดังกล่าวบ้าง (จำนวนนักท่องเที่ยวในกลุ่มดังกล่าวอยู่ระหว่าง $L[i]$ และ $R[i]$) และส่งผลให้เราสามารถแก้ปัญหาที่เหลือได้ในลักษณะเดียวกันกับปัญหาย่อยที่ 2

ทั้งนี้เราจะเก็บคำตอบไว้ในรูปของอาเรย์ โดยหากกลุ่มนักท่องเที่ยวบน $A[i]$ ค่าหนึ่งสามารถเดินทางบนคำสั่งรูปแบบที่ 2 ได้ เราจะบวกหนึ่งเข้าไปในช่องคำตอบของคำถามดังกล่าว แล้วจึงตอบคำถามทุกคำถามจากอาเรย์ดังกล่าวในตอนจบ

Time Complexity: $\mathcal{O}((M+Q)\times 100)$

### Official Solution

**ข้อสังเกต #2** กลุ่มนักท่องเที่ยวที่มีจำนวนคนเท่ากันจะมีคุณสมบัติในการคำนวณคำตอบเหมือนกันทุกประการ กล่าวคือสำหรับคำสั่งประเภทที่ 1 กลุ่มใด ๆ ที่มีจำนวนคนเท่ากันจะมีสถานะความสามารถการผ่าน / ไม่ผ่านทางรถไฟที่เหมือนกัน ส่งผลให้โครงสร้าง Disjoint Set มีลักษณะเหมือนกัน

**ข้อสังเกต #3** หากพิจารณาจากข้อสังเกตรูปแบบที่ 2 แล้ว จะทราบว่ามีกลุ่มนักท่องเที่ยวที่มีจำนวนสมาชิกแตกต่างกันไม่เกิน $\sqrt{2K}$ กลุ่ม โดยเราสามารถพิสูจน์เบื้องต้นจากการพิจารณา worst case scenario ซึ่งก็คือการที่เรามีสมาชิกในกลุ่มนักท่องเที่ยวของแต่ละกลุ่มแตกต่างกันทั้งหมด

หากเราทราบว่ามีนักท่องเที่ยวที่มีสมาชิกแตกต่างกันทั้งหมด $X$ กลุ่ม กลุ่มที่ 1 มีสมาชิก 1 คน, กลุ่มที่ 2 มีสมาชิก 2 คน, ... , จนถึงกลุ่มที่ $X$ มี $X$ คน จะได้ว่าจำนวนนักท่องเที่ยวทั้งหมดคือ

$$1+2+\cdots+X=\frac{X(X+1)}{2}$$

แต่เราทราบว่าจำนวนนักท่องเที่ยว $K$ ในโจทย์มีค่าไม่เกิน $100\,000$ จึงได้ว่า

$$\frac{X(X+1)}{2}\approx\frac{X^2}{2}\leq K\Longleftrightarrow X\leq\sqrt{2K}$$

ดังนั้น หากเราจำแนกกลุ่มนักท่องเที่ยวตามจำนวนสมาชิกและคำนวณในลักษณะเดียวกับปัญหาย่อยที่ 5 จะรับประกันว่าโปรแกรมของเราจะวนลูปการคำนวณคำถามทั้งหมดไม่เกิน $\sqrt{2K}$ อย่างแน่นอน

ทั้งนี้ ปัญหาย่อยที่ 6 มีไว้สำหรับโค้ดคำตอบบางกรณีที่ทำถูกวิธีแล้วแต่ใช้เวลาในการรันนานกว่าปกติ ซึ่งสามารถแก้ไขได้ด้วยการลองสลับ index $i$ และ $j$ ในการเรียกอาเรย์สองมิติ (หากมี)

Time Complexity: $\mathcal{O}((M+Q)\log N\times\sqrt{K})$

**หมายเหตุ** Solution code ของข้อนี้มีวิธีการ implement ที่แตกต่างจากคำอธิบายเล็กน้อย โดยเปลี่ยนจากการวนทุกคำสั่ง $\sqrt{K}$ รอบ เป็นการวนดูนักท่องเที่ยวทุกกลุ่มสำหรับแต่ละคำสั่งแทน ซึ่งให้ผลลัพธ์และ Time complexity เดียวกัน โดยเราจำเป็นต้องทำ **coordinate compression** บนจำนวนสมาชิกของกลุ่มนักท่องเที่ยวเพื่อให้สามารถใช้อาเรย์ขนาดไม่เกิน $N\sqrt{K}$ ได้

### Solution Code

```cpp
#include <bits/stdc++.h>
#define MAXN 50000
using namespace std;
int tour_group[100100], szidx[100100], gcnt[100100];
vector<pair<int, int>> mark[100100];
vector<pair<int, int>> edge;
int pa[100100][320], blocked[100100][320];
vector<int> allsz;
bitset<100100> szchk;

int findpa(int i, int a){
  return pa[a][i] == a ? a : pa[a][i] = findpa(i, pa[a][i]);
}

void union_range(int p, int l, int r){
  // Union edge #p for all group from index l to r
  int i = lower_bound(allsz.begin(), allsz.end(), l) - allsz.begin();
  for(; i < allsz.size() && allsz[i] <= r; i++){
    blocked[p][i]--;
    if(blocked[p][i]) continue;
    int gsz = allsz[i];

    auto [u, v] = edge[p];
    int pa_u = findpa(i, u), pa_v = findpa(i, v);
    if(pa_u != pa_v) pa[pa_u][i] = pa_v;
  }
}

int main(int argc, char* argv[]){
  int N, M, K, Q;
  scanf("%d %d %d %d", &N, &M, &K, &Q);

  // Collect how many tour groups and how many people,
  // Guaranteed to be lesser than sqrt(N)
  for(int i = 0, A; i < K; i++){
    scanf("%d", &A);
    tour_group[A]++;
  }
  for(int i = 1; i <= 100000; i++){
    if(!szchk[tour_group[i]]){
      szchk[tour_group[i]] = 1;
      allsz.push_back(tour_group[i]);
    }
    if(tour_group[i]) gcnt[tour_group[i]]++;
  }
  sort(allsz.begin(), allsz.end());

  // Construct initial graph
  edge.push_back({0, 0});
  for(int i = 1, u, v; i <= M; i++){
    scanf("%d %d", &u, &v);
    edge.push_back({u, v});
  }

  // Collect query
  vector<array<int, 4>> query;
  for(int i = 1; i <= Q; i++){
    int q; scanf("%d", &q);
    if(q == 1){
      int p, l, r; scanf("%d %d %d", &p, &l, &r);
      query.push_back({q, p, l, r});
      mark[p].push_back({l, r});
    }
    else{
      int s, e; scanf("%d %d", &s, &e);
      query.push_back({q, s, e, 0});
    }
  }

  // Add blocked layers for blocked edges
  for(int i = 1; i <= M; i++){
    for(auto [l, r] : mark[i]){
      int lidx = lower_bound(allsz.begin(), allsz.end(), l) - allsz.begin();
      int ridx = upper_bound(allsz.begin(), allsz.end(), r) - allsz.begin();
      blocked[i][lidx] += 1;
      blocked[i][ridx] -= 1;
    }
  }

  // Init DSU
  for(int i = 1; i <= N; i++)
    for(int j = 1; j < allsz.size(); j++) pa[i][j] = i;

  for(int i = 1; i <= M; i++){
    for(int j = 1; j < allsz.size(); j++){
      blocked[i][j] += blocked[i][j - 1];

      // Union all edges that can travel by default
      if(!blocked[i][j]){
        auto [u, v] = edge[i];
        pa[findpa(j, u)][j] = findpa(j, v);
      }
    }
  }

  // Start answering query from back to front
  stack<int> ans;
  for(int i = query.size() - 1; i >= 0; i--){
    int q = query[i][0];
    if(q == 1){
      int p = query[i][1], l = query[i][2], r = query[i][3];
      union_range(p, l, r);
    }
    else{
      int s = query[i][1], e = query[i][2], total_cnt = 0;
      for(int j = 1; j < allsz.size(); j++){
        if(findpa(j, s) == findpa(j, e))
          total_cnt += gcnt[allsz[j]];
      }
      ans.push(total_cnt);
    }
  }

  // Print all answers
  while(!ans.empty()){
    printf("%d\n", ans.top());
    ans.pop();
  }
  return 0;
}
```