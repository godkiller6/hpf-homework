贪吃蛇，是一款经典的益智游戏。我们可以通过算法捕捉和传递智能，设计出能自动跑着吃食物的智能蛇。

有不少人对此研究，设计出了很棒的算法，如图：

![](https://img-blog.csdn.net/20171226165448392?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlla2No/font/5a6L5L2T/fontsize/400/fil
l/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

归纳一些比较高级的算法为三类： 

1.宽度优先搜索最短路径 

2.走哈密顿回路 

3.特殊决策

这里我主要介绍最简单的贪婪算法及整个游戏的设计。

首先是自动寻路函数whereGoNext(void)。决策思想是走曼哈顿距离fabs(snakeX[0]-foody)+fabs(snakeY[0]-foodx)最小且不会死掉的方向。当然，这很不智能，容易死掉，但容易实现。

    char whereGoNext() {
        char movable[4]= {'D','A','S','W'};
        int min=9999,minDir;
        for(int i=0; i<4; i++) {
            snakeX[0]+=direx[i];
            snakeY[0]+=direy[i];
    
           if(!gameover()) {
                int cost=fabs(snakeX[0]-foody)+fabs(snakeY[0]-foodx);
                if(min>cost) {
                    min=cost;
                    minDir=i;
                }
            }
    
            snakeX[0]-=direx[i];
            snakeY[0]-=direy[i];
        }
        if(min<9999)
            return movable[minDir];
        else return 0;
    }
接着是如何制作平滑的输出界面。我的实现方法是：

    每走一步
       掩盖蛇尾
       输出蛇头
       输出蛇颈
完整的代码如下：
  

      #include<stdio.h>
        #include<stdlib.h>
        #include<math.h>
        #include<time.h>
        #include<unistd.h>
        
        #define SNAKE_MAX_LENGTH 150
        #define SNAKE_HEAD 'H'
        #define SNAKE_BODY 'X'
        #define BLANK_CELL ' '
        #define SNAKE_FOOD '$'
        #define WALL_CELL '*'
        #define MAP_LENGTH 12
        #define DELAY_TIME  100000  //delay 100ms
        
        //move snake
        void snakeMove(int dir);
        //output cells of the grid
        void show(void);
        //output snake
        void output(void);
        //put a food randomized on a blank cell,1 for put food successfully,0 for fail
        int putFood(void);
        //judge whether gameover;0 for not over,1 for game over
        int gameover(void);
        //decide the smart snake's direction
        char whereGoNext(void);
        
        char map[MAP_LENGTH][MAP_LENGTH+1]= {//initial status
            "************",
            "*XXXXH     *",
            "*      *   *",
            "*      *   *",
            "*     **   *",
            "*      *   *",
            "*          *",
            "*          *",
            "*          *",
            "*          *",
            "*          *",
            "************"
        };
        const int direx[4]= {0,0,1,-1};
        const int direy[4]= {1,-1,0,0};
        int snakeX[SNAKE_MAX_LENGTH]= {5,4,3,2,1};
        int snakeY[SNAKE_MAX_LENGTH]= {1,1,1,1,1};
        int snakeLength=5;
        int foodx,foody,eatFood=1;
        
        int main(void) {
            char s;
            srand(time(NULL));
            printf("\033[2J");//clear the screen
            printf("\033[1;1H");
            show();
            do {
                s=whereGoNext();
                if(s==0)break;
                switch(s) {
                    case 'D':
                        snakeMove(0);
                        break;
                    case 'A':
                        snakeMove(1);
                        break;
                    case 'S':
                        snakeMove(2);
                        break;
                    case 'W':
                        snakeMove(3);
                        break;
                }
                output();
                usleep(DELAY_TIME);
            } while(!gameover());
            printf("\033[13;1HGame over!!!\n");
            return 0;
        }
        
        void snakeMove(int dir) {
            //first clear the tail
            printf("\033[%d;%dH%c\n",snakeY[snakeLength-1]+1,snakeX[snakeLength-1]+1,BLANK_CELL);
            map[snakeY[snakeLength-1]][snakeX[snakeLength-1]]=BLANK_CELL;
        
            if(snakeX[0]+direx[dir]==foody&&snakeY[0]+direy[dir]==foodx) {//eat food
                eatFood=1;
                snakeLength++;//increase length
            }
        
            for(int i=snakeLength-1; i>0; i--) {//body move
                snakeX[i]=snakeX[i-1];
                snakeY[i]=snakeY[i-1];
            }
        
            snakeX[0]+=direx[dir];//head move
            snakeY[0]+=direy[dir];
        }
        
        void show() {
            for(int i=0; i<MAP_LENGTH; i++) {//output map
                for(int j=0; j<MAP_LENGTH; j++) {
                    printf("%c",map[i][j]);
                }
                printf("\n");
            }
        }
        
        void output(void) {
            printf("\033[%d;%dH%c\n",snakeY[0]+1,snakeX[0]+1,SNAKE_HEAD);//print snake
            printf("\033[%d;%dH%c\n",snakeY[1]+1,snakeX[1]+1,SNAKE_BODY);
            printf("\033[%d;%dH%c\n",snakeY[snakeLength-1]+1,snakeX[snakeLength-1]+1,SNAKE_BODY);
            //must empty buffers: printf+'\n' or fflush(stdout);
        
            map[snakeY[0]][snakeX[0]]=SNAKE_HEAD;
            map[snakeY[snakeLength-1]][snakeX[snakeLength-1]]=SNAKE_BODY;
        
            if(eatFood) {
                eatFood=0;
                if(putFood()) { //put food
                    printf("\033[%d;%dH%c\n",foodx+1,foody+1,SNAKE_FOOD);
                }
            } else {
                printf("\033[%d;%dH%c\n",foodx+1,foody+1,SNAKE_FOOD);
            }
            printf("\033[13;1H\n");
        }
        
        int gameover(void) {
            if(snakeLength>=(MAP_LENGTH-2)*(MAP_LENGTH-2))return 1;//the map is full
        
            if(map[snakeY[0]][snakeX[0]]==WALL_CELL)return 1;//not touch wall
        
            for(int j=1; j<snakeLength; j++) {//doesn't eat itself
                if(snakeX[0]==snakeX[j]&&snakeY[0]==snakeY[j])return 1;
            }
            return 0;
        }
        
        int putFood(void) {
            int foodMap[MAP_LENGTH*MAP_LENGTH][2],len,ran;
            len=0;
            for(int i=1; i<MAP_LENGTH-1; i++) {//put empty place into foodMap
                for(int j=1; j<MAP_LENGTH-1; j++) {
                    if(map[i][j]==BLANK_CELL) {
                        foodMap[len][0]=i;
                        foodMap[len][1]=j;
                        len++;
                    }
                }
            }
        
            if(len>1)ran=rand()%len;//place food randomly
            else if(len==0)return 0;
            else ran=0;
        
            foodx=foodMap[ran][0];
            foody=foodMap[ran][1];
            return 1;
        }
        
        char whereGoNext() {
            char movable[4]= {'D','A','S','W'};
            int min=9999,minDir;
            for(int i=0; i<4; i++) {
                snakeX[0]+=direx[i];
                snakeY[0]+=direy[i];
        
                if(!gameover()) {
                    int cost=fabs(snakeX[0]-foody)+fabs(snakeY[0]-foodx);
                    if(min>cost) {
                        min=cost;
                        minDir=i;
                    }
                }
        
                snakeX[0]-=direx[i];
                snakeY[0]-=direy[i];
            }
            if(min<9999)
                return movable[minDir];
            else return 0;

            
![
](https://img-blog.csdn.net/20171226174725188?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlla2No/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)