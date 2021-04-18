#pragma once
#include "StartInterface.h"
#include"windows.h"
#include"Operate.h"
#include"playgame.h"
#include<fstream>
//四边形模式

class Rect
{

	int dot[10][10];
	int answer_dots[10][10];

public:
	void set_answer_dots(string);
	void draw_rect_dots();//画矩形点阵
	void draw_num();//写出矩形中的数字
	void set_dot(int ,int);//对rect中的点阵进行操作，标记点
};

void play_rect();
void back_in_rect_interface(int,int,int,int,Rect&);//声明里不写变量名也可以
void draw_back();






