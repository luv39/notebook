# ncurses教程

## 从 Hello World 开始

### Hello World程序

```c
#include<ncurses.h>           // ncurses库已经包含stdio.h

int main()
{
    initscr();                // 初始化,进入NCURSES模式
    printw("hello world");    // 在虚拟屏幕上打印Hello World
    refresh();                // 将虚拟屏幕上的内容写到显示器上, 并刷新
    getchar();                // 等待用户输入
    endwin();                 // 退出NCURSES模式

    return 0;
}
```

* 编译`gcc hello.c -lncurses`

## 初始化

### `raw()`函数和`cbreak()`函数

* 通常情况下, 终端驱动程序会缓冲用户输入的字符, 直到遇到换行符或回车符后, 这些字符才可以被使用. 但是大多数程序要求字符再输入时就可以被使用. `raw()`和`cbreak()`两个函数都可以禁止缓冲.
* `raw()`函数模式下, 处理挂起(Ctrl-Z),中断或退出(Ctrl-C)等控制字符时, 将直接传送给程序去处理而不产生终端信号
* `cbreak()`模式下, 控制字符将被终端驱动程序解释而挂起程序或退出程序.

### `echo()`函数和`noecho()`函数

* 调用`echo()`函数将键盘输入的字符显示在终端上
* 调用`noecho()`函数禁止输入的字符出现在屏幕上

### `keypad()`函数

* 这个函数允许使用功能键, 例如: F1, F2, 方向键等功能键. 使用`keypad(stdscr, TRUE)`就为"标准屏幕"激活了功能键.

### `halfdelay()`函数

* `halfdelay()`函数会启用半延时模式. 当程序需要用户输入字符时,输入的每个字符都是可用的. 给`halfdelay()`传递一个整型参数(以0.1秒为单位), 他就会在这个参数时间内等待输入. 如果没有有效的输入, 则返回ERR.

### 示例程序

```c
#include<ncurses.h>

int main()
{
    int ch;
    initscr();                  //开始curses模式
    raw();                      //禁用行缓冲
    keypad(stdscr, TRUE);       //开启功能键响应模式
    noecho();                   //当执行getch()函数的时候关闭键盘回显
    printw("Type any character to see it in bold\n");
    refresh();
    ch = getch();               //如果没有调用raw()函数,我们必须按下enter键才可以键字符传递给程序
    if(ch == KEY_F(1))
        printw("F1 Key prssed");//如果没有使用noecho()函数,一些控制字符将会被打印到屏幕上
    else
    {
        printw("The pressed key is :");
        attron(A_BOLD);
        printw("%c", ch);
        attroff(A_BOLD);
    }
    refresh();
    getch();
    endwin();

    return 0;
}
```

## 输出函数

> 在curses函数中有三类输出函数
* `addch()`系列: 将单一的字符打印到屏幕上, 可以附加字符修饰参数的一类函数.
* `printw()`系列: 和`printf()`一样的具有格式化输出的一类函数.
* `addstr()`系列: 打印字符串的一类函数.

### `addch()`系列函数

* `addch()`函数用于在当前光标位置输入单个字符,并将光标右移一位.你可以使用这个函数输出一个字符,并给其添加修饰.如果一个字符关联有修饰效果(比如:粗体,反色等等),那么当curses输出这个字符的同时就会应用相关的修饰.

> 给单个字符关联属性有两种方法:
* 使用修饰是宏修饰.这些修饰宏可以在ncurses.h中找到.比如,你想输出一个具有加粗(BOLD)和家下划线(UNDERLINE)效果的字符变量ch,可以使用`addch(ch|A_BOLD|A_UNDERLINE);`
* 使用`addrset()`,`attron()`,`attroff()`修饰函数.它们将当前的修饰关联于给定的窗口.一旦十二只完成,则在相应窗口中输出的字符都会被修饰,直到关闭窗口.

### `mvaddch()`,`waddch()`,和`mvwaddch()`函数

* `mvaddch()`用于将光标移动到指定位置输出字符,`mvaddch(row, col, ch);`
* `waddch()`函数是将字符输出到指定窗口的指定坐标处.
* `mvwaddch()`函数是把光标移动到指定窗口中的指定位置处输出字符.

### `printw()`系列函数

* 这些函数的用法和我们熟悉的`printf()`函数相似,但增加了可以在屏幕任意位置输出的功能.

### `printw()`函数和`mvprintw()`函数

