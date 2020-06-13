เราสามารถมองสิ่งที่โจทย์กำหนดมาให้เป็น [**flow network**](https://en.wikipedia.org/wiki/Flow_network) ได้ดังนี้
* ปลายแต่ละด้านของแท่งไม้แต่ละแท่ง เป็น **node** 
* แท่งไม้ที่รับน้ำหนักมดได้ $w$ ขบวน จะเป็น **edge** ที่เชื่อม *node* ที่อยู่ปลายแท่งไม้ทั้งสองข้าง และมี *capacity* เป็น $w$
* พิกัด $(0,0)$ เป็น **source** (หรือ $s$) และมี *edge* ที่มี *capacity* เป็น $\infty$ เชื่อมไปยังทุกโหนดที่มีพิกัดแกน $x$ เป็น $0$
* ในทำนองเดียวกัน พิกัด $(L,0)$ เป็น **sink** (หรือ $t$) และมี *edge* ที่มี *capacity* เป็น $\infty$ เชื่อมไปยังทุกโหนดที่มีพิกัดแกน $x$ เป็น $L$

<<<<<<< HEAD
ชุดทดสอบตัวอย่างที่ 1 สามารถมองเป็น *flow network* ได้เช่นดังรูป  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph1.png)  
=======
ชุดทดสอบตัวอย่างที่ 1 สามารถมองเป็น *flow network* ได้เช่นดังรูป
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph1.png)
>>>>>>> 20f14dc12d569f4dc64b585deba0db64a72b89ab
(อนึ่ง *เส้นตรงสีน้ำตาล* คือเส้นริมฝั่งของแม่น้ำ ซึ่งจะถูกกล่าวถึงช่วงต่อไป)

