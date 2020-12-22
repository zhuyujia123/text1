#include<stdio.h>
#include<windows.h>
#include<conio.h>
#include<stdlib.h>
//#define N 16

char qipan[16][16];//全局变量，整个文件的都可以用
int x,y;//代表的是棋盘（数组)下标为x的行 和  下标为y列的一个交点
//函数声明
void initQipan();//棋盘的初始化
void printQipan();//打印棋盘
void Pos(int x, int y);//设置光标位置
void startGame();//游戏开始
int panduan(int x,int y);//判断是否有人连成五子
int showWhoWin();//输出谁赢了
void whitePlay();//白方下子
void blackPlay();//黑方下子
void printShuzi();//打印数字模板



void Pos(int x, int y)//设置光标位置，从哪里开始输出
{
    COORD pos;//表示一个字符在控制台屏幕上的坐标，左上角(0,0)
    HANDLE hOutput;
    pos.X = x;
    pos.Y = y;
    hOutput = GetStdHandle(STD_OUTPUT_HANDLE);//返回标准的输入、输出或错误的设备的句柄，也就是获得输入、输出/错误的屏幕缓冲区的句柄
    SetConsoleCursorPosition(hOutput, pos);
}

void printShuzi()
{
    int i;
    Pos(2,0);
    for(i=0;i<16;i++)
        printf("%2d",i);
    for(i=0;i<16;i++)
    {
        Pos(0,1+i);
        printf("%2d",i);
    }
}
void initQipan()
{
    int i,j;
    for(i=0;i<16;i++)
        for(j=0;j<16;j++)
            qipan[i][j]='*';
}
void printQipan()
{
    int i,j;
    printShuzi();
    for(i=0;i<16;i++)
    {
        Pos(2,1+i);//自动换行的输出的功能，代替printf("\n");
        for(j=0;j<16;j++)
            printf(" %c",qipan[i][j]);
    }
}

int  panduan(int x,int y)
{
    char temp;//保存下棋方的颜色,w  b
    int count=1;//统计个数,针对的同一个线（水平线）
    int i=1;//走一格
    int j=1;//和i同时用来代表走斜的
    int whoWin=0;//1代表白方，2代表黑方
    temp=qipan[x][y];
    //水平的左边
    while(temp==qipan[x][y-i]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }
    //水平的右边
    i=1;
   while(temp==qipan[x][y+i]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }

   //解决垂直的方向
   //垂直上方
   i=1;
   count=1;//清理掉之前的数据
   while(temp==qipan[x-i][y]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }
   //垂直下方
   i=1;
   while(temp==qipan[x+i][y]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }
   //解决左上的斜线  上方
     i=1;
     j=1;
     count=1;
   while(temp==qipan[x-i][y-j]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        j++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }
   //解决左上的斜线  下方
     i=1;
     j=1;
   while(temp==qipan[x+i][y+j]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        j++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }
   //解决右上的斜线  上方
     i=1;
     j=1;
     count=1;
   while(temp==qipan[x-i][y+j]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        j++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }
   //解决右上的斜线  下方
     i=1;
     j=1;
   while(temp==qipan[x+i][y-j]&&x>=0&&x<16&&y>=0&&y<15&&count<5)
    {
        i++;
        j++;
        count++;
        if(count==5)//首先count是5才能来判断谁赢了，不然连资格都没有
        {
            if(temp=='W')
                whoWin=1;

            else
                whoWin=2;
        }
    }
    return whoWin;
}
int showWhoWin()
{
    int overLeap=0;//1代表结束
    int leap;//用来接收谁赢了
    leap=panduan(x,y);
    if(leap==1)
    {
        overLeap=1;
        system("cls");
        printQipan();
        printf("\n白方胜利\n");
        system("pause");
    }
    if(leap==2)
    {
        overLeap=1;
        system("cls");
        printQipan();
        printf("\n黑方胜利\n");
        system("pause");
    }
    return overLeap;
}

void whitePlay()
{
    printf("\n请白方落子，按下行与列的坐标:");
    scanf("%d%d",&x,&y);//坐标的值
    while(1)//解决一直下错子的问题
    {
        if(qipan[x][y]=='*')//下子的地方没有其他子
        {
             qipan[x][y]='W';
             //解决while(1)
             break;
        }
        else
        {
            printf("您下子错误\n");
            printf("请白方落子，按下行与列的坐标:");
            scanf("%d%d",&x,&y);//坐标的值
        }

    }
    printQipan();   

}
void blackPlay()
{

    printQipan();
    printf("\n请黑方落子，按下行与列的坐标:");
    scanf("%d%d",&x,&y);//坐标的值
        while(1)//解决一直下错子的问题
    {
        if(qipan[x][y]=='*')//下子的地方没有其他子
        {
             qipan[x][y]='B';
             //解决while(1)
             break;
        }
        else
        {
            printf("您下子错误\n");
            printf("请黑方落子，按下行与列的坐标:");
        }
        scanf("%d%d",&x,&y);//坐标的值

    }
    printQipan();

}
void startGame()
{
    initQipan();
    printQipan();
    while(1)
    {
        whitePlay();
        if(showWhoWin()==1)//system("pause");
             break;
        system("cls");//清理屏幕,是屏幕上的字不重复
        blackPlay();
        if(showWhoWin()==1)//system("pause");
             break;
        system("cls");
        printQipan();
    }
    printf("您是否重新游戏:y  or n");
    if(getch()=='n')
    {
        system("cls");
        printf("游戏结束\n");
        exit(0);//因为程序终止
    }
    if(getch()=='y')
    {
        system("cls");
        startGame();
    }

}
int main()
{
    startGame();
    return 0;
}
