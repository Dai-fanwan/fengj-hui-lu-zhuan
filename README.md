#include "Operate.h"
#include "Rect.h"

template<typename T>
void pitch(int x, int y,MOUSEMSG msg,T shape)
{
	if (getpixel(x,y) != YELLOW)//判断这个点是否已经被标记了，如果不判断，那么只有长按该点的时候才会显示黄色（被刷掉了）
	{
		//把原点改成方形以告诉玩家自己的鼠标停留在该点上
		HRGN rgn = CreateRectRgn(x - 7, y - 7, x + 7, y + 7);
		setcliprgn(rgn);
		DeleteObject(rgn);
		setfillcolor(WHITE);
		solidrectangle(x - 6, y - 6, x + 6, y + 6);
		setcliprgn(NULL);
		if (msg.mkLButton == true)
		{
			setfillcolor(YELLOW);
			solidrectangle(x - 6, y - 6, x + 6, y + 6);
			shape.set_dot(x, y);
		}
	}
	return;
}




//当鼠标从停留的点上移开时，清除该点的矩形
//此处如果使用图像处理识别白色的矩形，还是比较难的，那就交给添加了标记的数组解决
void cancel(int arr[10][10])
{
	int i, j;
	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 10; j++)
		{
			if (arr[i][j] == -1)
			{
				HRGN rgn = CreateRectRgn(150 + i * 66 - 6, 50 + j * 66 - 6, 150 + i * 66 + 6, 50 + j * 66 + 6);
				setcliprgn(rgn);
				DeleteObject(rgn);
				clearcliprgn();
				setcliprgn(NULL);
				setfillcolor(WHITE);
				solidcircle(150 + i * 66, 50 + j * 66, 3);
			}
		}
	}
	return;
}

//存在的问题：一旦有四个点挨在一起，一定会形成4条线段，但游戏中不一定要练成4条，可能只有2条或三条
//emm这个时候就交给delete解决吧，如果用户在间隔很多点后点击两个相邻的点之后反而连不上了
void link()
{
	int i, j;
	int x[101] = { 0 };
	int y[101] = { 0 };
	int num = 0;
	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 10; j++)
		{
			COLORREF judge=getpixel(150 + i * 66, 50 + j * 66);
			if (judge == YELLOW)
			{
				x[num] = 150 + i * 66;
				y[num] = 50 + j * 66;
				num++;
			}
		}
	}
	for (i = 0; i < num-1; i++)
	{
		for (j = i + 1; j < num; j++)
		{
			if ((x[i] == x[j] && abs(y[i] - y[j]) == 66) || (y[i] == y[j] && abs(x[i] - x[j]) == 66))
			{
				line(x[i]-1, y[i]-1, x[j]-1, y[j]-1);
				//这里对连线的起点和终点稍作偏移，以为如果刚好连在中心，中心的颜色会被白色覆盖，不会触发pitch()，自然不会触发pitch()中的link()
				//还有一种解决方法，是将pitch()中颜色的判断进行偏移（但也有可能出现取到的点仍然在白线的覆盖范围内），将pitch()中的link()转移到select_dots()中
			}
		}
	}
	return;
}

void rect_select(Rect& rect)
{
	while (1)
	{
		MOUSEMSG msg1 = GetMouseMsg();
		int i, j;
		int x, y;
		for (i = 0; i < 10; i++)
		{
			for (j = 0; j < 10; j++)
			{
				x = 150 + i * 66;
				y = 50 + j * 66;
				//设置成边长为6的矩形区域太小了，用户体验有点差，改成12了
				if ((msg1.x > x - 6) && (msg1.x < x + 6) && (msg1.y > y - 6) && (msg1.y < y + 6))
				{
					pitch<Rect&>(x, y, msg1, rect);
				}
				else if (msg1.y > 700)
				{
					back_in_rect_interface(350, 450, 700, 750, rect);
				}
				else
				{
					continue;
				}
			}
		}
		link();
	}
}









