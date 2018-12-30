过了漫长的一学期地学习，一直以来的C语言作业都给我们留下了C似乎只能做数学题这样的一种重要却无趣的功能，这不禁让人有些厌烦。然而，这两周的软件工程导论作业却令我们耳目一新，作业的要求是让我们用C语言完成一个简单的贪吃蛇游戏，并且在此基础上让其“智能”化，即能够自己寻找食物。在我的程序编写过程中，出现了很多令人啼笑皆非的bug，在令人捧腹大笑地同时，也增加了对C语言地兴趣。好了，话不多说，接下来我要介绍一下我的“智能”蛇成长过程。 

失败代码如下：

    #include<stdio.h>
    #include<conio.h>
    #include<stdlib.h>
    #include<time.h>
    #include<math.h>
    #define WALL_CELL *
    #define SNAKE_HEAD H
    #define SNAKE_BODY X
    #define SNAKE_FOOD $
    #define SNAKE_MAX_LENGTH 20
    int snake_index = 4;//当前蛇尾数组下标
    int fail = 0;//判断是否失败
    int snake_x[SNAKE_MAX_LENGTH] = {5,4,3,2,1};
    int snake_y[SNAKE_MAX_LENGTH] = {1,1,1,1,1};
    int have_food = 0;//判断是否输出食物
    int distance[4] = {0};
    char movable[4] = {'W','S','A','D'};
    int Fx = 0,Fy = 0;
    
    char map[12][12] =
    {
        "************",
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
        "************"
    };
    int getmin(int distance[],char n)
    {
        int i = 0,temp = distance[0],j=0;
        for(i = 0;i<4;i++)
        {
            if(temp > distance[i] && movable[i] != n) {
                temp = distance[i];
                j = i;
            }
        }
        return j;
    }
    char getnot_move()
    {
        if( (map[snake_y[0]][snake_x[0] + 1] == 'x' && map[snake_y[0]][snake_x[0] + 1] == '*')) return 'D';
        else if( (map[snake_y[0]][snake_x[0] - 1] == 'x' &&  map[snake_y[0]][snake_x[0] - 1] == '*')) return 'A';
        else if( (map[snake_y[0]][snake_x[0] + 1] == 'x' && map[snake_y[0]][snake_x[0] + 1] == '*') ) return 'S';
        else if( (map[snake_y[0]][snake_x[0] - 1] == 'x' && map[snake_y[0]][snake_x[0] - 1] == '*') ) return 'W';
    
    }
    
    char wherego()
    {
        int i;
    
    
            distance[0] = abs(snake_y[0] -1 - Fy) + abs(snake_x[0] - Fx);
            distance[1] = abs(snake_y[0] +1 - Fy) + abs(snake_x[0] - Fx);
            distance[2] = abs(snake_y[0] - Fy) + abs(snake_x[0] -1 - Fx);
            distance[3] = abs(snake_y[0] - Fy) + abs(snake_x[0] +1 - Fx);
        char n = getnot_move();
        i = getmin(distance,n);
        return movable[i];
    
    }
    void movesnake(int sx , int sy)//利用贪吃蛇总是沿着之前的轨迹爬行
    {
        int i,tempx,tempy;
        map[snake_y[snake_index]][snake_x[snake_index]] = ' ';
        tempx = snake_x[snake_index];
        tempy = snake_y[snake_index];//记录新生成的尾巴的位置
        for(i = snake_index;i>0;i-- )
        {
            snake_x[i] = snake_x[i-1];
            snake_y[i] = snake_y[i-1];
            map[snake_y[i]][snake_x[i]] = 'X';
        }
        if(sx == 0){
            snake_y[0] = snake_y[0] + sy;
        }
        else snake_x[0] = snake_x[0] + sx;
        if(map[snake_y[0]][snake_x[0]] == '$') {
            have_food = 0;
            snake_index++;
            snake_x[snake_index] = tempx;
            snake_y[snake_index] = tempy;
            //map[snake_y[tempy]][tempx] = 'X';过多导致输出混乱
        }
        if(map[snake_y[0]][snake_x[0]] == '*' || map[snake_y[0]][snake_x[0]] == 'X') fail = 1;;
        map[snake_y[0]][snake_x[0]] = 'H';
    }
    
    void output()
    {
        int i ,j ;
        for( i = 0;i < 12;i++)
        {
            for(j = 0;j < 12;j++)
                printf("%c",map[i][j]);
            printf("\n");
        }
    }
    
    void put_money()
    {
        int i,j,k;
        if(!have_food)
        {
            srand((int)time(NULL));
           for(k = 0;k < 100 ;k++){
                i = rand() % 11 + 1;//使输出在框架之内
                j = rand() % 11 + 1;
                if(map[i][j] == ' ') {
                        map[i][j] = '$';
                        have_food = 1;
                        Fx = j;
                        Fy = i;
                        break;
                }
           }
    
        }
    }
    
    void gameover()
    {
        puts("gameover");
    }
    
    int main()
    {
        char ch；
        while(1)
        {
            output();
    
    
            while(1)
            {
                put_money();
                ch = wherego();
                if(ch == 'W') movesnake(0,-1);
                else if(ch == 'S') movesnake(0,1);
                else if(ch == 'A') movesnake(-1,0);
                else if(ch == 'D') movesnake(1,0);
                else break;
                if(fail){
                    gameover();
                    break;
                }
                output();
                if(snake_index == SNAKE_MAX_LENGTH - 1) {
                        puts("winning!");
                        break;
                }
            }
            break;
        }
    }

附上一些失败游戏图：

