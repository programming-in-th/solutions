### Abridged Problem Statement

โจทย์กำหนดตาราง 2 มิติที่มีขนาด $R$ แถว $C$ คอลัมน์ โดยต้องการหาจำนวนวิธีในการเคลื่อนที่ 2 เส้นทางจากช่อง $(1,1)$ ไปยังช่อง $(R,C)$ โดยที่สามารถเคลื่อนที่ได้เฉพาะทิศลงหรือขวาเท่านั้น และทั้ง 2 เส้นทางนี้ไม่สามารถใช้เส้นทางการเคลื่อนที่ร่วมกันได้ แต่สามารถใช้ช่องร่วมกันได้ เนื่องจากคำตอบอาจมีค่ามาก จึงให้ตอบเศษจากการหารด้วย $1\,000\,000\,007$

### Subtask 1-4

ในปัญหาย่อยนี้ $R,C\leq 200$ ทำให้สามารถใช้ Dynamic Programming ในการแก้ปัญหานี้ได้ดังนี้

กำหนดนิยามให้ $dp(i,j,n)$ คือการเดินที่สองเส้นทางพร้อมกันจากช่อง $(1,1)$ (บนซ้าย) ไปแล้ว $n$ ก้าวและเส้นทางแรกจบที่แถว $i$, เส้นทางที่สองจบที่แถว $j$

กำหนดให้ $dp(1,1,0)=1$ เป็นค่าเริ่มต้นและ $dp(i,j,n)=0$ ถ้า $i<1$ หรือ $j<1$ หรือ $n<\max(i,j)-1$

ในกรณี $i\neq j$ และ $n\geq \max(i,j)-1$ สังเกตว่าก้าวสุดท้ายของสองเส้นทางนี้จะไม่มีผลต่อกัน กล่าวคือเรามีวิธีทั้งหมด $4$ แบบ (เส้นทางแรกเดินลงหรือเดินขวา และ เส้นทางที่สองเดินลงหรือเดินขวา) จากข้างต้นเราจะได้ว่า

$$dp(i,j,n)=dp(i,j,n-1)+dp(i-1,j,n-1)+dp(i,j-1,n-1)+dp(i-1,j-1,n-1)$$

ในกรณี $i=j$ และ $n\geq\max(i,j)-1$ สังเกตว่าทั้งสองเส้นทางมาจบที่จุดเดียวกัน ทำให้ก้าวสุดท้ายของแต่ละเส้นทางไม่เหมือนกัน ซึ่งมีทั้งหมด $2$ รูปแบบ (เส้นแรกเดินลงเส้นสองเดินขวา และ เส้นแรกเดินขวาเส้นสองเดินลง) จากข้างต้น เราจะได้ว่า

$$dp(i,j,n)=dp(i-1,j,n-1)+dp(i,j-1,n-1)$$

เมื่อคำนวณตามวิธีนี้จะได้ว่าคำตอบคือ $dp(R,R,R+C-2)$

Time Complexity: $\mathcal{O}((R+C)R^2)$

### Subtask 5

ในปัญหาย่อยนี้ เราต้องใช้ทฤษฎีบทย่อยที่สำคัญข้อหนึ่ง นั่นคือ

**ทฤษฎีบท.** *ให้ $dp(i,j,n)$ แทนจำนวนวิธีการเดินซึ่งทำให้เส้นทางแรกจบทีแถวที่ $i$ และเส้นทางที่สองจบที่แถวที่ $j$ จะได้ว่า สำหรับค่า $i,j$ ที่ fix ไว้ใด ๆ ซึ่งไม่ใช่ $(1,1)$ จะมีพหุนาม $P(x)$ ดีกรีไม่เกิน $i+j-3$ ซึ่ง $dp(i,j,n)=P(n)$ สำหรับทุก $i\neq j$ และ $n\geq\max(i,j)-1$ และจะมีพหุนาม $P(x)$ ดีกรีไม่เกิน $i+j-4$ ซึ่ง $dp(i,j,n)=P(n)$ สำหรับทุก $i=j$ และ $n\geq\max(i,j)$*

*พิสูจน์.* สังเกตว่าถ้า $i=1$ และ $j>1$ และ $n\geq j-1$ จะได้ว่าคำตอบคือ $\binom{n-1}{j-2}$ ซึ่งเป็นพหุนามดีกรี $j-2\leq i+j-3$ จึงได้ว่าสิ่งที่เราต้องการเป็นจริงในกรณีนี้

