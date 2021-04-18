#include "StartInterface.h"
#include "playgame.h"
#include "Record.h"

//在各种页面返回主页面,但playgame页面有点特殊，如果用该函数返回主页面，没有点击返回的状态下移开鼠标，其他区域的操作不会再生效
void back(int x1,int x2,int y1,int y2)
{
	while (1)
	{
		MOUSEMSG msg = GetMouseMsg();
		if (msg.x > x1 && msg.x < x2 && msg.y>y1 && msg.y < y2)
		{
			drawtriangle(x1-50, y1-15);
			if (msg.mkLButton == true)
			{
				cleardevice();
				draw_interface();
				int num = 0;
				while (num == 0)
				{
					reaction(&num);
				}
				break;
			}
		}
		else
		{
			HRGN clip = CreateRectRgn(x1-50, y1-15, x1, y1+35);
			setcliprgn(clip);
			DeleteObject(clip);
			clearcliprgn();
			setcliprgn(NULL);//注意这里要取消之前设置的裁剪区，否则后续内容无法显示（只能在裁剪区内作图）
		}
	}
}

void drawtriangle(int x,int y)//当鼠标停留在选项上时，在选项的前面添加三角形
{
	setlinecolor(WHITE);
	line(x, y, x + 50, y + 25);
	line(x, y, x, y + 50);
	line(x, y + 50, x + 50, y + 25);
	floodfill(x + 10, y + 25, WHITE);
	return;
}

void draw_interface()//展示初始界面
{
	BeginBatchDraw();

	settextcolor(YELLOW);
	RECT title = RECT{ 200, 100, 700, 200 };//为什么这里不用小括号啊？不是调用构造函数吗
	settextstyle(50, 50,"黑体");
	drawtext("峰回路转",&title , DT_CENTER);
	RECT startgame = RECT{ 300,350,600,400 };
	settextstyle(25, 25, "宋体");
	drawtext("开始游戏", &startgame, DT_CENTER);
	RECT instruction = RECT{ 300,450,600,500 };
	drawtext("查看说明", &instruction, DT_CENTER);
	RECT record = RECT{ 300,550,600,600 };
	drawtext("我的记录", &record, DT_CENTER);
	

	EndBatchDraw();
	return;
}

//画出说明页面。原本是为了方便移开鼠标时重置，但是发现使用这个函数时会造成画面频闪（或许是没有使用batch?)，所以直接裁剪了
void drawinstruction()
{
	IMAGE instruction;
	loadimage(&instruction, "instruction.png");
	putimage(0, 0, &instruction);
	RECT confirm = RECT{ 200,600,600,700 };
	settextstyle(30, 30, "宋体");
	drawtext("确定", &confirm, DT_CENTER);
	return;
}

void reaction(int* num)
{
	MOUSEMSG msg = GetMouseMsg();
	if (msg.x < 600 && msg.x>300 && msg.y > 350 && msg.y < 400)
	{
		drawtriangle(250,335);
		if (msg.mkLButton == true)
		{
			(*num)++;
			playgame();
		}
	}
	else if (msg.x < 600 && msg.x>300 && msg.y > 450 && msg.y < 500)
	{
		drawtriangle(250, 435);
		if (msg.mkLButton == true)
		{
			(*num)++;//注意此处不能写*num++
			cleardevice();
			//RECT ins_rect = RECT{ 100,100,800,700 };//文本框
			//ifstream ifs;
			//ifs.open("textfile.txt", ios::in);
			//char instruction[2000] = { 0 };
			//int i = 0;
			//while (!ifs.eof())
			//{
			//	ifs >> instruction[i];
			//	i++;
			//}
			//我实在是没办法作文件输入了……查了一个下午也不知道char*到LPCTSTR怎么转，怎么转都是乱码╮(╯﹏╰）╭
			//原来使用多字节字符模式就可以不用把char*转LPCTSTR了，但图片都存了那就算了吧……(〃'▽'〃)
			drawinstruction();
			back(300, 600, 600, 700);
		}
	}
	else if (msg.x < 600 && msg.x>300 && msg.y > 550 && msg.y < 600)
	{
		drawtriangle(250, 535);
		if (msg.mkLButton == true)
		{
			(*num)++;
			showrecord();
			back(350, 550, 500, 550);
		}
	}
	else
	{
		//当鼠标从可以点击的图标移到其他位置时，要清除之前画的三角形（这里不用裁剪也不会频闪呢……）
		cleardevice();
		draw_interface();
	}

	return;
}