จะได้ว่า แท้จริงแล้ว โจทย์ข้อนี้คือ [**Maximum flow problem**](https://en.wikipedia.org/wiki/Maximum_flow_problem) นั่นเอง

สำหรับข้อนี้ หากใช้ Algorithm สำหรับคำนวณ *Maximum flow* โดยตรงจะใช้เวลามากเกินไป

### Observation
สังเกตได้ว่าตัวกราฟของ flow network นั้นเป็น [**planar graph**](https://en.wikipedia.org/wiki/Planar_graph#:~:text=In%20graph%20theory%2C%20a%20planar,no%20edges%20cross%20each%20other.) และนอกจากนี้ ยังเป็นกราฟบนระนาบสองมิติ เราสามารถ model กราฟขึ้นมาใหม่จากกราฟเดิม ให้สามารถคำนวณ *Minimum cut* อย่างมีประสิทธิภาพได้ โดยการแปลงปัญหานี้ให้เป็น [**Shortest Path problem**](https://en.wikipedia.org/wiki/Shortest_path_problem) แทน

(เนื่องจาก *Maximum flow = Minimum cut* เสมอโดย [_***Max-flow Min-cut theorem***_](https://en.wikipedia.org/wiki/Max-flow_min-cut_theorem) เราจึงสามารถคำนวณ *Minimum cut* แทนได้)

### Graph Modeling
ต่อจากนี้ เราจะไม่สนใจ *edge* ระหว่าง
* โหนด $(0,0)$ กับโหนดที่มีพิกัดแกน $x$ เป็น $0$ และ
* โหนด $(L,0)$ กับโหนดที่มีพิกัดแกน $x$ เป็น $L$ 

เพราะการลากเส้น *Cut* ผ่าน *edge* เหล่านี้จะเป็นการเพิ่มค่าของ *Cut* อย่างใช่เหตุ เนื่องจากไม่จำเป็นต้องลากเส้นผ่าน *edge* เหล่านี้เลย 
ภาพต่อไปนี้คือชุดทดสอบตัวอย่างที่ $1$ เมื่อทำการนำ *edge* ดังกล่าวออกแล้ว  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph2-1.png)  
**นิยาม** เราจะนิยามให้ "**Region**" หมายถึง พื้นที่ในกราฟที่ถูกล้อมด้วย *edge* หรือ*เส้นริมฝั่งแม่น้ำ*

**นิยาม** เราจะนิยามให้ "**Start Region**" หมายถึง *Region* ที่อยู่*ข้างบนสุด*

**นิยาม** เราจะนิยามให้ "**End Region**" หมายถึง *Region* ที่อยู่*ข้างล่างสุด*

<<<<<<< HEAD
ดังในรูปของชุดทดสอบตัวอย่างที่ 1 กราฟจะมีทั้งหมด $5$ *Region* (แต่ละ *Region* จะมีหมายเลขกำกับไว้)   
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph2-2.png)  
จากรูป, 
=======
ดังในรูปของชุดทดสอบตัวอย่างที่ 1 กราฟจะมีทั้งหมด $5$ *Region* (แต่ละ *Region* จะมีหมายเลขกำกับไว้) 

![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph2-4.png)จากรูป, 
>>>>>>> 20f14dc12d569f4dc64b585deba0db64a72b89ab
* *Start Region* คือ $[1]$ และ *End Region* คือ $[5]$
* สังเกตุได้ว่า *Cut* คือ การลากเส้นจาก *Start Region* ไปยัง *End Region* โดยค่าของ *Cut* คือผลรวมน้ำหนักของ *edge* ที่ลากผ่าน
* $(a)$, $(b)$, $(c)$ คือตัวอย่างของ *Cut* ที่เป็นไปได้ แต่ $(a)$ เป็น *Cut* ที่มีผลรวมน้ำหนักของ *edge* เป็น $2$ ซึ่งน้อยที่สุดเท่าที่เป็นไปได้แล้ว ในขณะที่ $(b)$ และ $(c)$ มีผลรวมน้ำหนักเป็น $3$
  
เราสามารถแปลงเป็นปัญหา *Shortest Path* โดยการมองว่า
* แต่ละ *Region* เป็นเสมือน *node* ๆ หนึ่ง
* เส้นที่คั่นระหว่างสอง *Region* จะเป็น *edge* เชื่อมคู่ *node* ของทั้งสอง *Region* นั้น
* เราต้องการเดินจาก *node* ของ *Start Region* ไปยังโหนดของ *End Region* โดยให้ผลรวมของน้ำหนักเส้นน้อยที่สุด

กราฟของชุดทดสอบตัวอย่างที่ $1$ จึงสามารถ model เป็นกราฟของปัญหา **Single-Source Shortest Path** ได้ดังรูป   
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph3.png)  
สามารถเห็นได้ว่า *Shortest Path* จาก $[1]$ ไปยัง $[5]$ ก็คือ *Minimum Cut* นั่นเอง

### Algorithm
<<<<<<< HEAD
ความท้าทายของโจทย์ข้อนี้คือการคิดและ implement วิธีสร้างกราฟขึ้นมาเพื่อนำมาใช้แก้ปัญหา Shortest Path  

ก่อนอื่น จากเงื่อนไขของโจทย์ ชุดทดสอบสามารถมี"component ลอย"ได้ กล่าวคือ เป็นกลุ่มของกิ่งไม้ที่ลอยอยู่บนแม่น้ำเฉยๆ ไม่มีส่วนใดเชื่อมต่อกับริมฝั่งแม่น้ำเลย ดังเช่นกิ่งไม้ที่เชื่อมต่อ $(1,3)$ กับ $(1,4)$ ในกรณีของรูปดังต่อไปนี้  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph6-0.png)   
เราจะ**ไม่สนใจ** "component ลอย" ภายในกราฟ เพราะอย่างไร เราก็สามารถลากเส้นอ้อมหลบ component เหล่านี้ได้เสมอ การลากเส้นผ่านกิ่งไม้เหล่านี้จะเป็นการเพิ่มน้ำหนักรวมของ *cut* โดยใช่เหตุ 