ต่อจากนี้เราจะทำการ induction (อุปนัยเชิงคณิตศาสตร์) บนค่าของ $i+j$ โดยเริ่มจาก $i+j=3$

Base Case: $i+j=3$ จะได้ว่า $i=1$ หรือ $j=1$ จึงได้รับการพิสูจน์แล้ว

Inductive Step: ถ้า $i=1$ หรือ $j=1$ ได้รับการพิสูจน์ไปแล้ว

ถ้า $i\neq j$ และ $n\geq\max(i,j)$ และ $i,j>1$ จะได้ว่า

$$dp(i,j,n)=dp(i,j,n-1)+dp(i-1,j,n-1)+dp(i,j-1,n-1)+dp(i-1,j-1,n-1)$$

ซึ่งเราสามารถจัดรูปใหม่ได้เป็น

$$Q(n)=dp(i,j,n)-dp(i,j,n-1)=dp(i-1,j,n-1)+dp(i,j-1,n-1)+dp(i-1,j-1,n-1)$$

จากสมมติฐานการอุปนัยจะได้ว่า $Q(n)$ (ซึ่งมีค่าเท่ากับ $P(n)-P(n-1))$ เป็นพหุนามที่ดีกรีไม่เกิน $i+j-4$ สำหรับทุก $n\geq\max(i,j)$ จะได้ว่า $P(n)$ เป็นพหุนามที่ดีกรีมากกว่า $Q(n)$ อยู่ $1$ พอดี (ยกเว้นกรณีที่ $Q(n)=0$ ซึ่งทำให้ดีกรีของ $P(n)$ ยังคงเป็น $0$) เราจึงสรุปได้ว่า $dp(i,j,n)=P(n)$ เป็นพหุนามที่ดีกรีไม่เกิน $(i+j-4)+1=i+j-3$

ส่วนถ้า $i=j>1$ และ $n\geq\max(i,j)$ จะได้ว่า $dp(i,j,n)=dp(i-1,j,n-1)+dp(i,j-1,n-1)$ จากสมมติฐานการอุปนัย เราจะได้ว่า $P(n)=dp(i,j,n)$ เป็นพหุนามที่ดีกรีไม่เกิน $i+j-4$

ดังนั้นจึงสรุป Theorem ได้ตามต้องการ

จาก Lemma จะได้ว่าค่าของ $dp(R,R,n)$ เป็นพหุนามดีกรีไม่เกิน $2R-4$ บน $n$ ดังนั้นการทำ Finite Difference (แปลง
$P(x)$ เป็น $P(x+1)-P(x)$ ซึ่งทำให้ดีกรีลดลงทีละ $1$) ซ้ำ $2R-4$ ครั้งจะทำให้เหลือผลลัพธ์เป็นค่าคงที่ ถ้าทำซ้ำ $2R-3$ ครั้งก็จะเหลือ $0$ พอดี นั่นคือ เราสามารถหาค่าของ $dp(R,R,n)$ ได้จาก $dp(R,R,n-1),dp(R,R,n-2),\dots,dp(R,R,n-2R+3)$ โดยใช้สมการที่ได้จากการทำ Finite Difference ซึ่งก็คือ

$$\sum\limits_{i=0}^{2R-3}(-1)^i\binom{2R-3}{i}dp(R,R,n-i)=0$$

ดังนั้น สิ่งที่ต้องทำในข้อนี้คือ การหา $dp(R,R,R),\dots,dp(R,R,3R−5),dp(R,R,3R−4)$ ซึ่งเช่นกันกับใน Subtask 1-4 จะใช้เวลา $\mathcal{O}(R^3)$ และจากนั้นจึงคำนวน $dp(R,R,3R-3),dp(R,R,3R-2),\dots,dp(R,R,R+C-2)$ ด้วยการใช้ finite difference ซึ่งใช้เวลา $\mathcal{O}(RC)$
สำหรับการหา $\binom{2R-3}{i}$ สามารถใช้ Dynamic Programming ในการ preprocess ไว้ล่วงหน้าก่อนภายในเวลา $\mathcal{O}(R^2)$

Time Complexity: $\mathcal{O}(R^3+RC)$