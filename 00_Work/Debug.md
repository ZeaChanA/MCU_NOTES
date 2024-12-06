## 准备工作
- 各个接口接线，还有调试线，电源线（可以看丝印）
	- 调试线接右边4个pin，黑蓝红绿（从左往右）
- 提醒客户先别装支撑臂和幕布的连接处
- main.c  170行  先注释掉M3_control
- 电机极对数都是2
- 编译下载，进入debug，reset -> run
- 看error是否报错，如果error为0x0200
	- init.c   -> 2433行   SPI_DMA_INIT_16BIT();  ->f12   -> 2242行   SPI_SetPinConfigMode(SPI0,true);取消注释
- 按顺序调试M0-2 ( **user** )，先设置Duty（最大值：3332）然后设置电机status（0->关闭，1->开启）
	-  **快要到顶前要将线束理好，防止幕布上升时夹到线束**
	- 方向direction（ **ram_con.h** ）（）
		- 支撑臂M0，M1：1向上，2向下
		- 卷筒M2：1向下，2向上
	- 电机转速，电流
		- 支撑臂
			- 下降转速5000左右，电流0.2A左右
			- 上升转速4800左右，电流0.2~0.3A
		- 卷筒
			- 转速4500~4700，电流0.3~0.4A
- **电机无法转动**
	- 补偿角angle_fix清零
	- 调整CWBUF / CCWBUF，轮流试值（60，180，300，420，540，660）
		- 确定无明显噪声且电流偏低 **？？？**
		- 调整补偿角 angle_fix
			- 观察电流和转速
				- 不正常再调整 angle_fix 正负值（-180 ~ +180）



## 开始标定
- **标定前确认好电机方向CW，CCW**
- **system_mode** 输入 **0X7F** 进入标定
- **reset**
- ( **calibration.c** )
	- 到零点之后会堵转，电流变大
	- 334，348行：支撑臂电流值     （电机该停的时候不停机，电流很大，修改电流值的判断值）
	- 404行：卷筒电流值

- 卡在task2，可以观察M0，M1的电流值，如果太小则跳不出来，可以将判断条件的电流值改小，如图
	- ![[Pasted image 20241111182435.png]]

## 标定结束
- 标定完之后M0~M2的磁角度都应 **< 100**
	- 如果M2的磁角度 **> 100** 可以 reset （以防万一可以先 reset）

- 调试完记录好相关的数据（Direction，angle）


## 电机运行
- 到底部M2不停机，调整电流值
	- ![[Pasted image 20241107215916.png]]
- 上升下降过程中，幕布跟不上
	- 把支撑臂电机转速改成2000，观察现象
		- ![[Pasted image 20241108224822.png]]
		- ![[Pasted image 20241108224841.png]]
	- 如果幕布在上升阶段后期还是跟不上，就改幕布的转速的补偿值，加大补偿值（即增加转速）
		- ![[Pasted image 20241108225415.png]]
	- 如果幕布还是跟不上，并且duty值已经最大，就说明支撑臂电机转速还是太快了，改小，参考前两张图的改法


## 注意问题
- 接电源线之后是否要调节电压，电流（12V，5A）
- 上电电流不对的话，可能是磁编线接反了，磁编线线序如图
	- ![[2024.11.11_17.35.49.jpg]]

- 如果电机极限转速是4000转左右，就把支撑臂转速设置为2000转
- 如果极限转速是5000，就支撑臂的设置到3000转





---
CW：1 向上
CCW：2 向下
- M0
	- CW：60~660     -50     0.3以内2.8 2.9   4500
	- CCW：420 540 660 60 180 300    40     0.12~0.13   4800
- M1
	- V0.1
		- CW：420 540 660 60 180 300   -10   0.27左右  4500
		- CCW：180 300 420 540 660 60   -30    0.31左右   4500
	- V0.2
		- CW：420 540 660 60 180 300     -40    0.25左右    4300
		- CCW：60~660     50     0.3左右     4900
		
- M2
	- V0.1
		- CW：60~660   -40    0.21~0.26    4200
		- CCW：420 540 660 60 180 300   40     0.28~0.3    4200
	- V0.2
		- CW：660 60 180 300 420 540    90    0.28左右    4550
		- CCW：420 540 660 60 180 300     40   0.35左右    4500

--- 

## nuvoTon
- **用小白盒烧录（加密）**
- 第一版软件（n套）
- 支撑臂撑直的电流值，硬件过流，电流保护点，灵敏一点的（n套）
- 每次按下按键能够清除错误，重新运行



