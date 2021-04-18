#include"Rect.h"

void Rect::draw_rect_dots()
{
	setfillcolor(WHITE);
	int i;
	int j;
	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 10; j++)
		{
			solidcircle(150 + i * 66, 50 + j * 66, 3);
		}
	}
	return;
}

void Rect::draw_num()
{

}

void play_rect()
{
	RECT number = RECT{ 200,300,700,500 };
	settextcolor(YELLOW);
	drawtext("第一关", &number,DT_CENTER);
	Sleep(750);
	cleardevice();
	static Rect rect1;//先建立一个实体，不能直接用类名使用类中的函数
	rect1.set_answer_dots("rect_level1.txt");
	rect1.draw_rect_dots();
	draw_back();
	rect_select(rect1);
	return;
}

void draw_back()
{
	settextcolor(YELLOW);
	RECT back = { 350,700,450,750 };
	drawtext("返回", &back, DT_CENTER);
	return;
}

void back_in_rect_interface(int x1,int x2,int y1,int y2,Rect& rect)//这里设置的参数顺序跟RECT参数顺序不一样……自己挖坑……
{
	while (1)
	{
		MOUSEMSG msg = GetMouseMsg();
		if (msg.x > x1 && msg.x < x2 && msg.y>y1 && msg.y < y2)
		{
			drawtriangle(x1 - 50, y1 - 15);
			if (msg.mkLButton == true)
			{
				cleardevice();
				draw_select_interface();
				int num = 0;
				while (num == 0)
				{
					react_in_select_interface(&num);
				}
				break;
			}
		}
		else
		{
			HRGN clip = CreateRectRgn(x1 - 50, y1 - 15, x1, y1 + 35);
			setcliprgn(clip);
			DeleteObject(clip);
			clearcliprgn();
			setcliprgn(NULL);//注意这里要取消之前设置的裁剪区，否则后续内容无法显示（只能在裁剪区内作图）
			rect_select(rect);
			//这个函数是为了当鼠标从“返回”移到点阵时，不会造成点阵中的点无法选中，跟back_in_playgame(&num)的最后一行作用相似
		}
	}
	return;
}

void Rect::set_dot(int x,int y)
{
	if (getpixel(x + 5, y + 5) == WHITE)//偏移
		dot[(x - 150) / 66][(y - 50) / 66] = -1;
	if (getpixel(x, y) == YELLOW)
		dot[(x - 150) / 66][(y - 50) / 66] = 1;
	return;
}

void Rect::set_answer_dots(string str)
{
	ifstream level;
	level.open(str);
	if (!level)
	{
		cerr << "error:can't find the file." << endl;
		exit(1);
	}
	int i, j;
	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 10; j++)
		{
			level >> answer_dots[i][j];
		}
	}
}











