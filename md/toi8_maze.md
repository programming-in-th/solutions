### Solution
ข้อนี้สามารถแก้ได้ด้วยการใช้ เทคนิค **Breadth First Search** หรือ  **BFS** โดยในข้อนี้จะทำการ BFS 
สองครั้งด้วยตัวเลขที่ต่างกันที่ต่างกัน ให้ $type(i,j)$ คือตัวแปรเก็บชนิด และ $dist(i,j)$ เก็บระยะทาง 
* ครั้งจะทำจากจุดเริ่มต้นไปจนถึงทุกจุดที่สามารถไปถึงได้ โดยไม่เดินทะลุกำแพง และแต่ละช่องที่ไปถึงได้จะเก็บค่า $type(i,j) = 1$ และ ค่าระยะทางจากจุดเริ่มต้นไว้ใน $dist(i,j)$
* ครั้งที่สองจะทำจากจุดจบไปจนถึงทุกจุดที่สามารถไปได้ถึงได้โดยไม่เดินทะลุกำแพงและแต่ละช่องที่ไปถึงได้จะเก็บค่า $type(i,j) = 2$ และค่าระยะทางจากจุดจบไว้ใน $dist(i,j)$

หลังจาก BFS ทั้งสองครั้งไล่ไปทุกๆ $(i,j)$ ที่ค่าช่อง $(i,j)$ เป็น 0 และสังเกตุว่า
* หากช่อง $(i,j)$ ใดๆมี $type(i,j)=1$ หมายความว่าสามารถไปถึงได้จากจุดเริ่มต้น
* หากช่อง $(i,j)$ ใดๆมี $type(i,j)=2$ หมายความว่าสามารถไปถึงได้จากจุดจบ

จากนั้นดูค่าของแต่ละคู่ช่องดังนี้ พร้อมทั้งเก็บระยะทางน้อยที่สุดจากผมรวมของ $dist(i,j)$ ของแต่ละตัวในแต่ละคู่
* ค่าช่อง $type(i+1,j) + type(i-1,j) = 3$ 
* ค่าช่อง $type(i+1,j) + type(i,j-1) = 3$
* ค่าช่อง $type(i+1,j) + type(i,j+1) = 3$
* ค่าช่อง $type(i-1,j) + type(i,j+1) = 3$
* ค่าช่อง $type(i-1,j) + type(i,j-1) = 3$
* ค่าช่อง $type(i,j+1) + type(i,j-1) = 3$

หากมีข้อใดข้อหนึ่งเปนจริงหมายความว่าหากพังกำแพงนี้สามารถเดินทางจากจุดเริ่มต้นมาถึงจุดจบได้และนับเพิ่มในคำตอบ