* `mvprintw()`函数将光标移动到指定的位置,然后打印内容.

### `vwprintw()`函数

* 这个函数和`vprintf()`相似,用于打印变量表中所对应的内容.

### `addstr()`系列函数

* `addstr()`函数用于在指定窗口输出字符串,如同连续使用`addch()`函数来输出指定字符串中的每个字符.`addstr()`系列函数还包括`mvaddstr()`,`waddstr()`和`mvwaddstr()`函数,这个函数集中还有一个特殊函数`addnstr()`,他需要一个整型参数n,用来打印字符串中的前n个字符.如果这个参数是负数,`addnstr()`将会打印整个字符串.

## 输入函数

> 输入函数被分为三种:
* `getch()`系列:读取一个字符的一类函数
* `scanw()`系列:按照格式化读取输入的一类函数
* `getstr()`系列:读取字符串的一类函数

### `getch()`系列函数

* 这个函数用于从键盘上读入一个字符

### `scanw()`系列函数

* 这些函数用法大体上和`scanf()`函数相似,只不过加入了能够在屏幕的任意位置读入格式化字符串的功能.这个系列还包括`mvscanw()`,`wscanw()`,`mvwscanw()`,`vwscanw()`.

### `getstr()`系列函数

* 这些函数用于从终端读取字符串.本质上,这个函数执行的任务和连续用`getch()`函数读取字符的功能相同,在遇到回车符,新行符和文末符时将用户指针指向该字符串.

## 输出修饰

* `attron()`函数和`attroff()`函数分别用来开启或关闭输出修饰,以下是已经定义的修饰属性

| 修饰属性 | 含义 |
| :---:|:---:|
| A_NORMAL | 普通字符输出 |
| A_STANDOUT | 终端字符最亮 |
| A_UNDERLINE | 下划线 |
| A_REVERSE | 字符反白显示 |
| A_BLINK | 闪动显示 |
| A_DIM | 半亮显示 |
| A_BOLD | 加亮加粗 |
| A_PROTECT | 保护模式 |
| A_INVIS | 空白显示模式 |
| A_ALTCHARSET | 字符交替 |
| A_CHARTEXT | 字符掩盖 |
| COLOR_PAIR(n) | 前景,背景色设置 |

* 我们可以个给一段输出同时设置多种修饰.这样可以得到多种结合的效果.`attron(A_REVERSE|A_BLINK)`

## 窗口机制

* 一个窗口是通过调用`newwin()`函数建立的,但当你调用这个函数后屏幕上并不会有任何变化.因为这个函数的实际作用是给用来操作窗口的结构体分配内存,这个结构体包含了窗口的大小,起始坐标等信息.可见,在curses里,窗口是一个假想的概念,像`wprintw()`等函数都需要以窗口指针作为参数.`delwin()`函数可以删除一个窗口,并释放用来存储窗口结构的内存和信息.`box()`函数可以在已定义的窗口外围画上边框.

```c
#include<ncurses.h>

WINDOW *creat_win(int height, int width, int starty, int startx);
void destroy_win(WINDOW *win);

int main()
{
    WINDOW *mywin;
    int height = 3, width = 10, startx = 0, starty = 0;
    int ch;
    initscr();
    cbreak();
    keypad(stdscr, TRUE);
    refresh();
    mywin = creat_win(height, width, startx, starty);

    while((ch=getch())!=KEY_HOME)
    {
        switch(ch)
        {
            case KEY_LEFT:
                destroy_win(mywin);
                mywin = creat_win(height, width, starty, --startx);
                break;
            case KEY_RIGHT:
                destroy_win(mywin);
                mywin = creat_win(height, width, starty, ++startx);
                break;
            case KEY_UP:
                destroy_win(mywin);
                mywin = creat_win(height, width, --starty, startx);
                break;
            case KEY_DOWN:
                destroy_win(mywin);
                mywin = creat_win(height, width, ++starty, startx);
                break;
        }
    }
    endwin();
    return 0;
}

WINDOW *creat_win(int height, int width, int starty, int startx)
{
    WINDOW *localwin;

    localwin = newwin(height, width, starty, startx);
    box(localwin, 0, 0);
    wrefresh(localwin);

    return localwin;
}

void destroy_win(WINDOW *win)
{
    //box(win,' ',' ');不会按照预期的那样清除窗口边框.而是在窗口的四个角落留下残余字符
    wborder(win, ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ');
    wrefresh(win);
    delwin(win);
}
```

