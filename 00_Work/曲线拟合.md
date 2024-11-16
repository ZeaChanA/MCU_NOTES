## 准备工作
- **上升下降** 过程中的 **M2补偿值** 全部变成 **-1000**
	- ![[Pasted image 20241110153209.png]]
	- ![[Pasted image 20241110153237.png]]
- **carrier.c**中的 **angle_get()** 函数中的 **最后一行** 注释掉，解开函数中最后一个 **if...else...** 如图：
	- ![[Pasted image 20241110153453.png]]
- 将表格中的 **上升幕布匹配转速** 和 **下降幕布匹配转速** 填入 **SVPWM_U123_tlb.h** 中的**上升曲线数组 const unsigned int Speed_plus[361] = {}** 和**下降曲线数组 const unsigned int Speed_plus_down[361] = {}**
	- ![[Pasted image 20241110154321.png]]
	- ![[Pasted image 20241110154338.png]]
	- ![[Pasted image 20241110154403.png]]