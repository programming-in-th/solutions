## Subtask 1, 2 ($R \leq 7$, $C \leq 1\,000$)

**Dynamic Programming** สามารถนำมาใช้แก้โจทย์ที่ถามหาจำนวนรูปแบบที่เป็นไปได้ทั้งหมดของปัญหาบางอย่างได้อย่างมีประสิทธิภาพ  

จากเงื่อนไขของโจทย์ สังเกตได้ว่าจำนวนแถวนั้นมีค่าน้อยมาก ๆ ($R \leq 7$)  
เราจึงสามารถนิยาม Recurrence Formula โดยใช้ *bitmask* เป็นตัวบ่งบอก *state* ของ *column* ล่าสุดว่า แถวใดบ้างที่ถูกโดมิโนวางทับไปแล้ว  

### Definition 

สมมุติหมายเลขของแถวและคอลัมน์เริ่มที่หมายเลข 1

**นิยาม** $dp[i][j] =$ จำนวนรูปแบบในการวางโดมิโนลงใน *column* ที่ 1 จนถึง *column* ที่ $i$ โดยที่ ใน *column* ที่ $i$ หากบิตที่ $k$ ของค่า $j$ มีค่าเป็น 1 หมายความว่าช่องในแถวที่ $k$ นั้นได้ถูกโดมิโนวางทับไปแล้ว และในทางกลับกันหากบิตที่ $k$ มีค่าเป็น 0 ช่องนั้นยังคงว่างอยู่  

ยกตัวอย่างเช่น $dp[5][0100101_2] =$ จำนวนรูปแบบในการวางโดมิโนลงใน *column* ที่ 1 จนถึง *column* ที่ 5 โดยที่ ใน *column* ที่ 5 ช่องในแถวที่ 1, 3, 6 ได้ถูกโดมิโนวางทับไปแล้ว ในขณะที่ช่องในแถวที่ 2, 4, 5, 7 ยังว่างอยู่  

### Base Case

เนื่องจากเราเริ่มวางจาก *column* ที่ 1 ดังนั้น ใน ***column* ที่ 0** จึงไม่มีช่องใดสามารถวางโดมิโนได้เลย *เสมือนว่าทุกช่องนั้นได้ถูกโดมิโนวางทับไปแล้ว* ดังนั้นเราจึงกำหนดให้ $dp[0][2^r-1]=1$ และให้ $dp[0][j]$ สำหรับ $j$ อื่น ๆ มีค่าเป็น $0$ ทั้งหมด  

### Transition
เนื่องจากการไล่กำหนด *recurrence* ด้วยมือนั้นเป็นเรื่องยากมาก ๆ เพราะเราจำเป็นต้องพิจารณาการวางโดมิโนบน *column* ล่าสุดและ *column* ที่แล้ว**ทุกวิธี** 

ดังนั้น เราจะเขียนโปรแกรมให้**ทดลองวางโดมิโนทุกวิธี** (จำนวนวิธีทั้งหมดจะมีไม่เกิน $3^r$  วิธี) แล้วดูว่าในการวางแต่ละวิธี สามารถ *transition* จากรูปแบบใดของ *column* ที่แล้วได้บ้าง 

หลังจากนี้จะขอเรียก รูปแบบของช่องที่ถูกโดมิโนวางทับใน *column* ล่าสุดของ $dp[i][j]$ ว่า "รูปแบบ $j$"

**นิยาม** $transition[n][m]$ คือ จำนวนวิธีที่เป็นไปได้ที่การวางโดมิโน *รูปแบบ* $n$ บน *column* ที่แล้ว จะสามารถ *transition* มาเป็น *รูปแบบ* $m$ บน *column* ปัจจุบัน  

สมมุติว่าปัจจุบันเราอยู่ *column* ที่ $i$

