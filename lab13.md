
# 贪吃蛇实验报告

## 会动的蛇

```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SNAKE_MAX_LENGTH 20
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'

//snake stepping : dy = -1(up),1(down); dx = -1(left),1(right),0(no move)
void snakeMove(int ,int);
//put a food randomized on a blank cell
void put_money(void);
//out cells of the grid
void output(void);
//outs when gameover
void gameover(void);

char map[12][12] =
    {"************",
    "*XXXXH     *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "************"};

//define vars for snake, notice name of vars in C
int snakeX[SNAKE_MAX_LENGTH]={1,2,3,4,5};
int snakeY[SNAKE_MAX_LENGTH]={1,1,1,1,1};
int snakeLength = 5;
int isOver = 0;

int main()
{
    char ch;
    output();
    while(!isOver)
    {
        scanf("%c", &ch);
        switch(ch)
        {
            case 'A':snakeMove(0,-1);
            break;
            case 'D':snakeMove(0,1);
            break;
            case 'W':snakeMove(-1,0);
            break;
            case 'S':snakeMove(1,0);
            break;
            default:continue;
        }
        output();
    }
    gameover();
    return 0;
}

void snakeMove(int y,int x)
{
    if(map[snakeY[snakeLength-1]+y][snakeX[snakeLength-1]+x]==' ')
    {
        map[snakeY[0]][snakeX[0]] = ' ';
        map[snakeY[snakeLength-1]][snakeX[snakeLength-1]] = 'X';
        map[snakeY[snakeLength-1]+y][snakeX[snakeLength-1]+x] = 'H';

        int i=0;

        while(i<snakeLength-1)
        {
            snakeX[i] = snakeX[i+1];
            snakeY[i] = snakeY[i+1];
            i++;
        }
        snakeX[snakeLength-1] = snakeX[snakeLength-2]+x;
        snakeY[snakeLength-1] = snakeY[snakeLength-2]+y;
    }
    else
        isOver = 1;
}

void output(void)
{
    if(!isOver)
    {
        int i,j;
        for(i=0;i<12;i++)
        {
            for(j=0;j<12;j++)
            {
                printf("%c",map[i][j]);
                if(j==11)
                    printf("\n");

            }
        }
    }
}

void gameover(void)
{
    printf("Game over\n");
}
```

## 会吃的蛇

```

\\当蛇撞墙或蛇的长度达到最大时，结束

#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SNAKE_MAX_LENGTH 20
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'

//snake stepping : dy = -1(up),1(down); dx = -1(left),1(right),0(no move)
void snakeMove(int ,int);
//put a food randomized on a blank cell
void put_money(void);
//out cells of the grid
void output(void);
//outs when gameover
void gameover(void);

char map[12][12] =
    {"************",
    "*XXXXH     *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "************"};

//define vars for snake, notice name of vars in C
int snakeX[SNAKE_MAX_LENGTH]={1,2,3,4,5};
int snakeY[SNAKE_MAX_LENGTH]={1,1,1,1,1};
int snakeLength = 5;
int isOver = 0;
int fx,fy;

int main()
{
    char ch;
    put_money();        \\放置食物
    output();           \\打印地图
    while(!isOver)
    {
        scanf("%c", &ch);
        switch(ch)
        {
            case 'A':snakeMove(0,-1);
            break;
            case 'D':snakeMove(0,1);
            break;
            case 'W':snakeMove(-1,0);
            break;
            case 'S':snakeMove(1,0);
            break;
            default:continue;
        }
        output();
    }
    gameover();
    return 0;
}

void snakeMove(int y,int x)
{
    if(snakeLength>SNAKE_MAX_LENGTH)
        isOver = 1;

    else if(map[snakeY[snakeLength-1]+y][snakeX[snakeLength-1]+x]==' ')
    {
        map[snakeY[0]][snakeX[0]] = ' ';
        map[snakeY[snakeLength-1]][snakeX[snakeLength-1]] = 'X';
        map[snakeY[snakeLength-1]+y][snakeX[snakeLength-1]+x] = 'H';

        int i=0;

        while(i<snakeLength-1)
        {
            snakeX[i] = snakeX[i+1];
            snakeY[i] = snakeY[i+1];
            i++;
        }
        snakeX[snakeLength-1] = snakeX[snakeLength-2]+x;
        snakeY[snakeLength-1] = snakeY[snakeLength-2]+y;
    }
    else if(map[snakeY[snakeLength-1]+y][snakeX[snakeLength-1]+x]=='$')
    {
        map[snakeY[snakeLength-1]+y][snakeX[snakeLength-1]+x] = 'H';
        map[snakeY[snakeLength-1]][snakeX[snakeLength-1]] = 'X';
        snakeLength++;
        snakeX[snakeLength-1] = snakeX[snakeLength-2]+x;
        snakeY[snakeLength-1] = snakeY[snakeLength-2]+y;
        put_money();
    }
    else
        isOver = 1;
}

void output(void)
{
    if(!isOver)
    {
        int i,j;
        for(i=0;i<12;i++)
        {
            for(j=0;j<12;j++)
            {
                printf("%c",map[i][j]);
                if(j==11)
                    printf("\n");

            }
        }
    }
}

void gameover(void)
{
    printf("Game over\n");
}

void put_money(void)
{
    srand(time(NULL));
    fx = rand()%10 + 1;
    fy = rand()%10 + 1;
    while(map[fy][fx]!=' ')
    {
        fx = rand()%10 + 1;
        fy = rand()%10 + 1;
    }
    map[fy][fx] = '$';
}

```