ย้อนกลับมายังกราฟเริ่มต้นของชุดทดสอบตัวอย่างที่ $1$ 
* สร้าง "**โหนดสมมุติ**" ขึ้นมา *4* โหนด รอบสี่ทิศแนวทะแยงของแต่ละ *node* ดังรูป   
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph4.png)  
* สร้างเส้นเชื่อมระหว่างโหนดสมมุติภายใน *node* ที่อยู่ในแนวตั้งหรือแนวนอนเดียวกัน  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5.png)  
* หากเป็น *edge* ที่มีปลายหนึ่ง(หรือทั้งสอง)เป็นโหนดสมมุติที่อยู่บนฝั่ง(กล่าวคือไม่ได้อยู่ในแม่น้ำ) ให้มีน้ำหนักเป็น $\infty$
* หากเป็น *edge* ที่ตัดกับเส้นเชื่อม*ในกราฟตั้งต้น* ให้มีน้ำหนักเท่ากับเส้นเชื่อมนั้น
* หากเป็น *edge* ที่ไม่ได้ตัดกับเส้นเชื่อมในกราฟตั้งต้น ให้มีน้ำหนักเป็น $0$  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-1.png)  
* สำหรับคู่ *node* ที่มี *edge* เชื่อมในกราฟตั้งต้น, ให้สร้างเส้นเชื่อมน้ำหนัก $0$ เชื่อมสองคู่โหนดสมมุติที่อยู่ใกล้กันในแนว *edge* ดังกล่าว  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-2.png)  
* ท้ายที่สุด สร้าง *edge* น้ำหนัก $0$ เชื่อมระหว่างโหนดสมมุติของ *node* ที่มีพิกัดในแนวแกน $x$ เป็น $0$ ที่อยู่ติดกันในแนวตั้ง (ดังรูปด้านล่าง)
* ในทำนองเดียวกัน สร้าง *edge* น้ำหนัก $0$ เชื่อมระหว่างโหนดสมมุติของ *node* ที่มีพิกัดในแนวแกน $x$ เป็น $L$ ที่อยู่ติดกันในแนวตั้ง  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-3.png)  

จากกราฟที่เรา model ขึ้นมา สามารถสังเกตุได้ว่า   
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-4.png)  
=======
ความท้าทายของโจทย์ข้อนี้คือการคิดและ implement วิธีสร้างกราฟขึ้นมาเพื่อนำมาใช้แก้ปัญหา Shortest Path

ย้อนกลับมายังกราฟเริ่มต้นของชุดทดสอบตัวอย่างที่ $1$ 
* สร้าง "**โหนดสมมุติ**" ขึ้นมา *4* โหนด รอบสี่ทิศแนวทะแยงของแต่ละ *node* ดังรูป
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph4.png)
* สร้างเส้นเชื่อมระหว่างโหนดสมมุติภายใน *node* ที่อยู่ในแนวตั้งหรือแนวนอนเดียวกัน
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5.png)
* หากเป็น *edge* ที่มีปลายหนึ่ง(หรือทั้งสอง)เป็นโหนดสมมุติที่อยู่บนฝั่ง(กล่าวคือไม่ได้อยู่ในแม่น้ำ) ให้มีน้ำหนักเป็น $\infty$
* หากเป็น *edge* ที่ตัดกับเส้นเชื่อม*ในกราฟตั้งต้น* ให้มีน้ำหนักเท่ากับเส้นเชื่อมนั้น
* หากเป็น *edge* ที่ไม่ได้ตัดกับเส้นเชื่อมในกราฟตั้งต้น ให้มีน้ำหนักเป็น $0$
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-2.png)
* สำหรับคู่ *node* ที่มี *edge* เชื่อมในกราฟตั้งต้น, ให้สร้างเส้นเชื่อมน้ำหนัก $0$ เชื่อมสองคู่โหนดสมมุติที่อยู่ใกล้กันในแนว *edge* ดังกล่าว 
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-3.png)
* ท้ายที่สุด สร้าง *edge* น้ำหนัก $0$ เชื่อมระหว่างโหนดสมมุติของ *node* ที่มีพิกัดในแนวแกน $x$ เป็น $0$ ที่อยู่ติดกันในแนวตั้ง (ดังรูปด้านล่าง)
* ในทำนองเดียวกัน สร้าง *edge* น้ำหนัก $0$ เชื่อมระหว่างโหนดสมมุติของ *node* ที่มีพิกัดในแนวแกน $x$ เป็น $L$ ที่อยู่ติดกันในแนวตั้ง
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-4.png)

