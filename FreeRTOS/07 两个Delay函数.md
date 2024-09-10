在实际开发过程中，编写的任务函数，一般来说建议使用事件来驱动，比如说按下按键才做某些事情，没有按下按键的话它就阻塞，并且在延时的时候不要使用`mydelay()`这种死循环，而是使用freeRTOS提供的Delay函数，这个函数会让当前任务放弃调度，不参与调度，让其他任务有更多的运行机会

有两个Delay函数：
- vTaskDelay：至少等待指定个数的Tick Interrupt才能变为就绪状态
- vTaskDelayUntil：等待到指定的绝对时刻，才能变为就绪态。

这2个函数原型如下：
```c
void vTaskDelay( const TickType_t xTicksToDelay ); /* xTicksToDelay: 等待多少给Tick */

/* pxPreviousWakeTime: 上一次被唤醒的时间
 * xTimeIncrement: 要阻塞到(pxPreviousWakeTime + xTimeIncrement)
 * 单位都是Tick Count
 */
BaseType_t xTaskDelayUntil( TickType_t * const                                        pxPreviousWakeTime,
                            const TickType_t                                          xTimeIncrement );
```

下面画图说明：
- 使用vTaskDelay(n)时，进入、退出vTaskDelay的时间间隔至少是n个Tick中断
- 使用xTaskDelayUntil(&Pre, n)时，前后两次退出xTaskDelayUntil的时间至少是n个Tick中断
    - 退出xTaskDelayUntil时任务就进入的就绪状态，一般都能得到执行机会
    - 所以可以使用xTaskDelayUntil来让任务周期性地运行
![[Pasted image 20240910171745.png]]