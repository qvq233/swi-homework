# 智能蛇实验报告

*前面我们制作了一个贪吃蛇，但是需要人来控制，不能自己移动，现在我们来制作一个能够自动移动的智能蛇。*

## sin-demo.c代码的编译运行

![](images\sin-demo.gif)

## 实现kbhit()

![](images\tty.gif)

## 编写智能算法

通过编写算法，使智能蛇能每秒自动走一步。

为了加大难度，在地图设置了一些障碍物，当蛇撞到障碍物时，游戏结束。

![](images\snake.gif)


![](images\snake2.gif)


代码如下

```
#include <stdio.h>
#include <string.h>
#include <sys/time.h>
#include <sys/types.h>
#include <unistd.h>
#include <termios.h>
#include <unistd.h>
#include <time.h>
#include <math.h>
#include <stdlib.h>

#define SNAKE_MAX_LENGTH 20
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'

static struct termios ori_attr, cur_attr;

static __inline
int tty_reset(void)
{
        if (tcsetattr(STDIN_FILENO, TCSANOW, &ori_attr) != 0)
                return -1;

        return 0;
}


static __inline
int tty_set(void)
{

        if ( tcgetattr(STDIN_FILENO, &ori_attr) )
                return -1;

        memcpy(&cur_attr, &ori_attr, sizeof(cur_attr) );
        cur_attr.c_lflag &= ~ICANON;
//        cur_attr.c_lflag |= ECHO;
        cur_attr.c_lflag &= ~ECHO;
        cur_attr.c_cc[VMIN] = 1;
        cur_attr.c_cc[VTIME] = 0;

        if (tcsetattr(STDIN_FILENO, TCSANOW, &cur_attr) != 0)
                return -1;

        return 0;
}

static __inline
int kbhit(void)
{

        fd_set rfds;
        struct timeval tv;
        int retval;

        /* Watch stdin (fd 0) to see when it has input. */
        FD_ZERO(&rfds);
        FD_SET(0, &rfds);
        /* Wait up to five seconds. */
        tv.tv_sec  = 0;
        tv.tv_usec = 0;

        retval = select(1, &rfds, NULL, NULL, &tv);
        /* Don't rely on the value of tv now! */

        if (retval == -1) {
                perror("select()");
                return 0;
        } else if (retval)
                return 1;
        /* FD_ISSET(0, &rfds) will be true. */
        else
                return 0;
        return 0;
}

char map[12][12] =					\\地图
    {"************",
    "*XXXXH     *",
    "*          *",
    "*          *",
    "*      *   *",
    "*    *     *",
    "*    *     *",
    "*  ***     *",
    "*          *",
    "*          *",
    "*          *",
    "************"};

//define vars for snake, notice name of vars in C
int snakeX[SNAKE_MAX_LENGTH]={1,2,3,4,5};
int snakeY[SNAKE_MAX_LENGTH]={1,1,1,1,1};
static int snakeLength = 5;
static int isOver = 0;			\\判断是否结束
static int fx,fy;
static char ch;

void put_money(void)					\\放置食物
{
    srand(time(NULL));
    fx = rand()%10 + 1;
    fy = rand()%10 + 1;
    while(map[fy][fx]!=' ')					\\寻找空位置
    {
        fx = rand()%10 + 1;
        fy = rand()%10 + 1;					
    }
    map[fy][fx] = '$';
}

void snakeMove(int y,int x)
{
    if(snakeLength==SNAKE_MAX_LENGTH)				
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


char whereGoNext(int hx,int hy,int fx,int fy)
{
    char movable[4] = {'A','D','W','S'};
    int distance[4] = {0,0,0,0};
    int i,p = 0;

    for(i=0;i<4;i++)
    {
        switch(movable[i])
        {
            case 'A':distance[i] = abs(fx-(hx-1))+abs(fy-hy);
            if(map[hy][hx-1]!=' '&&map[hy][hx-1]!='$')
                distance[i] = 9999;
            break;
            case 'D':distance[i] = abs(fx-(hx+1))+abs(fy-hy);
            if(map[hy][hx+1]!=' '&&map[hy][hx+1]!='$')
                distance[i] = 9999;
            break;
            case 'W':distance[i] = abs(fx-hx)+abs(fy-(hy-1));
            if(map[hy-1][hx]!=' '&&map[hy-1][hx]!='$')
                distance[i] = 9999;
            break;
            case 'S':distance[i] = abs(fx-hx)+abs(fy-(hy+1));
            if(map[hy+1][hx]!=' '&&map[hy+1][hx]!='$')
                distance[i] = 9999;
            break;
        }
    }
    for(i=1;i<4;i++)
        p = distance[i]<distance[p]?i:p;
    return movable[p];
}
//将你的 snake 代码放在这里

int main()
{
        //设置终端进入非缓冲状态
        int tty_set_flag;
        tty_set_flag = tty_set();

        //将你的 snake 代码放在这里    
    	put_money();
    	output();
   	while(!isOver)
    	{
    	    usleep(100000);
     	    ch = whereGoNext(snakeX[snakeLength-1],snakeY[snakeLength-1],fx,fy);
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

   	   printf("pressed `q` to quit!\n");
     	   while(1) {

              	  if( kbhit() ) {
               	         const int key = getchar();
              	          printf("%c pressed\n", key);
               	         if(key == 'q')
                                break;
               		 } else {
                       ;// fprintf(stderr, "<no key detected>\n");
              	  }
      	  }

        //恢复终端设置
        if(tty_set_flag == 0)
                tty_reset();
        return 0;
}
```

由于对算法不是很精通，还有很多BUG，智能蛇也不“智能”，有时候会陷入死循环，有时候会撞向自己，算法的很多地方都需要修改完善。emmmm，问题比较严重……

以上就是智能蛇的实验报告。

*Thanks for watching!*