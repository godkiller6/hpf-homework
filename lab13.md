1.snake_move.c


#include <stdio.h>

#include <stdlib.h>

#include <time.h> 

#include <conio.h>

#include <windows.h>

#define SNAKE_HEAD 'H'//表示蛇头的符号 

#define SNAKE_BODY 'X'//表示身体的符号 

#define SNAKE_AREA ' '//可以活动的区域 

#define WALL_AREA  '*'//表示墙的符号 

//第一层的函数 

void before_game();//游戏前要做的准备 

void play_game();//游戏时完成操作的函数 

void after_game();//游戏后跳出的界面 

//第二层的函数 

void make_map();//做出刚开始的地图 

void judge_ifGameBegin();//判断用户是否要开始游戏 

void output_map();//打印出地图 

void get_direction();//得到蛇下一次移动的方向 

void do_action();//改变蛇在数组中的位置 

void out_putchange();//输出下一个场景 

void judge_if_over();//判断是否撞墙 

void delete_output();//清除屏幕已有输出 

void output_GameOver();//输出游戏结束的结语 





const int height=12,length=12,snake_length=5;//地图大小以及蛇的长度 

char begin_map[height+1][length+1],position[height*length][2];//构建展示地图和存储蛇

的位置的二维数组 



int game_over=0,lastx,lasty;//判断是否游戏结束和 

char direction='d',last_direction='d';//存储前进方向 

char tempx,tempy;//存储位置 




int main()

{

	before_game();//游戏开始前要做的事情 

	play_game();//在玩家进行游戏时要做的事情 

	after_game();//游戏结束时要做的事情 



	return 0;

} 



void before_game(){

	make_map();//构建一个存放地图的二维数组 

	judge_ifGameBegin();//打印出确认开始界面，并等待开始 

	delete_output();//清除最初确认界面 

	output_map();//打印出游戏最初界面

	delete_output();//删除初始页面 

}

void play_game(){

	while(1){


		get_direction();//得到下一步的方向 

		do_action();//改变要输出的数组 

		out_putchange();//输出得到的数组 

		judge_if_over();//判断是否死亡 

		if(game_over==1)//是否结束游戏 

		break;

		output_map();//输出得到的地图 

		Sleep(500);//设置贪吃蛇速度 

		delete_output();//清屏 



	}



}

void after_game(){


	printf("\n\n\n\n\t\t\t\t\t游戏结束");

}

void make_map(){




	for(int i=0;i<height;i++)//构建区域 

	{

		for(int j=0;j<length;j++)

		{

			begin_map[i][j]=' ';

		}

	}

	for(int i=0;i<height;i++)//设置左右边界 

	{

		begin_map[i][0]='*';

		begin_map[i][length-1]='*';

		begin_map[i][length]='\0';

	}

	for(int i=1;i<length-1;i++)//设置上下边界 


	{

		begin_map[0][i]='*';

		begin_map[height-1][i]='*'; 

	}


	for(int i=1;i<snake_length;i++)//初始蛇身体 

	{

		begin_map[1][i]=SNAKE_BODY;

	}

	begin_map[1][snake_length]=SNAKE_HEAD;





	for(int i=0;i<snake_length;i++)//存储蛇的位置 

	{


		position[i][0]=1;

		position[i][1]=5-i;

	}





} 

void judge_ifGameBegin()

{

	char c;

	printf("\t\t\t\t欢迎来到古老的贪吃蛇小游戏\n\n\n\n\t\t\t\t按回车键以开始游戏\n");

	while(1)//输入回车开始游戏 

	{

		if((c=getchar())=='\n')

		{

			break;

		}

	 } 

}



void output_map(){//输出二维数组 

	for(int i=0;i<height;i++)

	{

			printf("%s\n",begin_map[i]);

	}

	printf("\n");

}

void get_direction()

{

	lastx=position[snake_length-1][0];

	lasty=position[snake_length-1][1];

	begin_map[lastx][lasty]=' ';//将尾部转化为空格 



	for(int i=snake_length-1;i>0;i--)//蛇身体的每部分换为前一部分的位置 

		{

			position[i][0]=position[i-1][0];

			position[i][1]=position[i-1][1];	

		}

	if(_kbhit())//有输入则改变方向 

	{

		last_direction=direction;

		direction=getch();

	}

} 

