#pragma once
#include<graphics.h>
#include<conio.h>
#include<string>
#include<iostream>
#include<fstream>
using namespace std;

void draw_interface();
void reaction(int* num);//num用来计数，防止点击了界面上的图标后依然可以进行点击原有图标位置的操作
void drawtriangle(int x,int y);
void drawinstruction();
void back(int x1,int x2,int y1,int y2);//返回标志左上角和右下角的坐标