ในการ **Brute Force** เราจะลองวางโดมิโนทุกวิธี**ที่ไม่กระทบกับ column ที่ i+1** บน *column* ที่ $i$ โดยเราจะเก็บ ***state* ของช่องที่ถูกวางทับใน *column* ที่ $i$** (หลังจากนี้ขอเรียกว่า *current_col*) และ ***state* ของช่องที่ถูกวางทับใน *column* ที่ $i-1$** (หลังจากนี้ขอเรียกว่า *prev_col*) ไปด้วย  

สมมุติว่าตอนนี้เรากำลังพิจารณาการวางโดมิโนวิธีหนึ่งอยู่ ให้เราดูว่าเราสามารถวางวิธีนี้กับ *รูปแบบ* ใดของ *column* ที่ $i-1$ ได้บ้าง  

สมมุติว่าเรากำลังพิจารณา *รูปแบบ* $x$ อยู่ เราสามารถใช้ *prev_col* เป็นตัวเช็คได้ว่ามีการวางโดมิโนซ้อนกันหรือไม่  

หากไม่มีโดมิโนซ้อนกัน จะได้ว่า "การวางวิธีนี้กับ *รูปแบบ* $x$ ของ *column* ที่แล้ว" เป็นหนึ่งวิธีในการ *transition* จาก *รูปแบบ* $x$  ของ *column* ที่แล้ว มายัง *รูปแบบ current_col* ใน *column* ปัจจุบัน นั่นคือ $transition[x][current\_col]$ จะมีค่าเพิ่มขึ้น 1

ด้านล่างนี้ เป็นตัวอย่างของโค้ดในการ *brute force*  และสร้างตาราง *transition* ขึ้นมา

```cpp
int r, c;
int transition[1 << 7][1 << 7]; 
void brute_force(int current_row = 0, int current_col = 0, int prev_col = 0) {
  // start from 0 for bitwise operation purpose
    
  if (current_row == r) { // finish placing
    for (int i = 0; i <= (1 << r) - 1; ++i) {
	  // try pairing with every possible states of previous column
      if (i & prev_col) // have overlapped dominoes
        continue;
      ++transition[i][current_col];
    }
    return;
  }

  int bit = (1 << current_row), next_bit = (1 << (current_row + 1));

  // placing a leftward domino
  brute_force(current_row + 1, current_col | bit, prev_col | bit);
  // placing a downward domino
  if (current_row < r - 1)
    brute_force(current_row + 2, current_col | bit | next_bit, prev_col);
  // not placing a domino
  brute_force(current_row + 1, current_col, prev_col);
}
```
### Recurrence

เราสามารถใช้ตารางนั้นเป็นตัวบอกว่า ใน *column* ที่ $i$ รูปแบบ $j$ จะ *transition* จาก *รูปแบบ* ของ *column* ที่ $i-1$ อย่างละกี่วิธี  

$$dp[i][j] = \sum_{k=0}^{2^r-1} dp[i-1][k]*transition[k][j]$$    

Time Complexity : $\mathcal{O}(3^r*2^r+C*(2^r)^2)$  
Space Complexity : $\mathcal{O}((2^r)^2+C*2^r)$  

## Subtask 3 ( R <= 7, C <= 10^9 )  

ในขณะเดียวกัน บ่อยครั้งมากที่เราสามารถใช้ [**Matrix Exponentiation**](https://www.hackerearth.com/practice/notes/matrix-exponentiation-1/) มาพัฒนาประสิทธิภาพในการแก้ปัญหาด้วย **Dynamic Programming** ที่ตัวแปรบางตัวมีค่าสูงมากๆได้

ในการแก้ปัญหาย่อยที่ 3 เราสามารถใช้ตาราง *transition* ก่อนหน้านี้มาใช้ร่วมกับเทคนิค **Matrix Exponentiation** ซึ่งช่วยทำให้สามารถคำนวณคำตอบสำหรับ *test case*  ที่มีค่า $C$ เยอะมากๆ ได้อย่างมีประสิทธิภาพ  

Time Complexity : $\mathcal{O}(3^r 2^r+(2^r)^3 \log{C})$  
Space Complexity : $\mathcal{O}((2^r)^2+(2^r)^2 \log{C})$  
