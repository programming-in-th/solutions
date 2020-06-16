โจทย์ข้อนี้สามรถแก้ได้ด้วยเทคนิค **Dynamic Programming** โดยนิยาม **DP(i,l)** คือ คำตอบที่ดีที่สุดเมื่อเลือกงาน $k$ มา $i$ งานและเลือกงาน $j$ มา $l$ งาน โดย $DP(i,l)$ จะเก็บ 2 ค่าคือ จำนวนวันที่น้อยที่สุดหรือ $DP(i,l).day$ และจำนวนนาทีในวันล่าสุดที่น้อยที่สุดหรือ $DP(i,l).min$
* จะได้ transition คือ $DP(i,l) = min( cost( DP(i,l-1),k(l) ), cost( DP(i-1,l),j(i) ) )$ เมื่อ $cost( x, y )$ คือเวลารวมที่ใช้เมื่อคิดเวลางาน $y$ เพิ่มเข้าไปในเวลา $x$ 
* โดย $cost( x, y )$ สามารถหาได้จาก การนำเวลา $y$ เพิ่มเข้าในเวลาของ $x.min$ หาผลรวมมีค่าเกิน $M$ หมายความว่าจำเป็นต้องย้ายงานใหม่ล่าสุดไปวันใหม่ เก็บ $x.min$ เป็นเวลาของงานล่าสุด และเพิ่มค่า $x.day$ ไป $1$


``` cpp
#include<stdio.h>
int m, n, l, i;
int j[1010],k[1010];
struct node{
    int day, min; // day, min minute
}temp, temp1, temp2;
struct node work[1010][1010];
int main()
{
    scanf("%d %d",&m,&n);
    for( i = 0 ; i < n ; i++ ){
        scanf("%d",&j[i]);
    }
    for( i = 0 ; i < n ; i++ ){
        scanf("%d",&k[i]);
    }
    // base case 
    for( i = 1 ; i <= n ; i++ ){
        temp.day = work[0][i - 1].day;
        temp.min = work[0][i - 1].min + j[i - 1];
        if( temp.min > m ){
            temp.day += 1;
            temp.min = j[i - 1];
        }
        work[0][i] = temp;
    }
    // base case
    for( i = 1 ; i <= n ; i++ ){
        temp.day = work[i - 1][0].day;
        temp.min = work[i - 1][0].min + k[i - 1];
        if( temp.min > m ){
            temp.day += 1;
            temp.min = k[i - 1];
        }
        work[i][0] = temp;
    }
    // Dynamic Programming
    for( i = 1 ; i <= n ; i++ ){
        for( l = 1 ; l <= n ; l++ ){
            // calculate cost( dp[i][l-1], j[l] )
            temp1.day = work[i][l - 1].day;
            temp1.min = work[i][l - 1].min + j[l - 1]; 
            if( temp1.min > m ){
                temp1.day += 1;
                temp1.min = j[l - 1];       
            }
            // calculate cost( dp[i-1][l], k[i] )
            temp2.day = work[i - 1][l].day;
            temp2.min = work[i - 1][l].min + k[i - 1];
            if( temp2.min > m ){
                temp2.day += 1;
                temp2.min = k[i - 1];       
            }
            // Find min( temp1, temp2 )
            if( temp1.day < temp2.day ){
                work[i][l] = temp1;        
            }
            else if( temp2.day < temp1.day ){
                work[i][l] = temp2;
            }
            else if( temp1.min <= temp2.min ){ // temp1.day = temp2.day check minute
                work[i][l] = temp1;
            }
            else{
                work[i][l] = temp2;
            }
        }
    }
    printf("%d\n%d",work[n][n].day + 1,work[n][n].min);
    return 0;
}
```