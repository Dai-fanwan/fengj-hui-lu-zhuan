#include"Record.h"


void put_record_in(int record)
{
	char rec_txt[10] = { 0 };
	sprintf(rec_txt, "%d", record);
	ofstream rec;
	rec.open("record.txt", ios::out);
	rec << rec_txt;
	rec.close();
	return;
}

int put_record_out()
{
	int record=0;
	ifstream rec;
	rec.open("record.txt", ios::in);
	char rec_txt[10] = { 0 };
	int i = 0;
	while (!rec.eof())
	{
		rec >> rec_txt[i];
		i++;
	}
	rec.close();
	int num = i-1;//得到该数的位数（由于多了一位/0，所以要减1）
	for (i = 0; rec_txt[i] != 0; i++)
	{
		record +=(rec_txt[i]-48) * pow(10, num - 1 - i);//注意(int)'1'=49,要化成数字1需要减48
	}
	return record;
}

void showrecord()
{
	cleardevice();
	settextcolor(WHITE);
	settextstyle(20, 20, "宋体");
	RECT hint = {0,100,800,150 };
	drawtext("您目前在无尽模式中通过的最高关卡数为：", &hint, DT_CENTER);
	settextcolor(YELLOW);
	settextstyle(50, 50,"宋体");
	RECT record = { 300,300,600,450 };
	int rec = put_record_out();
	char rec_txt[10];
	sprintf(rec_txt, "%d", rec);//把数化成字符串
	drawtext(rec_txt, &record, DT_CENTER);
	RECT back = { 300,500,600,600 };
	settextcolor(BROWN);
	settextstyle(30,30, "宋体");
	drawtext("返回", &back, DT_CENTER);
	return;
}











