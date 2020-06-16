โจทย์ข้อนี้ใช้วิธีการไล่ตรงๆในการคิด โดยมีตัวแปรเก็บจำนวนครั้งที่ติดกันของ **จำนวนคู่** และ **จำนวนคี่** เพื่อคิดค่าความเสียหายในการโจมตีแต่ละครั้ง 
``` cpp
#include<stdio.h>
int odden, evenen, i, en, counteven, countodd, atk;
int main()
{
    scanf("%d",&en);
    odden = en;
    evenen = en;

    for( i = 0 ; i < 2 * en ; i++ )
    {
        scanf("%d",&atk);

        if( atk % 2 == 1 )
        {
            counteven = 0 ;
            if( countodd + 1 >= 3 )
            {
                evenen -= 3;
                countodd++;
                if( evenen <= 0 )
                {
                    printf("1\n%d",atk);
                    break;
                }
            }
            else
            {
                evenen -= 1;
                countodd++;
                if( evenen <= 0 )
                {
                    printf("1\n%d",atk);
                    break;
                }
            }
        }
        else
        {
            countodd = 0 ;
            if( counteven + 1 >= 3 )
            {
                odden -= 3;
                counteven++;
                if( odden <= 0 )
                {
                    printf("0\n%d",atk);
                    break;
                }
            }
            else
            {
                odden -= 1;
                counteven++;
                if( odden <= 0 )
                {
                    printf("0\n%d",atk);
                    break;
                }
            }
        }
    }
    return 0;
}
```