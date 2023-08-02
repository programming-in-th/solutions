### Abridged Problem Statement

รับค่า $N,K,L_{1,\dots,k},R_{1,\dots,k}$ มา จงสร้าง permutation P ขนาด N ขึ้นมาเพื่อที่ทำให้ $\sum\limits_{i=1}^{k}\max\limits_{j=L_i}^{R_i}\{P_j\}-\min\limits_{j=L_i}^{R_i}\{P_j\}$ มีค่ามากที่สุด

### Subtask 1

สำหรับปัญหาย่อยแรกนั้น เราสามารถสร้างทุก permutation เพื่อมาหาว่าค่ามากสุดคือเท่าใด

Time Complexity: $\mathcal{O}(N!\cdot NK)$

### Subtask 2 - 4

เนื่องด้วยเราต้องการค่า $\max-\min$ แสดงว่าเราต้องพยายามสร้าง permutation ที่ห่างกันให้มากที่สุด กำหนดให้ $z$ คือจำนวนช่วงที่เราต้องการพิจารณา และ $A=\{1,2,\dots,z,n-z+1,n-z+2,\dots,n\}$ คือเลขที่เราต้องการพิจารณาเพื่อสร้าง permutation ให้ตรงตามเงื่อนไขและมีค่ามากที่สุด กำหนดให้ $B=\{x\in P(A)\mid\left|x\right|=z\}$ ซึ่งขนาดของ $B$ นั้นไม่เกิน $O((2k)^k)$ เนื่องด้วยเราต้องการตรวจสอบทุกวิธี เวลารวมเลยเป็น $O((2k)^k(2k)!k)$ ซึ่งจะรันไม่ทันในกรณี $k$ มาก ๆ เราจึงทำการ optimize โดยที่สังเกตว่าบางเซตในเซตของ $B$ นั้นไม่สามารถใช้ได้แน่นอนเราจึง กำหนดให้ $B=\bigcup\limits_{i=0}^{z}\left\{\bigcup\limits_{j=1}^{z-i}\{j\}\cup\bigcup\limits_{j=n-i+1}^{z}\{j\}\right\}$ ทำให้ขนาดของ $B$ นั้นไม่เกิน $O(k)$ และเราใช้ algorithm เดิมในการลองทุกรูปแบบเพื่อหาคำตอบ

Time Complexity: $\mathcal{O}((2k)!k^2)$

### Subtask 5

เราสามารถใช้ randomize algorithm เพื่อ generate คำตอบที่ใกล้ เคียงที่สุดโดยการ shuffle permutation ของ $1,\dots,N$ และทำการ track ว่าคำตอบของ permutation นั้นน้อยกว่า permutation ก่อนหน้าหรือเปล่า

Time Complexity: $\mathcal{O}(N)$