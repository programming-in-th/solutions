โจทย์์ข้อนี้สามารถแก้ได้ด้วยการใช้เทคนิค **Depth First Search** หรือ **DFS** โดยจะใช้ recursive function ในการ implement โดย มีตัวแปรเก็บผลลัพธ์ในแต่ละรอบ, เก็บจำนวนครั้งชัยชนะของทีม ก, จำนวนครั้งการพ่ายแพ้ของทีม ก, ผลลัพธ์ในการแข่งขันรอบปัจจุบัน และ รอบการแข่งขัน
```cpp
#include<stdio.h>
#include<string.h>
int win,lose,judge,i;
char ans[200001]; // tracking result
int result(int win,int lose,char round,int check) // win, lose, result in current round, number of the current round
{
    if(win>judge||lose>judge)
    {
        return 0;
    }
    if(check!=0) // 0th round doesn't count
        ans[check] = round;
    if(win==judge||lose==judge) // print result
    {
        int len;
        len = strlen(ans);
        for(i = 1 ;i <= check ; i++)
        {
            printf("%c ",ans[i]); // print result
        }
        printf("\n");
        return 0;
    }

    result(win+1,lose,'W',check+1); // 'W' case in check+1th round
    result(win,lose+1,'L',check+1); // 'L' case in check+1th round
}
main()
{
    scanf("%d",&judge);
    scanf("%d",&win);
    scanf("%d",&lose);
    result(win,lose,'W',0);
}
```