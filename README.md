#include"StartInterface.h"
#include"Rect.h"

int main()
{
	initgraph(900, 750, EW_DBLCLKS);
	draw_interface();
	int num = 0;
	while (num==0)
	{
		reaction(&num);
	}
	_getch();
	return 0;
}
