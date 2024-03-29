### Abridged Problem Statement

กำหนดตารางสองมิติขนาด $N$ แถว $M$ คอลัมน์ แต่ละช่องประกอบด้วยลูกแก้วสีแดงหรือเขียวอย่างใดอย่างหนึ่ง ต้องการเดินจากช่องบนซ้ายไปยังล่างขวาโดยเดินได้เพียงทิศลงหรือขวาเท่านั้น โดยเมื่อเดินผ่านแต่ละช่องจะเก็บลูกแก้วหรือไม่ก็ได้ ถามว่าจะเก็บได้อย่างมากที่สุดกี่ลูกโดยที่ทั้งสองสีนั้นจะต้องเท่ากัน และจะต้องเดินอย่างไร เก็บอย่างไร

### Subtask 1

ทดลองเดินทุกแบบที่เป็นไปได้ ทั้งหมด $\frac{(N−1+M−1)!}{(N−1)!(M−1)!}$ วิธี ซึ่งจะได้ไม่เกิน $20$ วิธีหากยึดตามข้อจำกัดของปัญหาย่อยนี้แต่ละวิธี ไล่หาทุกวิธีการเลือกที่เป็นไปได้ มีทั้งหมด $2^{(N+M−1)}$ วิธี ซึ่งจะได้ไม่เกิน $128$ วิธีตามข้อจำกัดของปัญหาย่อย
นี้

Time Complexity: $\mathcal{O}(\frac{(N+M−2)!}{(N−1)!(M−1)!}2^{(N+M−1)})$

### Subtask 2-3