จากกราฟที่เรา model ขึ้นมา สามารถสังเกตุได้ว่า
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph5-5.png)
>>>>>>> 20f14dc12d569f4dc64b585deba0db64a72b89ab
* การเดินทางระหว่างโหนดสมมุติใน *Region* เดียวกัน จะไม่ได้รับน้ำหนักรวมเพิ่ม
* การเดินทางระหว่างโหนดสมมุติที่อยู่ต่าง *Region* กัน น้ำหนักรวมจะเพิ่มขึ้นตามน้ำหนักของ *edge* ที่เราก้าวข้าม

จากสมบัติดังกล่าว การเดินทางข้ามไปมาระหว่าง *Region* บนกราฟที่เรา model ขึ้นมานี้ มีสมบัติไม่แตกต่างกับกราฟของปัญหา *Single-Source Shortest Path* ก่อนหน้านี้เลย กล่าวคือ เราสามารถใช้กราฟนี้แทนได้

<<<<<<< HEAD
ในที่สุด เราสามารถใช้ **Dijkstra's Algorithm** บนกราฟนี้ โดยให้โหนดสมมุติทั้งหมดที่อยู่ใน *Start Region* เป็นจุดเริ่มต้น ค่า shortest path ที่เราได้จากโหนดสมมุติใน *End Region* จะเป็นค่า *Minimum Cut* ซึ่งก็คือคำตอบของปัญหาของเรานั่นเอง

อย่างไรก็ดี เรา**ไม่จำเป็น**ต้องใช้โหนดสมมุติใน *Start Region* *ทุกโหนด*เป็นจุดเริ่มต้นก็ได้ เราสามารถใช้โหนดใดโหนดหนึ่งก็เพียงพอ เพราะการเดินทางระหว่างโหนดสมมุติภายใน *region* ข้างบนสุดย่อมไม่ได้น้ำหนักเพิ่มอยู่แล้ว  
หากแต่ด้วยข้อจำกัดของการ model กราฟของเรา การเลือกใช้โหนดสมมุติของโหนดใน "component ลอย"ใน *Start Region* อาจจะทำให้เราสามารถ Dijkstra ไปถึงโหนดสมมุติใน *End Region* ได้ พิจารณากรณีดังต่อไปนี้  
![](https://beta-programming-in-th.s3-ap-southeast-1.amazonaws.com/solutions/media/o56_apr19_ants/graph6.png)   
หากเราเลือกโหนดสมมุติของโหนด $(1,3)$ หรือ $(1,4)$ เป็นจุดเริ่มต้น เราจะไม่สามารถ Dijkstra ไปถึง *End Region* เพราะไม่มี *edge* เชื่อมลงไปข้างล่าง  
ดังนั้น เราควรเลือกโหนดสมมุติที่เรามั่นใจว่าไม่เป็นของ "component ลอย" เป็นจุดเริ่มต้น ยกตัวอย่างเช่น **โหนดสมมุติบนขวาของโหนดที่มีพิกัดแกน $x$ เป็น $0$ ที่อยู่บนสุด** หรือ **โหนดสมมุติบนซ้ายของโหนดที่มีพิกัดแกน $x$ เป็น $L$ ที่อยู่บนสุด** เป็นต้น 

=======
ในที่สุด เราสามารถใช้ **Dijkstra's Algorithm** บนกราฟนี้ โดยให้โหนดสมมุติทั้งหมดที่อยู่ใน *Start Region* เป็นจุดเริ่มต้น ค่า shortest path ที่เราได้จากโหนดสมมุติใน *End Region* จะเป็นค่า *Minimum Cut* ก็คือคำตอบของปัญหาของเรานั่นเอง
>>>>>>> 20f14dc12d569f4dc64b585deba0db64a72b89ab

Overall Time Complexity : $\mathcal{O}(M \log M)$

Space Complexity : $\mathcal{O}(M)$
