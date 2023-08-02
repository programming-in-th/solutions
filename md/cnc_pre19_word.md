### Subtask 1 (12 คะแนน) $K = 1$

สังเกตว่า มีตัวอักษรที่เราต้องเก็บเพียงแค่ตัวเดียว ซึ่งสามารถใช้วิธีการ BFS ธรรมดา เริ่มจากเมืองที่ $1$ ไปหาทุกเมืองที่มีตัวอักษรตัวนั้น แล้วส่งออกระยะห่างที่น้อยที่สุด (อย่าลืมคูณ $2$ เนื่องจากต้องคิดระยะทางทั้งขาไปและขากลับ)

Time Complexity: $\mathcal{O}(M)$

### Subtask 2 (14 คะแนน) เมืองทุกเมืองมีตัวอักษรเดียวกัน

เราสามารถทำการ BFS จากเมืองที่ $1$ ไปยังทุก ๆ เมือง หากว่าสายอักขระความยาว $K$ นั้น มีตัวอักษรอื่นนอกจากตัวอักษรที่แต่ละเมืองมีอยู่ หรือ $K>N$ ก็จะไม่สามารถทำได้ จึงตอบ $−1$ ไม่เช่นนั้นคำตอบจะเป็น ผลรวมของระยะห่างของแต่ละเมืองที่มีค่าน้อยที่สุด $K$ ตัวแรก ซึ่งสามารถทำได้โดยการ sort ระยะห่างของแต่ละเมืองจากน้อยไปมาก แล้วนำระยะห่างของ $K$ เมืองแรกมาบวกกัน

Time Complexity: $\mathcal{O}(M+N\log N)$

### Subtask 3 (19 คะแนน) $N,K\leq500$

ในแต่ละตัวอักษรในสายอักขระ เราสามารถทำการ BFS แล้วหาระยะทางที่น้อยที่สุดได้เหมือน subtask $1$ พร้อมกับทำสัญลักษณ์ว่าได้ทำการเก็บตัวอักษรจากเมืองที่มีระยะทางน้อยที่สุดนั้นไปแล้ว เพื่อที่จะไม่นำไปคำนวณซ้ำในตัวอักษรถัด ๆ ไป

Time Complexity: $\mathcal{O}(KM)$

### Official Solution

เราจะทำการ BFS เพื่อหาระยะห่างจากเมืองที่ $1$ ไปยังทุก ๆ เมือง และเก็บเข้า queue ของตัวอักษรทั้ง $26$ (สามารถดำเนินการได้ระหว่างทำ BFS) จากนั้นไล่ดูตัวอักษรในสายอักขระความยาว $K$ ทีละตัว หาก queue ของตัวอักษรที่ต้องการนั้นว่าง แสดงว่าไม่สามารถสร้างสายอักขระได้ ให้ตอบ $−1$ มิเช่นนั้นให้บวกค่าแรกของ queue เข้าไปในคำตอบ แล้ว pop ค่านั้นทิ้ง **โปรดระวังเรื่องการใช้ long long ซึ่งจะทำให้ได้คะแนนเพียงแค่ subtask $4$**

Time Complexity: $\mathcal{O}(M+K+N)$

### Solution Code

```cpp
#include <bits/stdc++.h>
using namespace std;
string letter, S;
vector<int> adj[500005];
priority_queue<int, vector<int>, greater<int>> best_pos[26];
int dist[500005];

int main(int argc, char* argv[]){
  ios_base::sync_with_stdio(0); cin.tie(0);

  int N, M, K; cin >> N >> M >> K;
  cin >> letter;

  for(int i = 1, u, v; i <= M; i++){
    cin >> u >> v;
    adj[u].push_back(v);
    adj[v].push_back(u);
  }

  queue<int> q; dist[1] = 1;
  q.push(1);
  while(!q.empty()){
    int n = q.front(); q.pop();
    for(auto x : adj[n]){
      if(dist[x]) continue;
      dist[x] = dist[n] + 1;
      q.push(x);
    }
  }
  for(int i = 1; i <= N; i++) best_pos[letter[i - 1] - 'A'].push(dist[i] - 1);

  long long ans = 0;
  cin >> S;
  for(auto x : S){
    if(best_pos[x - 'A'].empty()){
      printf("-1\n"); return 0;
    }
    ans += (best_pos[x - 'A'].top() * 1ll);
    best_pos[x - 'A'].pop();
  }
  printf("%lld\n", ans << 1);
  return 0;
}
```