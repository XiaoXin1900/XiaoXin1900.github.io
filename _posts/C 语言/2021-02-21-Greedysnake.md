---
layout: post
title: C 语言贪吃蛇
categories: [C 语言]
description: C 语言贪吃蛇
keywords: C 语言
---

> C 语言贪吃蛇的开发历程

### 效果展示

<img src="https://i.loli.net/2021/02/17/WZI8nvRD4zHJT9s.gif" alt="snake" style="zoom: 50%;" />

文件下载：
<a href="https://xiaoxin1900.club/Softwares/Snake.zip" download = Snake.zip>贪吃蛇压缩包(编码方式：GBK)</a>

### 支持功能

* 升级
* 死亡后退出
* 开始页面
* 等等~~

____

### 大体思路

> 编写主要函数。

#### 制作地图

> 一个矩形框

1. 定义长和高和整个地图。

   ```c
   #define H 31
   #define W 91
   int map[H][W] = {0};     //全局变量
   ```

2. 将地图上为墙体的部分标记为 1，其他标记为 0。

   ```c
   void init_map()
   {
     for(int i = 0; i < W; i++)
     {
       map[0][i] = 1;
       map[H - 1][i] = 1;
     }
     for(int j = 0; j < H; j++)
     {
       map[j][0] = 1;
       map[j][W - 1] = 1;
     }
   }
   ```

   

#### 初始化地图和蛇

1. 将上一步的标记的墙体打印，作为地图。

   ```c
   void drawmap()
   {
     gotoxy(0, 0);           //到（0，0）处开始打印地图
     for(int i = 0; i < H; i++)
     {
       for(int j = 0; j < W; j++)
       {
         if(map[i][j] == 1)
           printf("#");
         else
           printf(" ");
       }
       printf("\n");
     }
   }
   ```

   注意到 **gotoxy** 函数非库函数，其函数体如下：

   ```c
   void gotoxy(int x, int y);
   {
      COORD position = {y, x};
      SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), position);
   }
   ```

   COORD 为 windows API 中定义的一种结构，表示光标在控制台屏幕上的坐标，这个函数暂时不必深究。
   **值得注意的是，gotoxy(x, y); 控制台屏幕第一个位置坐标为（0，0）,向下 x 增加， 向右 y 增加。**

2. 初始化蛇，定义变量。

   ```c
   int snakelength;    //全局变量
   int snake[H * W][2];    //用于储存蛇的坐标 //可以使用结构体
   ```

   初始化蛇

   ```c
   void init_snake()
   {
     snakelength = 4;     //开始长度为 4
     snake[0][0] = H/2;
     snake[0][1] = W/2;
     for(int i = 1; i < 4; i++)
     {
       snake[i][0] = snake[0][0] + i;
       snake[i][1] = snake[0][0];          //蛇开始时为竖着的
     }
   }
   ```

   
打印开始时的蛇
   
   ```C
   void drawsnake()
   {
     for(int i = 0; i < snakelength; i++)
     {
       gotoxy(snake[i][0], snake[i][1]);
       printf("@");
     }
   }
   ```

#### 实现蛇的移动和转向

1. 定义方向变量

   ```c
   int direction;
   #define UP 0
   #define DOWN 1
   #define LEFT 2
   #define RIGHT 3
   ```

2. 蛇的移动

   ```c
   void move()
   {
     int i;
     for(i = snakelength - 1; i > 0; i--)
     {
    snake[i][0] = snake[i - 1][0];
       snake[i][1] = snake[i - 1][1];     //蛇的坐标移动，之后用 draw 函数画出即可
     }
     switch(direction)               //改蛇头坐标
     {
         case UP:
             snake[0][0]--;
             break;
         case DOWN:
             snake[0][0]++;
             break;
         case LEFT:
             snake[0][1]--;
             break;
         case RIGHT:
             snake[0][1]++;
             break
     }
   }
   ```
   
   

#### 擦除尾巴

在 move 函数里把下面一段代码加到 3、4行之间即可。

> 基本思路：光标跳转到蛇尾，然后输出空格擦除尾巴

```c
  gotoxy(snake[snakelength-1][0],snake[snakelength-1][1]);
    printf(" ");                            //在尾巴上面画空格以擦除尾巴
```



#### 投放食物

投放食物，那么投放位置应该随机，同时不应在地图之外，这里我们需要使用 rand 随机函数生成点位同时写 check 函数用于判断随机点的可用性。

```c
int check(int x, int y)          //检查食物投放点
{
  if(map[y][x] == 1)          //不与障碍物、地图重合
    return 0;

  for(int i = snakelength - 1; i > 0; i --)
    {
      if(((y == snake[i][0]) && (x == snake[i][1])) || ((y > H)||(x > W)))    //不与蛇重合且不超出边界
      return 0;
    }

  return 1;
}
```

```c
void food()             //投放食物
{
 int x, y;
   srand((unsigned)time(NULL));            //设置随机数种子为现在的时间
 do
 {
   x = rand()%W;
   y = rand()%H;
 }while(check(x, y) == 0);
 map[y][x] = -1;
 gotoxy(y, x);

 printf("$");        //空投食物

}
```



#### 摄取食物变长

基本思路是，蛇头碰到食物，那么食物由蛇头替代（这里只需继续移动即可），并且蛇尾增加一个单位。

```c
void getlength()
{
  move();      //继续移动
  gotoxy(snake[snakelength - 1][0], snake[snakelengeh][1]);
  printf("@");
  int x = snake[snakelengeh - 1][0]; int y = snake[snakelength - 1][1];
  //给新蛇尾的坐标
  snakelength++;
  snake[snakelength - 1][0] = x;  snake[snakelength - 1][1] = y;           //赋予坐标，接上上个蛇尾
}
```



#### 游戏升级

写一个 progress 函数，用于增加分数并记录，达到多少之后，等级提升，同时 wait_time 即每次移动间隔时间减少

> sources 、rank 已定义

```c
void progress()       //游戏进度
{
 if((sources%10 == 0) && (sources != 0))
   {
    rank++;
    wait_time = wait_time - 50;
   }

 sources++;
 gotoxy(H, 6); printf("%d", rank);                   //在指定位置记录数据并显示
 gotoxy(H + 1, 9); printf("%d", sources);
}

```



#### 游戏结束

> 游戏结束，那么我们需要写一个函数，判断蛇是否存活。

```c
void life()
{
  if(map[snake[0][0]][snake[0][1]] == 1)         //撞墙
   return 1;

  for(int i = 1; i < snakelength; i ++)       //咬到自己
  {
   if((snake[0][1] == snake[i][1]) && (snake[0][0] == snake[i][0]))
     return 1;
  }
  return 0; 
}
```



#### 游戏通关

> 一个 success 函数，通关界面（此时wait_time = 0)

```
void Success()
{
  Sleep(300);
  cls();                           
  gotoxy(H/2, W/2);
  printf("You win!");
  gotoxy(H/2 + 1, W/2);
  printf("press any key to exit");
  getch();
  exit(0);                    //按任意键正常退出
}
```

**cls()是一个自定义的清屏函数可以用 system("cls"); 代替**

#### 关于UI

压缩包里的源码包含了 start_UI 与 end_UI 俩个函数，这里不赘述，事实上，经过修改，可以去除。




_____
### 参考资料

1. <a href="https://zhuanlan.zhihu.com/p/321582318">知乎—喜欢编程的狗子 C 语言贪吃蛇</a>