![
](https://img-blog.csdn.net/20171223204825632?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGFpUkc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![
](https://img-blog.csdn.net/20171223205216263?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGFpUkc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

成功代码：

    #include<stdio.h>
    #include<conio.h>
    #include<stdlib.h>
    #include<time.h>
    #include<math.h>
    #define WALL_CELL *
    #define SNAKE_HEAD H
    #define SNAKE_BODY X
    #define SNAKE_FOOD $
    #define SNAKE_MAX_LENGTH 20
    int snake_index = 4;//当前蛇尾数组下标
    int fail = 0;//判断是否失败
    int snake_x[SNAKE_MAX_LENGTH] = {5,4,3,2,1};
    int snake_y[SNAKE_MAX_LENGTH] = {1,1,1,1,1};
    int have_food = 0;//判断是否输出食物
    int distance[4] = {0};
    char movable[4] = {'W','S','A','D'};
    int Fx = 0,Fy = 0;
    
    char map[12][12] =
    {
        "************",
        "*XXXXH     *",
        "* ***  *   *",
        "* ***  *   *",
        "*          *",
        "*  ****    *",
        "*          *",
        "*  *  **   *",
        "*  *       *",
        "*  *   **  *",
        "*          *",
        "************"
    };
    int getmin(int distance[])
    {
        int i = 0,temp = distance[0],j=0;
        for(i = 0;i<4;i++)
        {
            if(temp > distance[i] ) {
                temp = distance[i];
                j = i;
            }
        }
        return j;
    }
    int getnot_move(int i)
    {
        int dx = 0,dy = 0;
        if(i == 0) dy = -1;
        else if(i == 1) dy = 1;
        else if(i == 2) dx = -1;
        else dx = 1;
        if(dy){
            if(map[snake_y[0] + dy][snake_x[0]] == 'X') return 0;
            else if(map[snake_y[0] + dy][snake_x[0]] == '*') return 0;
            else return 1;
        }
        else{
            if(map[snake_y[0]][snake_x[0] + dx] == '*') return 0;
            else if(map[snake_y[0]][snake_x[0] + dx] == 'X') return 0;
            else return 1;
        }
    
    
    }
    
    char wherego()
    {
        int i;
        int count= 4;
    
            distance[0] = abs(snake_y[0] -1 - Fy) + abs(snake_x[0] - Fx);
            distance[1] = abs(snake_y[0] +1 - Fy) + abs(snake_x[0] - Fx);
            distance[2] = abs(snake_y[0] - Fy) + abs(snake_x[0] -1 - Fx);
            distance[3] = abs(snake_y[0] - Fy) + abs(snake_x[0] +1 - Fx);
    
        while(count--){
            i = getmin(distance);
            if(!getnot_move(i)){
                distance[i] = 9999;
                i = getmin(distance);
            }
        }
        return movable[i];
    
    }
    void movesnake(int sx , int sy)//利用贪吃蛇总是沿着之前的轨迹爬行
    {
        int i,tempx,tempy;
        map[snake_y[snake_index]][snake_x[snake_index]] = ' ';
        tempx = snake_x[snake_index];
        tempy = snake_y[snake_index];//记录新生成的尾巴的位置
        for(i = snake_index;i>0;i-- )
        {
            snake_x[i] = snake_x[i-1];
            snake_y[i] = snake_y[i-1];
            map[snake_y[i]][snake_x[i]] = 'X';
        }
        if(sx == 0){
            snake_y[0] = snake_y[0] + sy;
        }
        else snake_x[0] = snake_x[0] + sx;
        if(map[snake_y[0]][snake_x[0]] == '$') {
            have_food = 0;
            snake_index++;
            snake_x[snake_index] = tempx;
            snake_y[snake_index] = tempy;
            //map[snake_y[tempy]][tempx] = 'X';过多导致输出混乱
        }
        if(map[snake_y[0]][snake_x[0]] == '*' || map[snake_y[0]][snake_x[0]] == 'X') fail = 1;
        map[snake_y[0]][snake_x[0]] = 'H';
    }
    
    void output()
    {
        int i ,j ;
        system("cls");
        for( i = 0;i < 12;i++)
        {
            for(j = 0;j < 12;j++)
                printf("%c",map[i][j]);
            printf("\n");
        }
    }
    
    void put_money()
    {
        int i,j,k;
        if(!have_food)
        {
            srand((int)time(NULL));
           for(k = 0;k < 1000 ;k++){
                i = rand() % 11 + 1;//使输出在框架之内
                j = rand() % 11 + 1;
                if(map[i][j] == ' ') {
                        map[i][j] = '$';
                        have_food = 1;
                        Fx = j;
                        Fy = i;
                        break;
                }
           }
    
        }
    }
    
    void gameover()
    {
        puts("gameover");
    }
    
    int main()
    {
        char ch;
        system("title 贪吃蛇——HaiRG");
        system("mode con cols=35 lines=25");
        system("color F0");
        //clock_t start,end;
        while(1)
        {
            output();
    
    
            while(1)
            {
                put_money();
                ch = wherego();
                start = time(NULL);
                end   = time(NULL);
                while(end - start < 4 / snake_index) end = time(NULL);
                if(ch == 'W') movesnake(0,-1);
                else if(ch == 'S') movesnake(0,1);
                else if(ch == 'A') movesnake(-1,0);
                else if(ch == 'D') movesnake(1,0);
                else break;
                if(fail){
                    gameover();
                    break;
                }
                output();
                if(snake_index == SNAKE_MAX_LENGTH - 1) {
                        puts("winning!");
                        break;
                }
            }
            break;
        }
    }
