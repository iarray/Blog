## 创建任务
```c
portBASE_TYPE xTaskCreate( pdTASK_CODE pvTaskCode,
	const signed portCHAR * const pcName,
        unsigned portSHORT usStackDepth,
        void *pvParameters,
        unsigned portBASE_TYPE uxPriority,
        xTaskHandle *pxCreatedTask );
```

参数名|描述
-|-
pvTaskCode|参数pvTaskCode 是一个指向任务的实现函数的指针(任务是永不退出的C函数，实现常通常是一个死循环。)
pcName|具有描述性的任务名。这个参数不会被 FreeRTOS 使用。其只是单 纯地用于辅助调试。识别一个具有可读性的名字总是比通过句柄来 识别容易得多。
usStackDepth|当任务创建时，内核会分为每个任务分配属于任务自己的唯一状态。 usStackDepth 值用于告诉内核为它分配多大的栈空间。<br>这个值指定的是栈空间可以保存多少个字(word)，而不是多少个字节(byte)。比如说，如果是32位宽的栈空间，传入的 usStackDepth值为100，则将会分配400字节的栈空间(100 * 4bytes)。栈深度乘以栈宽度的结果千万不能超过一个 size_t 类型变量所能表达的最大值。
pvParameters|pvParameters 的值即是传递到任务中的值(void指针(void*))
uxPriority|指定任务执行的优先级。优先级的取值范围可以从最低优先级 0 到 最高优先级(configMAX_PRIORITIES – 1)。<br>configMAX_PRIORITIES 是一个由用户定义的常量。优先级号并没有上限(除了受限于采用的数据类型和系统的有效内存空间)，但最好使用实际需要的最小数值以避免内存浪费。如果 uxPriority 的值超过了(configMAX_PRIORITIES – 1)，将会导致实际赋给任务的优先级被自动封顶到最大合法值。
