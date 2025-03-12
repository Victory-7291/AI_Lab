# 标识与感知 --实验2

    
## 1.实现LED1闪烁

### 创建项目

  1.在电脑磁盘中创建`LED`文件夹  
  
  2.在IAR Embedding Worchbench中点击`Project`->`Create New Project`，选择以创建的`LED`文件夹  
  
  3.在IAR Embedding Worchbench中点击`File`->`New`->`File`，创建`main.c` `LED.c` `LED.h`  
  
  4.在IAR Embedding Worchbench中右击`LED - Debug`->`Add`， 把创建的三个文件加到项目的workspace中  
  
  5.在IAR Embedding Worchbench中右击`LED - Debug`->`Options`->`Debug`， Driver选择Texas instruments, 选中Overide default, 点击ok  


![erro](https://github.com/Victory-7291/AI_Lab/raw/main/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-03-12%20202237.png "2024-11-20%2021-42-15.png")  

-------------------------------------------------------------------------------------------------------------------

### 编写代码

复制以下代码到文件中  

**main.c :**
```c
#include "LED.h"

void main(void)
{
  Led_Init();
  while(1)
  {
    LED1_Off();
    Delay(10);
    LED1_On();
    Delay(10);
  }
}
```

**LED.c :**
```c
#include <ioCC2530.h>
#include "LED.h"

void Led_Init(void)
{
  P1SEL &= ~(1<<0);
  P1DIR |=  (1<<0);
  LED1 = 0;
}

void Delay(unsigned int time)
{
  unsigned int i,j;
  for(i = 0; i<time; i++)
    for(j = 0; j < 10000; j++);
}
```

**LED.h :**
```c
#include <ioCC2530.h>         

#define LED1 P1_0             
#define LED1_On() LED1=1;
#define LED1_Off() LED1=0;

extern void Led_Init(void);
extern void Delay(unsigned int time);
```

1.LED.h：定义了LED控制的宏和函数声明。  
2.LED.c：实现了LED初始化、开关、切换和闪烁功能。  
3.main.c：主程序，控制LED1、LED2、LED3的闪烁。  

### 编译并烧录

![erro](https://github.com/Victory-7291/AI_Lab/raw/main/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-03-12%20212532.png "2024-11-20%2021-42-15.png")

-------------------------------------------------------------------------------------------------------------------

![erro](https://github.com/Victory-7291/AI_Lab/raw/main/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-03-12%20212544.png "2024-11-20%2021-42-15.png")

-------------------------------------------------------------------------------------------------------------------

**按下设备上的RESET按键可以看到LED1闪烁**

  
### 讲解

在终端输入
``shell
tree D:\标识与感知\LED /f
```
可以看到这个嵌入式开发项目的目录结构

```shell
D:\标识与感知\LED
│  LED.c                #实现了LED初始化、开关、切换和闪烁功能
│  LED.dep              #依赖文件，用于记录项目中各个文件之间的依赖关系，便于编译时快速确定哪些文件需要重新编译
│  LED.ewd              #工程工作区文件，存储了工程的调试配置信息
│  LED.ewp              #工程文件，定义了项目的配置、源文件列表、编译选项等
|  LED.eww              #工程工作区文件，用于存储工程的全局配置信息
│  LED.h                #定义了LED控制的宏和函数声明  
│  main.c               #主程序，控制LED1、LED2、LED3的闪烁
│
├─Debug             #这个目录用于存放编译和调试过程中生成的文件
│  ├─Exe
│  │      LED.d51           #编译生成的可执行文件（目标文件），用于烧录到微控制器中
│  │
│  ├─List             #存放编译过程中生成的列表文件（如汇编代码、符号表等）
│  └─Obj              #存放编译过程中生成的对象文件（中间文件）
│          LED.pbd                 #程序调试信息文件，用于调试时查看变量、断点等信息
│          LED.pbi                 #编译生成的对象文件，存储了编译后的代码信息
│          LED.r51                 #编译生成的重定位文件，用于链接过程
│          main.pbi
│          main.r51
│
└─settings                 #这个目录用于存放与工程配置相关的文件，通常包含一些脚本或配置文件，用于支持工程的编译和调试
        LED.cspy.bat       #一个批处理文件，用于启动C-SPY调试器（IAR的调试工具）
        LED.dni            #工程的调试信息文件，存储了调试相关的配置
        LED.wsdt           #工程的工作区设置文件，用于存储工作区的配置信息
```


## 2.实现LED1,LED2,LED3闪烁

### 编写代码

**LED.h :**
```c
// LED.h
// 保护宏定义，防止头文件被重复包含
#ifndef LED_H
#define LED_H

// 包含 CC2530 微控制器的特定头文件，该文件定义了与 CC2530 相关的寄存器地址、位定义等
#include <ioCC2530.h>

// 定义宏，将 LED1、LED2、LED3 分别映射到 P1 端口的第 0、1、2 位
#define LED1 P1_0
#define LED2 P1_1
#define LED3 P1_2

// 函数声明，初始化 LED 相关的硬件
// 该函数将配置 LED 引脚为输出模式，并关闭所有 LED
void LED_Init(void);

// 函数声明，打开指定的 LED
// 参数 led 指定要打开的 LED（1、2 或 3）
void LED_On(unsigned char led);

// 函数声明，关闭指定的 LED
// 参数 led 指定要关闭的 LED（1、2 或 3）
void LED_Off(unsigned char led);

// 函数声明，切换指定 LED 的状态（开/关）
// 参数 led 指定要切换状态的 LED（1、2 或 3）
void LED_Toggle(unsigned char led);

// 函数声明，创建一个延时
// 参数 time 指定延时的长度（单位未指定，通常与具体的实现有关）
void Delay(unsigned int time);

#endif // LED_H
```

**LED.c :**
```c
// LED.c
// 包含LED头文件，该文件中包含了LED相关函数的声明和宏定义
#include "LED.h"

// LED初始化函数
// 配置P1端口的前三位（P1_0, P1_1, P1_2）为输出模式，用于控制LED
// 同时确保这些引脚没有被用于其他功能（例如UART通信）
void LED_Init(void) {
    P1DIR |= 0x07;  // 通过位或操作，设置P1_0, P1_1, P1_2为输出
    P1SEL &= ~0x07; // 通过位与操作和取反，确保这些引脚没有被配置为其他功能

    // 初始化时关闭所有LED
    LED_Off(1);
    LED_Off(2);
    LED_Off(3);
}

// 打开指定的LED
// 根据传入的参数led（1、2或3），设置对应的引脚为高电平，点亮LED
void LED_On(unsigned char led) {
    switch (led) {
        case 1: LED1 = 1; break; // 如果led为1，点亮LED1
        case 2: LED2 = 1; break; // 如果led为2，点亮LED2
        case 3: LED3 = 1; break; // 如果led为3，点亮LED3
    }
}

// 关闭指定的LED
// 根据传入的参数led（1、2或3），设置对应的引脚为低电平，关闭LED
void LED_Off(unsigned char led) {
    switch (led) {
        case 1: LED1 = 0; break; // 如果led为1，关闭LED1
        case 2: LED2 = 0; break; // 如果led为2，关闭LED2
        case 3: LED3 = 0; break; // 如果led为3，关闭LED3
    }
}

// 切换指定LED的状态
// 根据传入的参数led（1、2或3），切换对应的引脚电平，实现LED的开关切换
void LED_Toggle(unsigned char led) {
    switch (led) {
        case 1: LED1 = ~LED1; break; // 如果led为1，切换LED1的状态
        case 2: LED2 = ~LED2; break; // 如果led为2，切换LED2的状态
        case 3: LED3 = ~LED3; break; // 如果led为3，切换LED3的状态
    }
}

// 实现一个简单的延时函数
// 参数time表示延时的时间，具体延时长度取决于循环的执行时间
// 这里使用两层嵌套循环来实现延时
void Delay(unsigned int time) {
    unsigned int i, j;
    for (i = 0; i < time; i++) // 外层循环控制延时的总时间
        for (j = 0; j < 120; j++); // 内层循环提供一个固定的时间延迟
}
```

**main.c :**
```c
// main.c
// 包含LED头文件，该文件中包含了LED相关函数的声明和宏定义
#include "LED.h"

// 主函数，程序的入口点
void main(void) {
    // 定义延时时间变量，单位为毫秒（ms）
    unsigned int delayTime = 500;  

    // 调用LED初始化函数，配置LED相关的硬件引脚
    LED_Init();  

    // 无限循环，持续执行LED闪烁操作
    while (1) {
        // 切换LED1的状态（如果LED1是关闭的，则打开它，反之亦然）
        LED_Toggle(1);  
        // 调用Delay函数，延时指定的时间
        Delay(delayTime);
        
        // 切换LED2的状态
        LED_Toggle(2);  
        // 再次延时
        Delay(delayTime);
        
        // 切换LED3的状态
        LED_Toggle(3);  
        // 最后延时
        Delay(delayTime);
    }
}
```
### 编译并烧录

![erro](https://github.com/Victory-7291/AI_Lab/raw/main/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-03-12%20212532.png "2024-11-20%2021-42-15.png")

-------------------------------------------------------------------------------------------------------------------

![erro](https://github.com/Victory-7291/AI_Lab/raw/main/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-03-12%20212544.png "2024-11-20%2021-42-15.png")

-------------------------------------------------------------------------------------------------------------------
**按下设备上的RESET按键可以看到LED1,LED2,LED3闪烁**