void do_action(){//不是反方向就改变蛇头的位置 

		if(last_direction+direction!='w'+'s'&&last_direction+direction!='a'+'d')

		{

			switch(direction){

			case 'd' : position[0][1]++;break;

			case 'a' : position[0][1]--;break;

			case 'w' : position[0][0]--;break;

			case 's' : position[0][0]++;break;

		}

	}

}



void out_putchange(){//设置好存地图的二维数组 

		tempx=position[0][0];

		tempy=position[0][1];

		begin_map[tempx][tempy]='H';

		for(int i=1;i<snake_length;i++)

		{

			tempx=position[i][0];

			tempy=position[i][1];

			begin_map[tempx][tempy]='X';

		} 

}

void judge_if_over(){//判断是否撞到墙壁或者是自身 

	if(position[0][1]==0||position[0][1]==length-1||position[0][0]==0||position[0]
    
    [0]==height-1)
	

    
    {
	

    
    	game_over=1;
	
    }
	
    for(int i=1;i<snake_length;i++)
	
    {
	
    	if(position[i][0]==position[0][0]&&position[i][1]==position[0][1])
	
    	{
	
    		game_over=1;
	
    	}
	

    } 

}


void delete_output(){

	system("cls");

}

2.snake_eat.c

#include <stdio.h>

#include <stdlib.h>

#include <time.h> 

#include <conio.h>

#include <windows.h>



#define SNAKE_HEAD 'H'//表示蛇头的符号 

#define SNAKE_BODY 'X'//表示身体的符号 

#define SNAKE_AREA ' '//可以活动的区域 

#define WALL_AREA  '*'//表示墙的符号 

#define SNAKE_FOOD '$'//表示食物的符号 



//第一层的函数 

void before_game();//游戏前要做的准备 

void play_game();//游戏时完成操作的函数 

void after_game();//游戏后跳出的界面 



//第二层的函数 

void make_map();//做出刚开始的地图 

void judge_ifGameBegin();//判断用户是否要开始游戏 

void output_map();//打印出地图 

void get_direction();//得到蛇下一次移动的方向 

void do_action();//改变蛇在数组中的位置 

void out_putchange();//输出下一个场景 

void judge_if_over();//判断是否撞墙 

void delete_output();//清除屏幕已有输出 

void output_GameOver();//输出游戏结束的结语 

void put_food();//随机存放食物 

void judge_if_eatfood();//判断是否吃到食物 





const int height=12,length=12;//地图的区域 

char begin_map[height+1][length+1],position[height*length][2];//存地图的数组和存蛇的

位置的数组 



int game_over=0,lastx,lasty,if_eatfood=0,foodx=0,foody=0;//判断是否死亡，蛇尾的坐标，

食物的坐标 

char direction='d',last_direction='d';//蛇运动的方向 

char tempx,tempy,snake_length=5;//临时坐标与蛇的长度 


int main()

{

	before_game();//游戏开始前要做的事情 

	play_game();//在玩家进行游戏时要做的事情 

	after_game();//游戏结束时要做的事情 



	return 0;


} 



void before_game(){

	make_map();//构建一个存放地图的二维数组 


	judge_ifGameBegin();//打印出确认开始界面，并等待开始 

	delete_output();//清除最初确认界面 

	put_food();//放置食物 

	output_map();//打印出游戏最初界面

	delete_output();//删除初始页面 

}

void play_game(){

	while(1){

		if(if_eatfood==1)//食物被吃再放食物 

		put_food(); 

		get_direction(); //得到运动方向 

		do_action();// 改变蛇头的位置 

		judge_if_eatfood();// 修改存放蛇的位置的数组 

		out_putchange();// 修改存放地图的数组 

		judge_if_over();// 判断是否死亡 

		if(game_over==1)// 死亡 就退出游戏 

		break;


		output_map();// 输出地图 

		Sleep(500);//设置蛇的速度 

		delete_output();



	}



}