เราจะใช้วิธีการ Dynamic Programming โดยพิจารณาว่า สำหรับการเดินจากช่องบนซ้าย $(1,1)$ มาจนถึงช่องที่ $(i,j)$ ใด ๆ โดยที่หยิบลูกแก้วสีแดงไปแล้ว $R$ ลูก และหยิบลูกแก้วสีเขียวไปแล้ว $G$ ลูก ทำได้หรือไม่ และหากทำได้ ถือว่าคำนวณจากการเดิน state ก่อนหน้าในช่องใด เราจะแทนค่านี้ด้วยตัวแปร $dp(i,j,R,G)$ (มีวิธีการเขียนหลายแบบ วิธีหนึ่งคือมองว่า $dp(i,j,R,G)$ จะเป็น
* $0$ เมื่อไม่สามารถทำให้เกิด state ดังกล่าวได้
* $1$ หากสามารถเกิดได้จาก $dp(i−1,j,R',G')$ หรือ $dp(i−1,j,R,G)$
* $2$ หากสามารถเกิดได้จาก $dp(i,j−1,R',G')$ หรือ $dp(i,j−1,R,G)$

เมื่อ $R',G'$ แทนจำนวนลูกแก้วไม่รวมช่อง $(i,j)$

หากนิยาม state ของ Dynamic Programming ว่าเป็นจริงก็ต่อเมื่อช่องปัจจุบันเป็นช่องที่ถูกเลือก การย้าย state จะใช้เวลา $\mathcal{O}(NM)$ ทำให้ได้เวลารวม $\mathcal{O}(N^3M^3)$

หากพัฒนาขึ้นมาเรื่อย ๆ จนได้นิยามดังที่กล่าวข้างต้น จะใช้เวลา $\mathcal{O}(N^2M^2)$

Time Complexity: $\mathcal{O}(N^2M^2)$

### Subtask 4

ต่อจากปัญหาย่อยที่ผ่านมา แทนที่จะเก็บค่าทั้ง $R$ และ $G$ เราจะเก็บค่าผลต่างแทน กล่าวคือเก็บค่า $R−G$ พร้อมทั้งข้อมูล $R+G$ ภายใน state จะได้เป็น $dp(i,j,R−G):=(R+G,code)$ เมื่อ $code$ เป็น $0,1,2$ ตามที่นิยามในปัญหาย่อยก่อนหน้า หากมี $R+G$ ที่เป็นไปได้หลายค่าสำหรับ state เดียวกัน จะพิจารณาตามค่าที่มากที่สุดที่เป็นไปได้ และการหาคำตอบก็ใช้วิธี backtrack แบบเดียวกับปัญหาย่อยก่อนหน้า

Time Complexity: $\mathcal{O}(NM(N+M))$

### Subtask 5

เนื่องจากวิธีการเดินมีได้เพียงแบบเดียว เราสามารถพิจารณาเพียงแค่ว่าจะหยิบลูกแก้วตำแหน่งใดบ้าง ซึ่งจะได้ว่าคำตอบเป็น $\min(R,G)$ เมื่อ $R,G$ แทนจำนวนลูกแก้วสีแดงและสีเขียวทั้งหมด ตามลำดับ สังเกตว่าการหยิบลูกแก้วสีเดียวกัน ไม่ว่าจะหยิบตำแหน่งใดก็ไม่สำคัญ เราจึงสามารถหยิบไปเรื่อย ๆ จนได้ค่าดังกล่าว

Time Complexity: $\mathcal{O}(M)$

### Subtask 6

แยกเป็นสองแถว สิ่งที่จำเป็นต้องเก็บสำหรับแถวแรก คือข้อมูลว่าหากเดินไปทางขวาเรื่อย ๆ ถึงคอลัมน์ที่ $i$ แล้วจะเก็บลูกแก้วสีแดงและเขียวได้มากที่สุดกี่ลูก นิยามเป็น $R1_i,G1_i$ ตามลำดับ สำหรับแถวที่สอง เก็บจากคอลัมน์ที่ $M$ มาถึงคอลัมน์ที่ $i$ นิยามเป็น $R2_i,G2_i$ คำตอบทุกคำตอบที่เป็นไปได้ จะต้องมีการเปลี่ยนจากแถวแรกไปยังแถวที่สอง ในบางตำแหน่ง (เรียกตำแหน่งนั้นว่า $i'$) คำตอบที่ดีที่สุดเกิดขึ้นเมื่อ $\min(R1_{i'}+R2_{i'},G1_{i'}+G2_{i'})$ มีค่ามากที่สุด เราทำการไล่ค่าทุก $i'$ ตั้งแต่ $1$ ถึง $M$ แล้วหาค่าที่มากที่สุดที่เป็นไปได้ หลังจากนั้นจะสามารถแก้ได้หาวิธีการเดินได้แบบเดียวกับปัญหาย่อย 5

Time Complexity: $\mathcal{O}(M)$

### Subtask 7

เราจะพิจารณาข้อสังเกตบางข้อเพิ่มเติมดังนี้

**ข้อสังเกต.** *การเดินทางจากช่องบนซ้ายไปยังขวาล่าง โดยเดินได้เพียงแค่ไปทางขวากับลงล่างนั้น จะเดินผ่านทั้งหมด $N+M-1$ ช่องพอดี และเนื่องจากผ่านทั้งหมด $N+M-1$ ช่อง หากบังคับว่าเมื่อเดินผ่านทุกช่องแล้วจะต้องเก็บทุกช่อง และเก็บลูกแก้วสีแดงได้ทั้งหมด $R$ ลูก แล้วจะได้ว่าเก็บลูกแก้วสีเขียวได้ $N+M-1-R$ ลูก*

ต่อมาเราจึงพิจารณาเส้นทางเส้นหนึ่งที่เป็นไปได้ สมมติว่ามีลูกแก้วสีแดง $R$ ลูก และมีลูกแก้วสีเขียว $N+M-1-R$ ลูก คำตอบที่ดีที่สุดที่จะได้จากการเดินบนเส้นทางนี้คือ $2\min(R,N+M-1-R)$ พอดี จากจุดนี้เราสามารถดัดแปลงโจทย์ให้ง่ายลง แต่มีคำตอบเหมือนโจทย์เดิม ได้ว่า: สำหรับทุกเส้นทางเราจะหยิบลูกแก้วทุกลูกที่เดินผ่าน แล้วพิจารณาค่าของเส้นทางเป็น $2\min(R,N+M+1-R)$ เมื่อ $R$ แทนจำนวนลูกแก้วสีแดงบนเส้นทางนั้น ต่อมา สังเกตข้อสังเกตเพิ่มเติมดังนี้

**ข้อสังเกต** (Intermediate Value Theorem). *หากมีเส้นทางอย่างน้อยเส้นหนึ่งที่เมื่อเก็บลูกแก้วสีแดงทุกลูกที่อยู่บนเส้นทางนั้นแล้วได้เป็นค่า $R_1$ และมีอีกเส้นทางหนึ่งที่ เมื่อเก็บลูกแก้วสีแดงทุกลูกบนเส้นนั้นเช่นกันแล้วได้เป็นค่า $R_2$ โดยที่ $R_1\leq R_2$ จะได้ว่า สำหรับทุก ๆ $R'$ ที่ $R_1\leq R'\leq R_2$ จะมีเส้นทางอย่างน้อยหนึ่งเส้นทางที่ทำให้ได้ลูกแก้วสีแดง จำนวน R' ลูกพอดีเมื่อหยิบทุกลูกบนเส้นนี้*

ทฤษฎีบทข้างต้นสามารถพิสูจน์ได้ด้วยการอุปนัยเชิงคณิตศาสตร์ การพิสูจน์นี้ถือเป็นแบบฝึกหัดสำหรับผู้อ่าน

เมื่อข้อสังเกตข้างต้นเป็นทฤษฎีบทที่เป็นจริง เราจึงปรับนิยามของ Dynamic Programming จากปัญหาย่อย 4 จากที่
เป็น $dp(i,j,R-G)$ กลายเป็น $dp(i,j)$ เก็บว่าสามารถหยิบลูกแก้วสีแดงได้ค่าเท่าใดบ้างหากบังคับว่าหยิบทุกลูกจากจุดเริ่มต้นจนถึงช่องปัจจุบัน เนื่องจากมีได้หลายค่าแต่ทฤษฎีบทข้างต้นเป็นจริง จึงได้ว่าเราจำเป็นต้องเก็บเพียงค่าที่น้อยที่สุดและมากที่สุดที่เป็นไปได้ ตามลำดับ (ค่าระหว่างนั้นทุกค่าจะเป็นไปได้เสมอ)

เมื่อถึงช่องในแถวที่ $N$ คอลัมน์ที่ $M$ แล้ว จะได้ค่า $dp(N,M)$ เป็นช่วง ๆ หนึ่ง เราต้องการหาค่าสูงสุดของฟังก์ชัน $f(x):=2\min(x,N+M-1-x)$ สำหรับ $x$ ที่อยู่ภายในช่วงนี้ (ฟังก์ชันนี้แทนการคำนวณคะแนนจากจำนวนลูกแก้วสีแดงที่มีอยู่)

**ข้อสังเกต.** *ฟังก์ชัน $f:\mathbb{R}\to\mathbb{R}$ นิยามโดย $f(x):=2\min(x,N+M+1-x)$ มีจุดสูงสุดอยู่ที่ $x_{crit}=\frac{N+M-1}{2}$ จึงได้ว่าฟังก์ชันที่คล้ายกัน (เรียกเป็น $g:\mathbb{Z}\to\mathbb{Z}$ นิยามโดย $g(x):=2\min(x,N+M+1-x))$ ก็มีจุดสูงสุดอยู่ที่ $x_{crit}=\frac{N+M-1}{2}$ หาก $N+M−1$ เป็นจำนวนเต็มคู่ แต่จะมีจุดสูงสุดอยู่สองจุดที่ $x_{crit}=\frac{N+M}{2}−1$ และ $\frac{N+M}{2}$ หาก $N+M-1$ เป็นจำนวนเต็มคี่*

หลังจากนี้เราจึงสามารถหาคำตอบโดยการแยกกรณี $N+M-1$ เป็นจำนวนเต็มคู่/คี่ แล้วแยกกรณีต่อว่าช่วงของ $R$
ที่เป็นไปได้นั้นครอบคลุมจุดสูงสุดหรือไม่

**ข้อสังเกต.** *$g(x_{crit}-k)=g(x_{crit})-k$ เมื่อ $x_crit$ แทนค่า $x$ ที่ทำให้ $g(x)$ มีค่าสูงสุด หากมีหลายค่าจะแทนด้วยค่าต่ำสุดที่เป็นไปได้*

*ในทำนองเดียวกันสำหรับ $g(x_{crit}+k)=g(x_{crit})-k$ เมื่อ $x_{crit}$ แทนค่า $x$ ที่ทำให้ $g(x)$ มีค่าสูงสุด หากมีหลายค่าจะแทนด้วยค่าสูงสุดที่เป็นไปได้*

ข้อสังเกตข้างต้นสามารถได้จากการสังเกตลักษณะของฟังก์ชัน $g$ ที่นิยามไว้ข้างต้น และสามารถพิสูจน์ทางคณิตศาสตร์
ได้โดยตรง

สำหรับกรณีที่ช่วงคำตอบของ R ครอบคลุม $x_{crit}$ เราสามารถตอบได้ทันที แต่ถ้าหากไม่คลุมจุดดังกล่าว จะแยกได้ $2$
กรณีคือกรณีที่ทั้งช่วงน้อยกว่า $x_{crit}$ และกรณีที่ทั้งช่วงมากกว่า $x_{crit}$ แล้วคำนวณค่าคำตอบที่ดีที่สุดจากข้อสังเกตข้างต้น

สำหรับการย้อนรอยหาคำตอบนั้น สามารถทำได้โดยพิจารณาว่าค่า $R$ ที่ใช้นั้น มาจากส่วนใดของช่วง $dp(N,M)$ แล้วพิจารณาว่าควรย้อนไปในทิศทางใด ทำคล้ายปัญหาย่อยที่ผ่านมา
