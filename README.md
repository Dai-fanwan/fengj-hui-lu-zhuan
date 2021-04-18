#pragma once
#include<math.h>
#include "StartInterface.h"
#include"Rect.h"



void rect_select(Rect&);

template<typename T>
void pitch(int x, int y,MOUSEMSG msg,T shape);

void cancel(int [10][10]);

void link();//将选中的相邻两点相连