void after_game(){

	printf("\n\n\n\n\t\t\t\t\t游戏失败");

}

void make_map(){



	for(int i=0;i<height;i++)//构建区域 

	{

		for(int j=0;j<length;j++)

		{

			begin_map[i][j]=' ';

		}

	}

	for(int i=0;i<height;i++)//构建左右边界 

	{

		begin_map[i][0]='*';

		begin_map[i][length-1]='*';

		begin_map[i][length]='\0';

	}

	for(int i=1;i<length-1;i++)//构建上下边界 

	{

		begin_map[0][i]='*';

		begin_map[height-1][i]='*'; 

	}

	for(int i=1;i<snake_length;i++)//构建蛇的位置 

	{

		begin_map[1][i]=SNAKE_BODY;

	}

	begin_map[1][snake_length]=SNAKE_HEAD;





	for(int i=0;i<snake_length;i++)//蛇位置初始化 

	{

		position[i][0]=1;

		position[i][1]=5-i;

	}





} 

void judge_ifGameBegin()

{

	char c;

	printf("\t\t\t\t欢迎来到古老的贪吃蛇小游戏\n\n\n\n\t\t\t\t按回车键以开始游戏\n");

	while(1)//输入回车开始游戏 

	{

		if((c=getchar())=='\n')

		{

			break;

		}

	 } 

}



void output_map(){//输出地图 

	for(int i=0;i<height;i++)

	{

			printf("%s\n",begin_map[i]);

	}

	printf("\n");

}

void get_direction()// 设置下一个画面蛇的位置 

{

	lastx=position[snake_length-1][0];

	lasty=position[snake_length-1][1];

	if(if_eatfood==0)//没吃食物蛇长不变 

	{

		begin_map[lastx][lasty]=' ';

	}

	else//吃到食物蛇加长 

	{

		snake_length++;

		if_eatfood=0;

	}


	for(int i=snake_length-1;i>0;i--)//蛇身体的每部分换为前一部分的位置 

		{

			position[i][0]=position[i-1][0];

			position[i][1]=position[i-1][1];	

		}

	if(_kbhit())//有输入则改变方向 

	{

		last_direction=direction;

		direction=getch();

	}

} 

void do_action(){//不是反方向就改变蛇头的位置 

		if(last_direction+direction!='w'+'s'&&last_direction+direction!='a'+'d')

		{

			switch(direction){

			case 'd' : position[0][1]++;break;

			case 'a' : position[0][1]--;break;

			case 'w' : position[0][0]--;break;

			case 's' : position[0][0]++;break;

		}

	}

}



void out_putchange(){//设置好存地图的二维数组

		tempx=position[0][0];

		tempy=position[0][1];

		begin_map[tempx][tempy]='H';

		for(int i=1;i<snake_length;i++)

		{

			tempx=position[i][0];

			tempy=position[i][1];

			begin_map[tempx][tempy]='X';

		} 

}

void judge_if_over(){//判断是否撞到墙壁或者是自身 

	if(position[0][1]==0||position[0][1]==length-1||position[0][0]==0||position[0]
    
    [0]==height-1)
	
    {
	
    	game_over=1;
	
    }
	
    for(int i=1;i<snake_length;i++)
	
    {
	
    	if(position[i][0]==position[0][0]&&position[i][1]==position[0][1])
	
    	{
	
    		game_over=1;
	
    	}
	

    } 

}



void delete_output(){

	system("cls");

}

void put_food(){//加入食物 



    srand(time(0)); 



    int i,j;



    while (1) { // 一直循环随机出可行坐标 




        i=rand() % height;j=rand() % length;    



        if (begin_map[i][j]==' ') {    



            begin_map[i][j]=SNAKE_FOOD; 

			foodx=i;

			foody=j;  

            return;     

        }  

    } 

}

void judge_if_eatfood(){//判断是否吃到食物 



	if(foodx==position[0][0]&&foody==position[0][1]) 

	{

		if_eatfood=1;

	}

} 