```cpp
#include<stdio.h>
#include<queue>

using namespace std;

int i, j, startx, starty, endx, endy, maze[200][200], m, n, mins = 500, counts, check;
struct flag{
    int level = 0; // dist
    int types; // type
}mark[200][200];
struct maps{
    int x, y, lv;
    int type;
}temp;
queue<maps> q, p;
int main()
{
    scanf("%d %d",&m,&n);
    scanf("%d %d",&startx,&starty);
    scanf("%d %d",&endx,&endy);
    for( i = 1 ; i <= m ; i++ ){
        for( j = 1 ; j <= n ; j++ ){
            scanf("%d",&maze[i][j]);
        }
    }
    // 1st BFS from beginning
    q.push( { startx, starty, 1, 1 } );
    while( !q.empty() ){
        temp = q.front();
        mark[temp.x][temp.y].level = temp.lv;
        mark[temp.x][temp.y].types = temp.type;
        q.pop();
        if( maze[temp.x - 1][temp.y] == 1 && mark[temp.x - 1][temp.y].level == 0 ){
            mark[temp.x - 1][temp.y].level = temp.lv + 1;
            mark[temp.x - 1][temp.y].types = temp.type;
            q.push({ temp.x - 1 , temp.y , temp.lv + 1 , 1 });
        }
        if( maze[temp.x][temp.y - 1] == 1 && mark[temp.x][temp.y - 1].level == 0 ){
            mark[temp.x][temp.y - 1].level = temp.lv + 1;
            mark[temp.x][temp.y - 1].types = temp.type;
            q.push({ temp.x , temp.y - 1 , temp.lv + 1 , 1 });
        }
        if( maze[temp.x][temp.y + 1] == 1 && mark[temp.x][temp.y + 1].level == 0 ){
            mark[temp.x][temp.y + 1].level = temp.lv + 1;
            mark[temp.x][temp.y + 1].types = temp.type;
            q.push({ temp.x , temp.y + 1 , temp.lv + 1 , 1 });
        }
        if( maze[temp.x + 1][temp.y] == 1 && mark[temp.x + 1][temp.y].level == 0 ){
            mark[temp.x + 1][temp.y].level = temp.lv + 1;
            mark[temp.x + 1][temp.y].types = temp.type;
            q.push({ temp.x + 1 , temp.y , temp.lv + 1 , 1 });
        }
    }
    // 2nd BFS from the end
    p.push( { endx, endy, 1, 2 } );
    while( !p.empty() ){
        temp = p.front();
        mark[temp.x][temp.y].level = temp.lv;
        mark[temp.x][temp.y].types = temp.type;
        p.pop();
        if( maze[temp.x - 1][temp.y] == 1 && mark[temp.x - 1][temp.y].level == 0 ){
            mark[temp.x - 1][temp.y].level = temp.lv + 1;
            mark[temp.x - 1][temp.y].types = temp.type;
            p.push({ temp.x - 1 , temp.y , temp.lv + 1 , 2 });
        }
        if( maze[temp.x][temp.y - 1] == 1 && mark[temp.x][temp.y - 1].level == 0 ){
            mark[temp.x][temp.y - 1].level = temp.lv + 1;
            mark[temp.x][temp.y - 1].types = temp.type;
            p.push({ temp.x , temp.y - 1 , temp.lv + 1 , 2 });
        }
        if( maze[temp.x][temp.y + 1] == 1 && mark[temp.x][temp.y + 1].level== 0 ){
            mark[temp.x][temp.y + 1].level = temp.lv + 1;
            mark[temp.x][temp.y + 1].types = temp.type;
            p.push({ temp.x , temp.y + 1 , temp.lv + 1 , 2 });
        }
        if( maze[temp.x + 1][temp.y] == 1 && mark[temp.x + 1][temp.y].level == 0 ){
            mark[temp.x + 1][temp.y].level = temp.lv + 1;
            mark[temp.x + 1][temp.y].types = temp.type;
            p.push({ temp.x + 1 , temp.y , temp.lv + 1 , 2 });
        }
    }
    // Calculating minimum distance and counting walls
    for( i = 1 ; i <= m ; i++ ){
        for( j = 1 ; j <= n ; j++ ){
                check = 0; // check if this wall is one of the answer
            if( maze[i][j] == 0 ){
                if( mark[i - 1][j].types + mark[i][j - 1].types == 3 ){ // 1st case
                    if( mark[i - 1][j].level + mark[i][j - 1].level < mins ){
                        mins = mark[i - 1][j].level + mark[i][j - 1].level; // check min
                    }
                    check = 1; 
                }
                if( mark[i - 1][j].types + mark[i][j + 1].types == 3 ){ // 2nd case
                    if( mark[i - 1][j].level + mark[i][j + 1].level < mins ){
                        mins = mark[i - 1][j].level + mark[i][j + 1].level; // check min
                    }
                    check = 1;
                }
                if( mark[i - 1][j].types + mark[i + 1][j].types == 3 ){ // 3rd case
                    if( mark[i - 1][j].level + mark[i + 1][j].level < mins ){
                        mins = mark[i - 1][j].level + mark[i + 1][j].level; // check min
                    }
                    check = 1;
                }
                if( mark[i + 1][j].types + mark[i][j - 1].types == 3 ){ // 4th case
                    if( mark[i + 1][j].level + mark[i][j - 1].level < mins ){
                        mins = mark[i + 1][j].level + mark[i][j - 1].level; // check min
                    }
                    check = 1;
                }
                if( mark[i + 1][j].types + mark[i][j + 1].types == 3 ){ // 5th case
                    if( mark[i + 1][j].level + mark[i][j + 1].level < mins ){
                        mins = mark[i + 1][j].level + mark[i][j + 1].level; // check min
                    }
                    check = 1;
                }
                if( mark[i][j - 1].types + mark[i][j + 1].types == 3 ){ // 6th case
                    if( mark[i][j - 1].level + mark[i][j + 1].level < mins ){
                        mins = mark[i][j - 1].level + mark[i][j + 1].level; // check min
                    }
                    check = 1;
                }
                if( check ){
                    counts++; // count ans
                }
            }
        }
    }
    printf("%d\n%d",counts,mins + 1);
    return 0;
}
```
