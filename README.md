#include <iostream>
#include <conio.h>
#include <stdlib.h>
#include <Windows.h>
#include <ctime>
using namespace std;
bool gameOver=false;
enum storonu {STOP = 0, LEFT, RIGHT, UP, DOWN};
storonu dir;
short int x, y, fruitx, fruity, z, score=0,speed=1;
const int visota = 20;
const int shirota = 20;
int tailx[100], taily[100], ntail;
void setup()
{
	srand(time(0));
	x=shirota/2-1;
	y=visota/2-1;
	fruitx=rand()%shirota;
	fruity=rand()%visota;
	dir = STOP;
}
void draw()
{
	system("cls");
	for(int i=0;i<=shirota+1;i++)
	{
		cout<<"#";
	}
	cout<<endl;
	for(int i=0;i<=visota;i++)
	{
		for(int j=0;j<visota;j++)
		{
			
			if(j==0)
			{
				cout<<"#";
			}
			if(fruitx==j&&fruity==i)
			{
				cout<<"F";
			}
				else if(x==j&&y==i)
			{
				cout<<"0";
			}
			else 
			{
				bool risov=false;
				for(int n=0;n<ntail;n++)
				{
					if(tailx[n]==j&&taily[n]==i)
					{
						risov=true;
						cout<<"o";
					}
				}
				if(!risov)
				{
					cout<<" ";
				}
			}
			if(j==19)
			{
				cout<<"#"<<endl;
			}
		}
	}
		for(int i=0;i<=shirota+1;i++)
	{
		cout<<"#";
	}
	if(fruitx==x&&fruity==y)
	{
		cout<<"\nAMMM";
		Beep(2500, 75);
		Beep(1000, 75);
		Beep(5000, 75);
		fruitx=rand()%(shirota);
		fruity=rand()%(visota);
		ntail++;
		score+=10;
		if(score%50==0)
			speed+=5;
	}
	cout<<"\nScore = "<<score;
}
void input()
{
	if(_kbhit ())
	{
		switch(getch())
		{
			case 'a':
				dir=LEFT;
				break;
			case 'd':
				dir=RIGHT;
				break;
			case 'w':
				dir=UP;
				break;
			case 's':
				dir=DOWN;
				break;	
			case 'g':
				gameOver=true;
				break;		
		}
	}
}
void logik()
{
	int predx=tailx[0];
	int predy=taily[0];
	int pred2x, pred2y;
	tailx[0]=x;
	taily[0]=y;
	for(int i=1;i<ntail;i++)
	{
		pred2x=tailx[i];
		pred2y=taily[i];
		tailx[i]=predx;
		taily[i]=predy;
		predx=pred2x;
		predy=pred2y;
	}
	switch(dir)
	{
		case LEFT:
			x=(x+19)%20;
		break;
		case RIGHT:
			x=(x+1)%20;
		break;
		case UP:
			y=(y+21)%22;
		break;
		case DOWN:
			y=(y+1)%22;
		break;
	}
	for(int i=0;i<ntail;i++)
	{
		if(tailx[i]==x&&taily[i]==y)
		{
			gameOver=true;
		}
	}
}
int main()
{
	setup();
	while(!gameOver)
	{
		draw();
		input();
		Sleep(100/speed);
		logik();
		
	}
}
